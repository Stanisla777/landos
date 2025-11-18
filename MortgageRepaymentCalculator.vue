npm install --save-dev sass-loader@8.0.2


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
  // entry будет переопределён ниже в зависимости от режима
  output: {
    filename: './js/bundle.js',
    publicPath: '/dist/'
  },
  devtool: 'source-map',
  mode: 'production',
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
        test: /\.(sass|scss)$/i,
        include: path.resolve(__dirname, 'src/scss'),
        use: [
          'cache-loader',
          // будет заменён в module.exports → MiniCssExtractPlugin.loader или style-loader
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
          cache: true
        }
      },
      {
        test: /\.js$/,
        exclude: /node_modules/,
        use: [
          'cache-loader',
          {
            loader: 'babel-loader',
            options: {
              cacheDirectory: true
            }
          },
          'source-map-loader'
        ]
      }
    ]
  },
  plugins: [
    new VueLoaderPlugin(),
    new CopyWebpackPlugin([
      { from: './src/fonts', to: './fonts' },
      { from: './src/img', to: './img' }
    ])
  ],
  resolve: {
    alias: {
      vue: 'vue/dist/vue.js'
    }
  }
};

module.exports = (env, argv) => {
  const isProduction = argv.mode === 'production';

  // ———— Динамическая настройка в зависимости от режима ————

  // entry
  config.entry = isProduction
    ? ['./src/js/index.js', './src/scss/style.scss']
    : {
        main: './src/js/index.js',
        styles: './src/scss/style.scss'
      };

  // output filename
  config.output.filename = isProduction
    ? './js/bundle.js'
    : './js/[name].bundle.js';

  // devtool
  config.devtool = isProduction ? 'source-map' : 'eval-cheap-module-source-map';

  // optimization
  config.optimization = {
    minimize: isProduction,
    minimizer: isProduction
      ? [
          new TerserPlugin({
            parallel: true,
            cache: true
          })
        ]
      : []
  };

  // plugins
  if (isProduction) {
    config.plugins.push(
      new CleanWebpackPlugin(),
      new MiniCssExtractPlugin({
        filename: './css/all.css'
      })
    );
    config.resolve.alias.vue = 'vue/dist/vue.min.js';
  } else {
    // В dev — подключаем HtmlWebpackPlugin, но с HMR-дружественной схемой
    config.plugins = config.plugins.concat(
      generateHtmlPlugins('./src/pug/views')
    );
  }

  // ———— Обновляем правило SCSS dinamically ————
  const scssRule = config.module.rules.find(rule =>
    rule.test && rule.test.toString().includes('sass|scss')
  );

  scssRule.use = [
    'cache-loader',
    isProduction
      ? MiniCssExtractPlugin.loader
      : 'style-loader',
    {
      loader: 'css-loader',
      options: {
        sourceMap: !isProduction,
        url: false
      }
    },
    {
      loader: 'postcss-loader',
      options: {
        ident: 'postcss',
        sourceMap: !isProduction,
        plugins: () => [
          require('cssnano')({
            preset: ['default', { discardComments: { removeAll: true } }]
          })
        ]
      }
    },
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
  ];

  // ———— devServer (для npm start) ————
  if (!isProduction) {
    config.devServer = {
      contentBase: path.resolve(__dirname, 'dist'),
      publicPath: '/dist/',
      hot: true,
      compress: true,
      port: 8080,
      stats: 'minimal',
      open: true
    };
  }

  return config;
};




