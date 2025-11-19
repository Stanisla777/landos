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
      chunkFilename: isProduction ? './js/[name].[contenthash:8].chunk.js' : './js/[name].chunk.js',
    },
    devtool: isProduction ? 'source-map' : 'eval-cheap-module-source-map',
    mode: argv.mode || 'development',
    cache: {
      type: 'filesystem', // Кэш для всех режимов
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
      // Ускоряет сборку в development
      removeAvailableModules: false,
      removeEmptyChunks: false,
      splitChunks: false,
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
        ignored: /node_modules/,
        aggregateTimeout: 300,
      },
      // Ускоряет dev server
      writeToDisk: false,
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
            // Development: быстрый HMR, Production: отдельный файл
            isDevelopment ? {
              loader: 'style-loader',
              options: {
                injectType: 'singletonStyleTag',
                attributes: { 'data-hmr': 'true' }
              }
            } : {
              loader: MiniCssExtractPlugin.loader,
              options: {
                publicPath: '../'
              }
            },
            {
              loader: 'css-loader',
              options: {
                sourceMap: !isProduction,
                url: false,
                importLoaders: 1 // Только sass-loader для development
              }
            },
            ...(isProduction ? [
              {
                loader: 'postcss-loader',
                options: {
                  sourceMap: !isProduction,
                  postcssOptions: {
                    plugins: [
                      require('autoprefixer')(),
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
                implementation: require('sass'),
                sassOptions: {
                  quietDeps: true,
                  silenceDeprecations: ['slash-div', 'import', 'legacy-js-api'],
                  // Ускоряет компиляцию SASS
                  outputStyle: isDevelopment ? 'expanded' : 'compressed'
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
      // MiniCssExtractPlugin только для production
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
      })
    ],
    resolve: {
      alias: {
        vue: isProduction ? 'vue/dist/vue.min.js' : 'vue/dist/vue.js'
      },
      extensions: ['.js', '.vue', '.json']
    },
    // Игнорировать ненужное для ускорения
    watchOptions: {
      ignored: /node_modules/
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
