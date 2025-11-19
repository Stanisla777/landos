npm install --save-dev sass-loader@8.0.2
npm install --save-dev style-loader@1.3.0 --legacy-peer-deps
npm install --save-dev postcss@7 --legacy-peer-deps
npm install --save-dev css-loader@4.3.0 --legacy-peer-deps
npm install --save-dev cross-env


2092 modules
i ｢wdm｣: Compiled successfully.

----------------------------------------------------------

[WDS] Hot Module Replacement enabled.
index.js?http://localhost:8080:52 [WDS] Live Reloading enabled.


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
      chunks: ['main']
    });
  });
}

module.exports = (env, argv) => {
  const isProduction = argv.mode === 'production';
  const isDevelopment = !isProduction;

  const config = {
    entry: {
      main: './src/js/index.js',
    },
    output: {
      filename: isProduction ? './js/[name].[contenthash:8].js' : './js/[name].js',
      publicPath: '/dist/',
    },
    devtool: isProduction ? 'source-map' : 'eval-cheap-module-source-map',
    mode: argv.mode || 'development',

    // ВКЛЮЧАЕМ КЭШ ДАЖЕ В PRODUCTION ДЛЯ ПОВТОРНЫХ СБОРОК
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
      // ОТКЛЮЧАЕМ SPLIT CHUNKS В DEVELOPMENT ДЛЯ СКОРОСТИ
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
      inline: true,
      compress: true,
      port: 8080,
      stats: 'minimal',
      open: true,
      // КРИТИЧНО ДЛЯ СКОРОСТИ HMR
      watchOptions: {
        ignored: /node_modules/,
        aggregateTimeout: 200, // Уменьшаем задержку
        poll: 1000,
      },
      // УСКОРЯЕМ DEV SERVER
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
          // ИСПОЛЬЗУЕМ УПРОЩЕННУЮ КОНФИГУРАЦИЮ ДЛЯ СКОРОСТИ
          use: [
            // В DEVELOPMENT - ТОЛЬКО style-loader ДЛЯ МГНОВЕННОГО HMR
            isDevelopment ? 'style-loader' : {
              loader: MiniCssExtractPlugin.loader,
              options: { publicPath: '../' }
            },
            {
              loader: 'css-loader',
              options: {
                sourceMap: !isProduction,
                url: false,
                // УБИРАЕМ importLoaders ДЛЯ СКОРОСТИ
              }
            },
            // В DEVELOPMENT - БЕЗ postcss-loader ДЛЯ СКОРОСТИ
            ...(isProduction ? [
              {
                loader: 'postcss-loader',
                options: {
                  sourceMap: !isProduction,
                  postcssOptions: {
                    plugins: [
                      // eslint-disable-next-line global-require
                      require('autoprefixer')(),
                      // eslint-disable-next-line global-require
                      require('cssnano')({
                        preset: ['default', {
                          discardComments: { removeAll: true },
                        }]
                      })
                    ]
                  }
                }
              }
            ] : []),
            {
              loader: 'sass-loader',
              options: {
                sourceMap: !isProduction,
                // eslint-disable-next-line global-require
                implementation: require('sass'),
                sassOptions: {
                  quietDeps: true,
                  silenceDeprecations: ['slash-div', 'import', 'legacy-js-api'],
                  // ВКЛЮЧАЕМ КЭШ ДЛЯ SASS
                  cache: true,
                }
              }
            }
          ]
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
        {
          enforce: 'pre',
          test: /\.js$/,
          exclude: /node_modules/,
          loader: 'eslint-loader',
          options: {
            cache: true,
            cacheIdentifier: 'eslint-cache'
          }
        },
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

      // MiniCssExtractPlugin ТОЛЬКО ДЛЯ PRODUCTION
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
        'process.env.NODE_ENV': JSON.stringify(argv.mode || 'development')
      }),

      // ДОБАВЛЯЕМ HOT MODULE REPLACEMENT ДЛЯ DEVELOPMENT
      ...(isDevelopment ? [
        new webpack.HotModuleReplacementPlugin(),
      ] : []),
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
