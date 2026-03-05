/* eslint-disable */
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

-----------------------------------------

alias: {
      // Убираем алиас на глобальную сборку, используем модульную
      vue$: 'vue/dist/vue.runtime.esm-bundler.js'
    },

    -------------

    plugins: [
    new rspack.DefinePlugin({
      __VUE_OPTIONS_API__: JSON.stringify(true),
      __VUE_PROD_DEVTOOLS__: JSON.stringify(false),
      __VUE_PROD_HYDRATION_MISMATCH_DETAILS__: JSON.stringify(false),
      'process.env.NODE_ENV': JSON.stringify(process.env.NODE_ENV || 'development')
    }),

    ----------------------

    module.exports = (env, argv) => {
  config.mode = argv.mode || 'development';
  
  if (argv.mode === 'production') {
    config.resolve.alias.vue$ = 'vue/dist/vue.runtime.esm-bundler.js';
    config.optimization.minimize = true;
  }
  
  return config;
};

npm install --save-dev @vue/compiler-sfc @babel/core @babel/preset-env babel-loader


document.addEventListener('DOMContentLoaded', () => {
  function initTelegramBlockIfNeeded() {
    const el = document.getElementById('telegram-block');

    if (!el || el.__vue_initialized) {
      return;
    }

    // Убедимся, что элемент существует и содержит шаблон
    if (!el.querySelector('telegram-block')) {
      return;
    }

    try {
      const app = createApp(TelegramBlock);
      app.mount(el);
      el.__vue_initialized = true;
      console.log('✅ TelegramBlock mounted');
    } catch (err) {
      console.error('❌ Failed to mount TelegramBlock:', err);
    }
  }

  window.initTelegramBlockIfNeeded = initTelegramBlockIfNeeded;
  initTelegramBlockIfNeeded();
});

------------------------------------

<template>
  <article class="telegram" v-if="active">
    <div class="telegram__popup">
      <button class="telegram__popup-close" @click="hideBlock">
        <svg
          xmlns="http://www.w3.org/2000/svg"
          width="24"
          height="24"
          viewBox="0 0 24 24"
          fill="none"
        >
          <path
            fill-rule="evenodd"
            clip-rule="evenodd"
            d="M4 4.00005C4.39053 3.60952 5.02369 3.60952 5.41422 4.00005L11.7783 10.3642L18.1421 4.00034C18.5327 3.60981 19.1658 3.60981 19.5564 4.00034C19.9469 4.39086 19.9469 5.02403 19.5564 5.41455L13.1925 11.7784L19.5564 18.1422C19.9469 18.5327 19.9469 19.1659 19.5564 19.5564C19.1658 19.9469 18.5327 19.9469 18.1421 19.5564L11.7783 13.1926L5.41422 19.5567C5.02369 19.9472 4.39053 19.9472 4 19.5567C3.60948 19.1662 3.60948 18.533 4 18.1425L10.3641 11.7784L4 5.41426C3.60948 5.02374 3.60948 4.39057 4 4.00005Z"
            fill="#252628"
          />
        </svg>
      </button>
      <img :src="qr" class="telegram__popup-qr" alt="">
      <p class="telegram__popup-text" v-html="description"></p>
      <div class="telegram__popup-block">
        <img
          v-if="mobilicon"
          class="telegram__popup-block-icon"
          :src="mobilicon"
          alt=""
        >
        <p
          class="telegram__popup-text telegram__popup-text_mobile"
          v-html="description"
        ></p>
      </div>
      <a
        class="telegram__popup-link-button"
        target="_blank"
        :href="link"
      >
        Подписаться
      </a>
    </div>
  </article>
</template>

<script>
export default {
  name: 'TelegramBlock',
  props: {
    qr: { type: String, default: '' },
    mobilicon: { type: String, default: '' },
    link: { type: String, default: '' },
    description: { type: String, default: '' }
  },
  data() {
    return {
      active: false
    };
  },
  mounted() {
    console.log('🔵 TelegramBlock mounted with props:', this.$props);
    if (!this.getCookie('telegram')) {
      this.active = true;
    }
  },
  methods: {
    hideBlock() {
      this.setCookie('telegram', true, 1);
      this.active = false;
    },
    setCookie(key, value, days) {
      const expires = new Date();
      expires.setTime(expires.getTime() + days * 24 * 60 * 60 * 1000);
      document.cookie = `${key}=${value};path=/;expires=${expires.toUTCString()}`;
    },
    getCookie(key) {
      const keyValue = document.cookie.match(`(^|;) ?${key}=([^;]*)(;|$)`);
      return keyValue ? keyValue[2] : null;
    }
  }
};
</script>
