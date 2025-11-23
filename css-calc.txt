const path = require('path');
const fs = require('fs');
const { CleanWebpackPlugin } = require('clean-webpack-plugin');
const CopyWebpackPlugin = require('copy-webpack-plugin');
const HtmlWebpackPlugin = require('html-webpack-plugin');
const MiniCssExtractPlugin = require('mini-css-extract-plugin');
const TerserPlugin = require('terser-webpack-plugin');
const VueLoaderPlugin = require('vue-loader/lib/plugin');
// eslint-disable-next-line no-unused-vars
const { BundleAnalyzerPlugin } = require('webpack-bundle-analyzer');

function generateHtmlPlugins(templateDir) {
  const templateFiles = fs.readdirSync(path.resolve(__dirname, templateDir));
  return templateFiles.map((item) => {
    const parts = item.split('.');
    const name = parts[0];
    const extension = parts[1];
    return new HtmlWebpackPlugin({
      filename: `${name}.html`,
      template: path.resolve(__dirname, `${templateDir}/${name}.${extension}`),
      inject: false
    });
  });
}

const config = {
  entry: ['./src/js/index.js', './src/scss/style.scss'],
  // entry: {
  //   main: ['./src/js/index.js', './src/scss/style.scss'],
  //   gamedd: ['./src/js/index_dd', './src/scss/style.scss']
  // },
  output: {
    // filename: './js/[name].bundle.js'
    filename: './js/bundle.js',

    // 2023-04-25 fix корневого пути  для корректной ленивой подгрузки apexchart и js вообще.
    // TODO: сделать аналогичный fix корневого пути и для картинок
    publicPath: '/dist/',
    // path: path.resolve(__dirname, 'dist'), // с этим тоже работает
    // filename: 'js/bundle.js' // с этим тоже работает
  },
  devtool: 'source-map',
  mode: 'production',
  optimization: {
    minimize: true,
    // splitChunks: {
    //   chunks: 'all'
    // },
    minimizer: [
      new TerserPlugin({
        parallel: true,
        cache: true
      })
      // new TerserPlugin({ parallel: true })
    ]
  },
  performance: {
    hints: false
  },
  module: {
    rules: [
      {
        test: /\.vue$/,
        loader: 'vue-loader'
      },
      {
        test: /\.(sass|scss)$/,
        include: path.resolve(__dirname, 'src/scss'),
        use: [
          {
            loader: MiniCssExtractPlugin.loader,
            options: {}
          },
          {
            loader: 'css-loader',
            options: {
              sourceMap: true,
              url: false
            }
          },
          {
            loader: 'postcss-loader',
            options: {
              ident: 'postcss',
              sourceMap: true,
              plugins: () => [
                // eslint-disable-next-line global-require
                require('cssnano')({
                  preset: [
                    'default',
                    {
                      discardComments: {
                        removeAll: true
                      }
                    }
                  ]
                }),
                // включать при сборке продакшен
                // require('autoprefixer')({
                //   grid: true
                // })
              ]
            }
          },
          {
            loader: 'sass-loader',
            options: {
              sourceMap: true
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
          // formatter: require("./.eslintrc.js")
          // cache: true
        }
      },
      {
        test: /\.js$/,
        exclude: /node_modules/,
        use: {
          loader: 'babel-loader'
        }
      },
      {
        test: /\.js$/,
        enforce: 'pre',
        use: ['source-map-loader'],
      },
      {
        test: /\.js$/,
        use: [{
          loader: 'cache-loader'
        }]
      },
      {
        test: /\.(sass|scss)$/,
        use: [{
          loader: 'cache-loader'
        }]
      },
      // {
      //   test: /\.pug$/,
      //   use: [{
      //     loader: 'cache-loader'
      //   }]
      // }
    ]
  },
  plugins: [
    new VueLoaderPlugin(),
    new MiniCssExtractPlugin({
      filename: './css/all.css'
    }),
    new CopyWebpackPlugin([
      {
        from: './src/fonts',
        to: './fonts'
      },
      {
        from: './src/img',
        to: './img'
      }
    ]),
    // new BundleAnalyzerPlugin()
  ],
  resolve: {
    alias: {
      vue: 'vue/dist/vue.js'
    }
  }
};

module.exports = (env, argv) => {
  if (argv.mode === 'production') {
    config.plugins.push(new CleanWebpackPlugin());
    config.resolve.alias.vue = 'vue/dist/vue.min.js';
    config.devtool = 'eval';
    // config.publicPath = './';
    // config.publicPath =  path.resolve(__dirname, 'dist');
  } else {
    config.plugins = config.plugins.concat(generateHtmlPlugins('./src/pug/views'));
  }
  return config;
};
