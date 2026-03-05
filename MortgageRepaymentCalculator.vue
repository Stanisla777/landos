Uncaught ReferenceError: Vue is not defined
    at vue.global.js:16323:1
    at vue.global.js:16323:1
    at vue.global.js:16323:1

Uncaught TypeError: (0 , i.createElementVNode) is not a function
    at ./src/js/modules/TelegramBlock.vue (bundle.js:1:1000)
    at s (vue.global.js:16323:1)
    at vue.global.js:16323:1
    at vue.global.js:16323:1
    at vue.global.js:16323:1




rspack.config.js
/* eslint-disable */
const path = require('path');
const fs = require('fs');
const rspack = require('@rspack/core');

const config = {
  entry: {
    all: ['./src/js/index.js'],
    'general-style': ['./src/scss/general-style.scss'],
    instruction: ['./src/scss/instruction.scss'],
    'general-js': './src/js/general.js',
    'instruction-js': './src/js/instruction.js'
  },

  output: {
    filename: './js/[name].bundle.js',
    publicPath: '/dist/',
    clean: true,
    path: path.resolve(__dirname, 'dist'),
  },

  experiments: {
    css: false
  },

  mode: 'production',

  optimization: {
    minimize: true,
    splitChunks: {
      cacheGroups: {
        defaultVendors: false,
        vendor: {
          test: /[\\/]node_modules[\\/](axios|vue-axios|swiper|imask)[\\/]/,
          name: 'vendor-script',
          chunks: (chunk) => ['general-js', 'instruction-js', 'instruction-lazy'].includes(chunk.name),
          enforce: true
        }
      }
    }
  },

  performance: { hints: false },

  module: {
    rules: [
      {
        test: /\.vue$/,
        use: [{
          loader: path.resolve(__dirname, 'loaders/vue-compiler-loader.js')
        }]
      },
      {
        test: /\.(sass|scss)$/,
        include: path.resolve(__dirname, 'src/scss'),
        use: [
          rspack.CssExtractRspackPlugin.loader,
          { loader: 'css-loader', options: { sourceMap: true, url: false } },
          {
            loader: 'postcss-loader',
            options: {
              sourceMap: true,
              postcssOptions: {
                plugins: [require('cssnano')({ preset: ['default', { discardComments: { removeAll: true } }] })]
              }
            }
          },
          { loader: 'sass-loader', options: { sourceMap: true } }
        ],
        type: 'javascript/auto'
      },
      {
        test: /\.pug$/,
        oneOf: [
          { include: path.resolve(__dirname, 'src/pug/'), exclude: /\.vue$/, use: ['pug-loader'] },
          { use: ['pug-plain-loader'] }
        ]
      },
      { test: /\.js$/, exclude: /node_modules/, use: { loader: 'babel-loader' } },
      { test: /\.js$/, enforce: 'pre', use: ['source-map-loader'] }
    ]
  },

  plugins: [
    // ✅ DefinePlugin с ПРАВИЛЬНЫМИ значениями (JSON.stringify!)
    new rspack.DefinePlugin({
      __VUE_OPTIONS_API__: JSON.stringify(true),
      __VUE_PROD_DEVTOOLS__: JSON.stringify(false),
      __VUE_PROD_HYDRATION_MISMATCH_DETAILS__: JSON.stringify(false),
      'process.env.NODE_ENV': JSON.stringify('development')  // ← Будет перезаписано ниже
    }),

    new rspack.CssExtractRspackPlugin({ filename: './css/[name].css' }),
    new rspack.CopyRspackPlugin({
      patterns: [
        { from: './src/fonts', to: './fonts' },
        { from: './src/img', to: './img' }
      ]
    }),
    {
      apply: (compiler) => {
        compiler.hooks.afterEmit.tap('RenameMainToBundle', () => {
          const fs = require('fs');
          const path = require('path');
          const mainPath = path.resolve(compiler.outputPath, 'js/all.bundle.js');
          const bundlePath = path.resolve(compiler.outputPath, 'js/bundle.js');
          if (fs.existsSync(mainPath)) {
            fs.renameSync(mainPath, bundlePath);
            console.log('✅ all.bundle.js → bundle.js');
          }
        });
      }
    },
    {
      apply: (compiler) => {
        compiler.hooks.afterEmit.tap('CleanupCSSOnlyJS', () => {
          const fs = require('fs');
          const path = require('path');
          ['general-style', 'instruction'].forEach((name) => {
            const jsPath = path.resolve(compiler.outputPath, `js/${name}.bundle.js`);
            if (fs.existsSync(jsPath)) fs.unlinkSync(jsPath);
          });
        });
      }
    }
  ],

  resolve: {
    alias: {
      // ✅ Полная UMD-сборка — работает с любыми импортами
      vue: 'vue/dist/vue.global.js'
    },
    extensions: ['.js', '.vue', '.json']
  },

  devtool: 'source-map',

  devServer: {
    port: 8080,
    open: true,
    hot: true,
    static: { directory: path.join(__dirname, 'dist') },
    historyApiFallback: true,
    watchFiles: ['src/pug/**/*.pug', 'src/**/*'],
  },
};

