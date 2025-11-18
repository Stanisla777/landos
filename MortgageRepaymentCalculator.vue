npm install --save-dev sass-loader@8.0.2
npm install --save-dev style-loader@1.3.0 --legacy-peer-deps
npm install --save-dev postcss@7 --legacy-peer-deps
npm install --save-dev css-loader@4.3.0 --legacy-peer-deps


const path = require('path');
const fs = require('fs');
const webpack = require('webpack'); // ← добавлено для HMR
const { CleanWebpackPlugin } = require('clean-webpack-plugin');
const CopyWebpackPlugin = require('copy-webpack-plugin');
const HtmlWebpackPlugin = require('html-webpack-plugin');
const MiniCssExtractPlugin = require('mini-css-extract-plugin');
const TerserPlugin = require('terser-webpack-plugin');
const VueLoaderPlugin = require('vue-loader/lib/plugin');
const cssnano = require('cssnano'); // ← переменная используется

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
      chunks: ['main', 'styles']
    });
  });
}

module.exports = (env, argv) => {
  const isProduction = argv.mode === 'production';

  const config = {
    entry: {
      main: './src/js/index.js',
      styles: './src/scss/style.scss'
    },
    output: {
      filename: isProduction ? './js/bundle.js' : './js/[name].bundle.js',
      publicPath: '/dist/'
    },
    devtool: isProduction ? 'source-map' : 'eval-cheap-module-source-map',
    mode: argv.mode || 'development',
    optimization: {
      minimize: isProduction,
      minimizer: isProduction ? [
        new TerserPlugin({ parallel: true, cache: true })
      ] : []
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
      open: true
    },
    module: {
      rules: [
        { test: /\.vue$/, loader: 'vue-loader' },
        {
          test: /\.(sass|scss)$/i,
          include: path.resolve(__dirname, 'src/scss'),
          use: [
            // 'cache-loader',
            isProduction ? MiniCssExtractPlugin.loader : 'style-loader',
            { loader: 'css-loader', options: { sourceMap: !isProduction, url: false } },
            // ↓↓↓ ВРЕМЕННО: отключён в dev для ускорения и избежания ошибок ↓↓↓
            ...(isProduction
              ? [
                  {
                    loader: 'postcss-loader',
                    options: {
                      ident: 'postcss',
                      sourceMap: false,
                      plugins: () => [
                        cssnano({
                          preset: ['default', { discardComments: { removeAll: true } }]
                        })
                      ]
                    }
                  }
                ]
              : []
            ),
            // ↑↑↑ в prod — остаётся, в dev — пропускается ↑↑↑
            {
              loader: 'sass-loader',
              options: {
                sourceMap: !isProduction,
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
            { include: path.resolve(__dirname, 'src/pug/'), exclude: /\.vue$/, use: ['pug-loader'] },
            { use: ['pug-plain-loader'] }
          ]
        },
        {
          enforce: 'pre',
          test: /\.js$/,
          exclude: /node_modules/,
          loader: 'eslint-loader',
          options: { cache: true }
        },
        {
          test: /\.js$/,
          exclude: /node_modules/,
          use: [
            'cache-loader',
            { loader: 'babel-loader', options: { cacheDirectory: true } },
            'source-map-loader'
          ]
        }
      ]
    },
    plugins: [
      new VueLoaderPlugin(),
      new MiniCssExtractPlugin({
        filename: isProduction ? './css/all.css' : '[name].css'
      }),
      new CopyWebpackPlugin([
        { from: './src/fonts', to: './fonts' },
        { from: './src/img', to: './img' }
      ])
    ],
    resolve: {
      alias: {
        vue: isProduction ? 'vue/dist/vue.min.js' : 'vue/dist/vue.js'
      }
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
