npm install --save-dev sass-loader@8.0.2
npm install --save-dev style-loader@1.3.0 --legacy-peer-deps
npm install --save-dev postcss@7 --legacy-peer-deps
npm install --save-dev css-loader@4.3.0 --legacy-peer-deps
npm install --save-dev cross-env
npm install concurrently@8.2.2 --save-dev



start-webapck-template@2.0.0 C:\Users\sshchegolev\PhpstormProjects\sprosi.dom.rf
`-- (empty)


2092 modules
i ｢wdm｣: Compiled successfully.


----------------------------------------------------------

webpack.config.js

const path = require('path');
const fs = require('fs');
const webpack = require('webpack');
const { CleanWebpackPlugin } = require('clean-webpack-plugin');
const CopyWebpackPlugin = require('copy-webpack-plugin');
const HtmlWebpackPlugin = require('html-webpack-plugin');
const MiniCssExtractPlugin = require('mini-css-extract-plugin');
const TerserPlugin = require('terser-webpack-plugin');
const VueLoaderPlugin = require('vue-loader/lib/plugin');

function generateHtmlPlugins(templateDir) {
  const templateFiles = fs.readdirSync(path.resolve(__dirname, templateDir));
  return templateFiles.map(function(item) {
    const parts = item.split('.');
    const name = parts[0];
    const extension = parts[1];
    return new HtmlWebpackPlugin({
      filename: name + '.html',
      template: path.resolve(__dirname, templateDir + '/' + name + '.' + extension),
      inject: true,
      // styles — только если НЕ dev-server
      chunks: ['main', ...(process.argv.some(arg => arg.includes('webpack-dev-server')) ? [] : ['styles'])]
    });
  });
}

module.exports = function(env, argv) {
  const isDevServer = process.argv.some(arg => arg.includes('webpack-dev-server'));
  const mode = argv.mode || 'development';
  const isProduction = mode === 'production';
  const isDevServerMode = !isProduction && isDevServer;

  const config = {
    entry: {
      main: './src/js/index.js',
      // styles — ТОЛЬКО для сборки в файлы (npm run dev / build)
      ...(isDevServerMode ? {} : { styles: './src/scss/style.scss' })
    },
    output: {
      filename: isProduction ? './js/[name].[contenthash:8].js' : './js/[name].js',
      publicPath: '/dist/',
    },
    devtool: isProduction ? 'source-map' : 'eval-cheap-module-source-map',
    mode: mode,

    optimization: {
      minimize: isProduction,
      minimizer: isProduction ? [
        new TerserPlugin({
          parallel: true,
          terserOptions: {
            compress: {
              drop_console: true,
            },
          },
        })
      ] : [],
      splitChunks: isProduction ? {
        chunks: 'all',
        cacheGroups: {
          vendor: {
            test: /[\\/]node_modules[\\/]/,
            name: 'vendors',
            chunks: 'all',
          },
        },
      } : false,
    },

    performance: { hints: false },

    devServer: {
      contentBase: path.resolve(__dirname, 'dist'),
      publicPath: '/dist/',
      hot: true,
      hotOnly: true,
      inline: true,
      compress: true,
      port: 8080,
      stats: 'minimal',
      open: false, // ← отключил автооткрытие (concurrently может дублировать)
      watchOptions: {
        ignored: /node_modules/,
        aggregateTimeout: 50,
      },
      writeToDisk: false,
      lazy: false,
    },

    module: {
      rules: [
        {
          test: /\.vue$/,
          use: [
            'cache-loader',
            'vue-loader'
          ]
        },
        // 🔴 УДАЛЁН весь блок для sass/scss — его обрабатывает webpack.css.js
        {
          test: /\.pug$/,
          oneOf: [
            {
              include: path.resolve(__dirname, 'src/pug/'),
              exclude: /\.vue$/,
              use: [
                'cache-loader',
                'pug-loader'
              ]
            },
            {
              use: ['pug-plain-loader']
            }
          ]
        },
        // ESLint — только в production
        isProduction ? {
          enforce: 'pre',
          test: /\.js$/,
          exclude: /node_modules/,
          loader: 'eslint-loader',
          options: {
            cache: true,
            cacheIdentifier: 'eslint-cache'
          }
        } : null,
        {
          test: /\.js$/,
          exclude: /node_modules/,
          use: [
            'cache-loader',
            {
              loader: 'babel-loader',
              options: {
                cacheDirectory: true,
                cacheCompression: false
              }
            }
          ]
        }
      ].filter(Boolean)
    },

    plugins: [
      new VueLoaderPlugin(),

      // MiniCssExtractPlugin — ТОЛЬКО если НЕ dev-server (для dev/build)
      (!isDevServerMode) ? new MiniCssExtractPlugin({
        filename: './css/all.css'
      }) : null,

      new CopyWebpackPlugin([
        { from: './src/fonts', to: './fonts' },
        { from: './src/img', to: './img' }
      ]),

      new webpack.DefinePlugin({
        'process.env.NODE_ENV': JSON.stringify(isProduction ? 'production' : 'development')
      }),

      // HMR — только для dev-server
      isDevServerMode ? new webpack.HotModuleReplacementPlugin() : null
    ].filter(Boolean),

    resolve: {
      alias: {
        vue: isProduction ? 'vue/dist/vue.min.js' : 'vue/dist/vue.js'
      },
      extensions: ['.js', '.vue', '.json']
    },
  };

  if (isProduction) {
    config.plugins.push(new CleanWebpackPlugin());
  } else {
    config.plugins = config.plugins.concat(generateHtmlPlugins('./src/pug/views'));
  }

  return config;
};



--------------------------------------------------------------------

Создайте webpack.css.js (файл в корне проекта, рядом с webpack.config.js)

// webpack.css.js
const path = require('path');
const MiniCssExtractPlugin = require('mini-css-extract-plugin');

module.exports = {
  mode: 'development',
  entry: './src/scss/style.scss',
  output: {
    path: path.resolve(__dirname, 'dist'),
    filename: 'js/css-dummy.js',
  },
  module: {
    rules: [
      {
        test: /\.(sass|scss)$/i,
        use: [
          MiniCssExtractPlugin.loader,
          'css-loader',
          'cache-loader',
          {
            loader: 'sass-loader',
            options: {
              implementation: require('sass'),
              sassOptions: {
                quietDeps: true,
                silenceDeprecations: ['slash-div', 'import', 'legacy-js-api'],
              }
            }
          }
        ]
      }
    ]
  },
  plugins: [
    new MiniCssExtractPlugin({
      filename: 'css/all.css'
    })
  ],
  watchOptions: {
    ignored: /node_modules/,
    aggregateTimeout: 50
  },
  stats: 'errors-only'
};

----------------------------------------------------------------------

Удалите (или переименуйте) src/js/style-hmr.js

-------------------------------------------------------------------
В Pug-шаблонах подключите CSS вручную в <head>
  head
  meta(charset="utf-8")
  meta(name="viewport" content="width=device-width, initial-scale=1")
  title= title || 'My App'
  // ← ЕДИНСТВЕННОЕ подключение CSS:
  link(rel="stylesheet" href="/dist/css/all.css")

  Установите concurrently
npm install concurrently@8.2.2 --save-dev

------------------------------------------------------------------

  "scripts": {
  "dev:css": "webpack --config webpack.css.js --watch --mode development",
  "dev:js": "webpack-dev-server --mode development --hot",
  "start": "concurrently \"npm run dev:css\" \"npm run dev:js\" --kill-others-on-fail --prefix name",
  "dev": "webpack --mode development && prettier --print-width=120 --parser html --write dist/*.html",
  "build": "webpack --mode production",
  "lint": "eslint --ext .js, --ignore-path .gitignore ."
}
  