module.exports = (env, argv) => {
  if (argv.mode === 'production') {
    // ✅ Production: та же сборка, но prod-версия
    config.resolve.alias.vue = 'vue/dist/vue.global.prod.js';
    config.devtool = 'eval-source-map';
    config.optimization.minimize = true;
    // Обновляем NODE_ENV в DefinePlugin
    config.plugins[0] = new rspack.DefinePlugin({
      __VUE_OPTIONS_API__: JSON.stringify(true),
      __VUE_PROD_DEVTOOLS__: JSON.stringify(false),
      __VUE_PROD_HYDRATION_MISMATCH_DETAILS__: JSON.stringify(false),
      'process.env.NODE_ENV': JSON.stringify('production')
    });
  } else {
    config.mode = 'development';
    config.devtool = 'cheap-module-source-map';
  }
  return config;
};
-----------
vue-compiler-loader.js
/* eslint-disable */
// loaders/vue-compiler-loader.js
const { parse, compileTemplate, compileScript } = require('@vue/compiler-sfc');
const path = require('path');

module.exports = function vueCompilerLoader(source) {
  const callback = this.async();

  console.log('🔍 [VUE LOADER] Compiling:', this.resourcePath);

  const { descriptor, errors } = parse(source, { filename: this.resourcePath });

  if (errors && errors.length > 0) {
    callback(new Error(errors[0].message));
    return;
  }

  // ✅ Импорт через 'vue' — алиас разрешит его правильно
  let output = 'import { defineComponent } from "vue";\n';

  // === Script ===
  if (descriptor.script || descriptor.scriptSetup) {
    try {
      const { content } = compileScript(descriptor, {
        id: require('crypto').createHash('md5').update(this.resourcePath).digest('hex').slice(0, 8)
      });
      output += content + '\n';
    } catch (e) {
      callback(new Error(`Script compilation error: ${e.message}`));
      return;
    }
  } else {
    output += 'export default defineComponent({})\n';
  }

  // === Template ===
  if (descriptor.template) {
    try {
      const { code } = compileTemplate({
        source: descriptor.template.content,
        filename: this.resourcePath,
        id: require('crypto').createHash('md5').update(this.resourcePath).digest('hex').slice(0, 8),
        compilerOptions: {
          bindingMeta: descriptor.scriptSetup ? descriptor.scriptSetup.bindings : undefined
        }
      });
      console.log('🔍 [VUE LOADER] Template code preview:', code.substring(0, 500));
      output += code + '\n';
    } catch (e) {
      callback(new Error(`Template compilation error: ${e.message}`));
      return;
    }
  }

  // === Styles ===
  if (descriptor.styles.length > 0) {
    const styles = descriptor.styles.map((style, index) => {
      const scopedId = style.scoped
        ? `data-v-${require('crypto').createHash('md5').update(this.resourcePath + index).digest('hex').slice(0, 8)}`
        : '';
      return `${scopedId ? `\n/* scoped: ${scopedId} */` : ''}\n${style.content}`;
    }).join('\n');
    output += `\nif (typeof __VUE_HMR_RUNTIME__ !== 'undefined') { __VUE_HMR_RUNTIME__.addStyle(${JSON.stringify(styles)}); }\n`;
  }

  callback(null, output);
};

index.js
import {createApp} from 'vue';

document.addEventListener('DOMContentLoaded', () => {
function initTelegramBlockIfNeeded() {
    const el = document.getElementById('telegram-block');

    // Защита 1: уже инициализирован
    if (el && el.__vue_initialized) {
      return;
    }
    // Защита 2: уже содержит отрендеренный контент
    if (el && el.querySelector('article.telegram')) {
      return;
    }
    // Защита 3: нет шаблона <telegram-block>
    if (!el || !el.querySelector('telegram-block')) {
      return;
    }

    // ✅ Vue 3: createApp вместо new Vue
    const app = createApp({
      components: { 'telegram-block': TelegramBlock }
    });
    app.mount('#telegram-block');

    el.__vue_initialized = true;
  }
  window.initTelegramBlockIfNeeded = initTelegramBlockIfNeeded;
  initTelegramBlockIfNeeded();
      })

      TelegramBlock.vue

      <template>
  <article class="telegram" v-if="active">
    <div class="telegram__popup">
      <button class="telegram__popup-close">
        <svg
          xmlns="http://www.w3.org/2000/svg"
          width="24"
          height="24"
          viewBox="0 0 24 24"
          fill="none"
          @click="hideBlock"
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
  // ✅ Vue 3: props объявлены явно
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
    console.log('Приложение')
    if (this.qr) { this.qr = this.qr; }
    if (this.mobilicon) { this.mobilicon = this.mobilicon; }
    if (this.link) { this.link = this.link; }
    if (this.description) { this.description = this.description; }
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

