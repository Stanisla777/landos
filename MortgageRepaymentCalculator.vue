npm install --save-dev sass-loader@8.0.2
npm install --save-dev style-loader@1.3.0 --legacy-peer-deps
npm install --save-dev postcss@7 --legacy-peer-deps
npm install --save-dev css-loader@4.3.0 --legacy-peer-deps
npm install --save-dev cross-env


2092 modules
i ｢wdm｣: Compiled successfully.

----------------------------------------------------------

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
      chunks: ['main', 'styles']
    });
  });
}

module.exports = function(env, argv) {
  const mode = argv.mode || 'development';
  const isProduction = mode === 'production';
  const isDevelopment = mode === 'development';

  // 🔍 (Опционально) Раскомментируйте для отладки:
  // console.log('\n🔧 Webpack mode:', mode);
  // console.log('🚀 isDevelopment:', isDevelopment);
  // console.log('📦 isProduction:', isProduction);
  // console.log('');

  const config = {
    entry: {
      main: './src/js/index.js',
      styles: './src/js/style-hmr.js',
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
      open: true,
      watchOptions: {
        ignored: /node_modules/,
        aggregateTimeout: 50,
        // poll: 1000 — УДАЛЕНО
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
        {
          test: /\.(sass|scss)$/i,
          use: [
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
            // postcss-loader только в production
            isProduction ? {
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
            } : null,
            'cache-loader',
            {
              loader: 'sass-loader',
              options: {
                sourceMap: !isProduction,
                implementation: require('sass'),
                sassOptions: {
                  quietDeps: true,
                  silenceDeprecations: ['slash-div', 'import', 'legacy-js-api'],
                  // cache: true — УДАЛЕНО: не работает в sass-loader (только CLI)
                }
              }
            }
          ].filter(Boolean) // удаляет null (когда postcss-loader отключён)
        },
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
        // ESLint только в production
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

      isProduction ? new MiniCssExtractPlugin({
        filename: './css/all.css'
      }) : null,

      new CopyWebpackPlugin([
        { from: './src/fonts', to: './fonts' },
        { from: './src/img', to: './img' }
      ]),

      new webpack.DefinePlugin({
        'process.env.NODE_ENV': JSON.stringify(mode)
      }),

      isDevelopment ? new webpack.HotModuleReplacementPlugin() : null
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
