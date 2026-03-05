vue-compiler-loader.js остаётся таким же?:
const { parse, compileTemplate, compileScript, rewriteDefault } = require('@vue/compiler-sfc');
const crypto = require('crypto');

module.exports = function vueCompilerLoader(source) {
  const callback = this.async();
  const filename = this.resourcePath;
  const id = crypto.createHash('md5').update(filename).digest('hex').slice(0, 8);

  console.log('🔍 [VUE LOADER] Compiling:', filename);

  try {
    const { descriptor, errors } = parse(source, { filename });

    if (errors && errors.length > 0) {
      callback(new Error(errors[0].message));
      return;
    }

    // Компилируем script
    let scriptContent = '';
    if (descriptor.script) {
      scriptContent = rewriteDefault(descriptor.script.content, '__component');
    } else {
      scriptContent = 'const __component = {};\n';
    }

    // Компилируем template
    let templateRender = '';
    if (descriptor.template) {
      const templateResult = compileTemplate({
        source: descriptor.template.content,
        filename,
        id,
        compilerOptions: {
          scopeId: descriptor.styles.some(s => s.scoped) ? `data-v-${id}` : null,
          bindingMetadata: descriptor.script?.bindings
        }
      });

      if (templateResult.errors.length) {
        callback(new Error(templateResult.errors[0].message));
        return;
      }

      templateRender = templateResult.code;
    }

    // Компилируем стили (если нужно)
    let stylesCode = '';
    if (descriptor.styles.length > 0) {
      stylesCode = descriptor.styles.map((style, index) => {
        const styleId = style.scoped ? `data-v-${id}` : '';
        return `
          const styleElement${index} = document.createElement('style');
          styleElement${index}.innerHTML = ${JSON.stringify(style.content)};
          ${styleId ? `styleElement${index}.setAttribute('scoped', '${styleId}');` : ''}
          document.head.appendChild(styleElement${index});
        `;
      }).join('\n');
    }

    // Финальный код
    const output = `
      import { defineComponent, resolveComponent, openBlock, createElementBlock, createVNode, render } from 'vue';

      ${scriptContent}

      ${templateRender ? templateRender.replace('export function render', 'function render') : ''}

      ${stylesCode}

      const __component__ = defineComponent({
        ...__component,
        name: __component.name || 'AnonymousComponent',
        render: ${templateRender ? 'render' : 'undefined'}
      });

      export default __component__;
    `;

    callback(null, output);
  } catch (err) {
    callback(err);
  }
};
