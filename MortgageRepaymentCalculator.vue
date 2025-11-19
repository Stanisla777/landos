npm install --save-dev sass-loader@8.0.2
npm install --save-dev style-loader@1.3.0 --legacy-peer-deps
npm install --save-dev postcss@7 --legacy-peer-deps
npm install --save-dev css-loader@4.3.0 --legacy-peer-deps
npm install --save-dev cross-env


2092 modules
i ｢wdm｣: Compiled successfully.

----------------------------------------------------------

// src/js/style-hmr.js
import '../scss/style.scss';

if (module.hot) {
  module.hot.accept('../scss/style.scss', () => {
    console.log('[HMR] ✅ style.scss updated — no JS rebuild!');
  });
}


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
  return templateFiles.map((item) => {
    const parts = item.split('.');
    const name = parts[0];
    const extension = parts[1];
    return new HtmlWebpackPlugin({
      filename: `${name}.html`,
      template: path.resolve(__dirname, `${templateDir}/${name}.${extension}`),
      inject: true,
      chunks: ['main', 'styles'] // ← добавлен чанк 'styles'
    });
  });
}

module.exports = (env, argv) => {
  // 🔍 НАДЁЖНОЕ определение режима
  const mode = argv.mode || 'development';
  const isProduction = mode === 'production';
  const isDevelopment = mode === 'development';

  // 🔍 ЛОГ РЕЖИМА
  console.log('\n🔧 Webpack mode:', mode);
  console.log('🚀 isDevelopment:', isDevelopment);
  console.log('📦 isProduction:', isProduction);
  console.log('');

  const config = {
    entry: {
      main: './src/js/index.js',
      styles: './src/js/style-hmr.js', // ← отдельный entry для стилей
    },
    output: {
      filename: isProduction ? './js/[name].[contenthash:8].js' : './js/[name].js',
      publicPath: '/dist/',
    },
    devtool: isProduction ? 'source-map' : 'eval-cheap-module-source-map',
    mode,

    // ВКЛЮЧАЕМ КЭШ
    cache: {
      type: 'filesystem',
      buildDependencies: {
        config: [__filename]
      }
    },

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
      hotOnly: true, // ← отключает Live Reload, если HMR сломан
      inline: true,
      compress: true,
      port: 8080,
      stats: 'minimal',
      open: true,
      watchOptions: {
        ignored: /node_modules/,
        aggregateTimeout: 50, // ← уменьшено с 200 до 50
        // poll: 1000 — УДАЛЕНО (критично для Windows!)
      },
      writeToDisk: false,
      lazy: false,
    },

    module: {
      rules: [
        {
          test: /\.vue$/,
          loader: 'vue-loader',
        },
        {
          test: /\.(sass|scss)$/i,
          use: (() => {
            const loaders = [
              isDevelopment ? 'style-loader' : {
                loader: MiniCssExtractPlugin.loader,
                options: { publicPath: '../' }
              },
              {
                loader: 'css-loader',
                options: {
                  sourceMap: !isProduction,
                  url: false,
                }
              },
              // postcss-loader ТОЛЬКО в production
              ...(isProduction ? [{
                loader: 'postcss-loader',
                options: {
                  sourceMap: !isProduction,
                  postcssOptions: {
                    plugins: [
                      require('autoprefixer')(),
                      require('cssnano')({
                        preset: ['default', {
                          discardComments: { removeAll: true },
                        }]
                      })
                    ]
                  }
                }
              }] : []),
              {
                loader: 'sass-loader',
                options: {
                  sourceMap: !isProduction,
                  implementation: require('sass'),
                  sassOptions: {
                    quietDeps: true,
                    silenceDeprecations: ['slash-div', 'import', 'legacy-js-api'],
                    cache: true,
                  }
                }
              }
            ];

            // 🔍 ЛОГ ЛОАДЕРОВ
            console.log('🎨 SCSS Loaders:');
            loaders.forEach((l, i) => {
              const name = typeof l === 'string'
                ? l
                : (l.loader || l.constructor?.name || '[object Object]');
              console.log(`  ${i + 1}. ${name}`);
            });
            console.log('');

            return loaders;
          })()
        },
        {
          test: /\.pug$/,
          oneOf: [
            {
              include: path.resolve(__dirname, 'src/pug/'),
              exclude: /\.vue$/,
              use: ['pug-loader']
            },
            {
              use: ['pug-plain-loader']
            }
          ]
        },
        // ESLint отключён в dev (временно)
        ...(isProduction ? [{
          enforce: 'pre',
          test: /\.js$/,
          exclude: /node_modules/,
          loader: 'eslint-loader',
          options: {
            cache: true,
            cacheIdentifier: 'eslint-cache'
          }
        }] : []),
        {
          test: /\.js$/,
          exclude: /node_modules/,
          use: [
            {
              loader: 'babel-loader',
              options: {
                cacheDirectory: true,
                cacheCompression: false
              }
            }
          ]
        }
      ]
    },

    plugins: [
      new VueLoaderPlugin(),

      ...(isProduction ? [
        new MiniCssExtractPlugin({
          filename: './css/all.css'
        })
      ] : []),

      new CopyWebpackPlugin([
        { from: './src/fonts', to: './fonts' },
        { from: './src/img', to: './img' }
      ]),

      new webpack.DefinePlugin({
        'process.env.NODE_ENV': JSON.stringify(mode)
      }),

      ...(isDevelopment ? [new webpack.HotModuleReplacementPlugin()] : []),
    ],

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
