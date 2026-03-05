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
    let bindings;
    
    if (descriptor.script) {
      const compiledScript = compileScript(descriptor, { id });
      scriptContent = rewriteDefault(compiledScript.content, '__component');
      bindings = compiledScript.bindings;
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
          bindingMetadata: bindings
        }
      });

      if (templateResult.errors.length) {
        callback(new Error(templateResult.errors[0].message));
        return;
      }

      templateRender = templateResult.code.replace('export function render', 'function render');
    }

    // Обрабатываем стили внутри компонента
    let stylesCode = '';
    if (descriptor.styles.length > 0) {
      stylesCode = descriptor.styles.map((style, index) => {
        const styleId = style.scoped ? `data-v-${id}` : '';
        const styleContent = JSON.stringify(style.content);
        
        // Добавляем инлайн стили в head
        return `
          (function() {
            const styleId = 'vue-style-${id}-${index}';
            // Проверяем, не добавлен ли уже этот стиль
            if (!document.getElementById(styleId)) {
              const styleElement = document.createElement('style');
              styleElement.id = styleId;
              styleElement.innerHTML = ${styleContent};
              ${styleId ? `styleElement.setAttribute('scoped', '${styleId}');` : ''}
              styleElement.setAttribute('data-vue-component', '${id}');
              document.head.appendChild(styleElement);
              
              // Для HMR - удаляем старый стиль при обновлении
              if (import.meta.hot) {
                import.meta.hot.dispose(() => {
                  const oldStyle = document.getElementById(styleId);
                  if (oldStyle) oldStyle.remove();
                });
              }
            }
          })();
        `;
      }).join('\n');
    }

    // Определяем, какие импорты из vue нужны
    const vueImports = ['defineComponent'];
    if (templateRender) {
      vueImports.push('resolveComponent', 'openBlock', 'createElementBlock', 'createVNode');
      // Проверяем, используется ли render функция
      if (templateRender.includes('render')) {
        vueImports.push('render');
      }
    }

    // Финальный код
    const output = `
      import { ${vueImports.join(', ')} } from 'vue';

      ${scriptContent}

      ${templateRender || ''}

      ${stylesCode}

      const __component__ = defineComponent({
        ...__component,
        name: __component.name || 'AnonymousComponent',
        ${templateRender ? 'render' : ''}
      });

      export default __component__;
    `;

    callback(null, output);
  } catch (err) {
    console.error('❌ [VUE LOADER] Error:', err);
    callback(err);
  }
};
