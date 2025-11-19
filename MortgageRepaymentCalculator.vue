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
  return templateFiles.map((item) => {
    const parts = item.split('.');
    const name = parts[0];
    const extension = parts[1];
    return new HtmlWebpackPlugin({
      filename: `${name}.html`,
      template: path.resolve(__dirname, `${templateDir}/${name}.${extension}`),
      inject: true,
      chunks: ['main'] // Убрали styles из chunks - стили инжектятся автоматически
    });
  });
}

module.exports = (env, argv) => {
  const isProduction = argv.mode === 'production';
  const isDevelopment = !isProduction;

  const config = {
    entry: {
      main: './src/js/index.js',
      // Убрали отдельный entry point для стилей - они импортируются через JS
    },
    output: {
      filename: isProduction ? './js/[name].[contenthash:8].js' : './js/[name].js',
      publicPath: '/dist/',
      chunkFilename: isProduction ? './js/[name].[contenthash:8].chunk.js' : './js/[name].chunk.js',
    },
    devtool: isProduction ? 'source-map' : 'eval-cheap-module-source-map',
    mode: argv.mode || 'development',
    cache: {
      type: isDevelopment ? 'filesystem' : false, // Включение кэширования файловой системы
      buildDependencies: {
        config: [__filename] // Инвалидация кэша при изменении конфига
      }
    },
    optimization: {
      minimize: isProduction,
      minimizer: isProduction ? [
        new TerserPlugin({
          parallel: true,
          terserOptions: {
            compress: {
              drop_console: true, // Удаление console.log в production
            },
          },
        })
      ] : [],
      splitChunks: {
        chunks: 'all',
        cacheGroups: {
          vendor: {
            test: /[\\/]node_modules[\\/]/,
            name: 'vendors',
            chunks: 'all',
          },
        },
      },
    },
    performance: { hints: false },
    devServer: isProduction ? undefined : {
      contentBase: path.resolve(__dirname, 'dist'),
      publicPath: '/dist/',
      hot: true,
      inline: true,
      compress: true,
      port: 8080,
      stats: 'minimal',
      open: true,
      watchOptions: {
        ignored: /node_modules/, // Игнорировать node_modules для ускорения
        aggregateTimeout: 300, // Задержка перед пересборкой
      }
    },
    module: {
      rules: [
        {
          test: /\.vue$/,
          loader: 'vue-loader',
        },
        {
          test: /\.(sass|scss)$/i,
          use: [
            isDevelopment ? {
              loader: 'style-loader',
              options: {
                injectType: 'singletonStyleTag' // Все стили в один тег для HMR
              }
            } : {
              loader: MiniCssExtractPlugin.loader,
              options: {
                publicPath: '../' // Корректные пути для изображений/шрифтов
              }
            },
            {
              loader: 'css-loader',
              options: {
                sourceMap: !isProduction,
                url: false,
                importLoaders: 2 // Обеспечивает применение postcss и sass к импортам
              }
            },
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
                          normalizeWhitespace: false
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
                  silenceDeprecations: ['slash-div', 'import', 'legacy-js-api']
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
            cacheIdentifier: 'eslint-cache' // Ключ для кэша
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
                cacheCompression: false // Ускоряет development
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
          filename: './css/[name].[contenthash:8].css',
          chunkFilename: './css/[name].[contenthash:8].chunk.css'
        })
      ] : []),
      new CopyWebpackPlugin([
        { from: './src/fonts', to: './fonts' },
        { from: './src/img', to: './img' }
      ]),
      new webpack.DefinePlugin({
        'process.env.NODE_ENV': JSON.stringify(argv.mode || 'development')
      })
    ],
    resolve: {
      alias: {
        vue: isProduction ? 'vue/dist/vue.min.js' : 'vue/dist/vue.js'
      },
      extensions: ['.js', '.vue', '.json']
    }
  };

  if (isProduction) {
    config.plugins.push(new CleanWebpackPlugin());
  } else {
    config.plugins.push(new webpack.HotModuleReplacementPlugin());
    config.plugins = config.plugins.concat(generateHtmlPlugins('./src/pug/views'));
  }

  return config;
};



