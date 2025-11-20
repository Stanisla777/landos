npm install --save-dev sass-loader@8.0.2
npm install --save-dev style-loader@1.3.0 --legacy-peer-deps
npm install --save-dev postcss@7 --legacy-peer-deps
npm install --save-dev css-loader@4.3.0 --legacy-peer-deps
npm install --save-dev cross-env
npm install concurrently@8.2.2 --save-dev



"scripts": {
  "start": "webpack-dev-server --mode development --hot --open",
  "dev": "webpack --mode development --env devOutput && prettier --print-width=120 --parser html --write dist/*.html",
  "build": "webpack --mode production",
"lint": "eslint --ext .js --ignore-path .gitignore ."
}

----------------------------------

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
  return templateFiles.map(item => {
    const [name, extension] = item.split('.');
    return new HtmlWebpackPlugin({
      filename: `${name}.html`,
      template: path.resolve(__dirname, `${templateDir}/${name}.${extension}`),
      inject: true,
      chunks: ['main', 'styles']
    });
  });
}

module.exports = (env, argv) => {
  const mode = argv.mode || 'development';
  const isProduction = mode === 'production';
  const isDevOutput = argv.env && argv.env.devOutput; // ← true при npm run dev

  return {
    entry: {
      main: './src/js/index.js',
      styles: './src/scss/style.scss'
    },
    output: {
      filename: isProduction ? './js/[name].[contenthash:8].js' : './js/[name].js',
      publicPath: '/dist/'
    },
    devtool: isProduction ? 'source-map' : 'eval-cheap-module-source-map',
    mode,

    optimization: {
      minimize: isProduction,
      minimizer: isProduction ? [
        new TerserPlugin({ parallel: true, terserOptions: { compress: { drop_console: true } } })
      ] : [],
      splitChunks: isProduction ? {
        chunks: 'all',
        cacheGroups: {
          vendor: { test: /[\\/]node_modules[\\/]/, name: 'vendors', chunks: 'all' }
        }
      } : false
    },

    devServer: {
      contentBase: path.resolve(__dirname, 'dist'),
      publicPath: '/dist/',
      hot: true,
      port: 8080,
      open: true,
      watchOptions: { ignored: /node_modules/, aggregateTimeout: 50 }
    },

    module: {
      rules: [
        { test: /\.vue$/, use: ['cache-loader', 'vue-loader'] },
        {
          test: /\.pug$/,
          oneOf: [
            { include: path.resolve(__dirname, 'src/pug/'), exclude: /\.vue$/, use: ['cache-loader', 'pug-loader'] },
            { use: ['pug-plain-loader'] }
          ]
        },
        {
          test: /\.js$/,
          exclude: /node_modules/,
          use: ['cache-loader', { loader: 'babel-loader', options: { cacheDirectory: true } }]
        },
        {
          test: /\.(sass|scss)$/i,
          use: [
            // ✅ В dev-server — style-loader (HMR), в dev/prod — MiniCssExtractPlugin.loader
            (isDevOutput || isProduction) ? MiniCssExtractPlugin.loader : 'style-loader',
            'css-loader',
            'cache-loader',
            {
              loader: 'sass-loader',
              options: {
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
          test: /\.(png|jpe?g|gif|svg|webp)$/i,
          loader: 'file-loader',
          options: { name: 'img/[name].[ext]', publicPath: '/dist/' }
        },
        {
          test: /\.(woff2?|eot|ttf|otf)$/i,
          loader: 'file-loader',
          options: { name: 'fonts/[name].[ext]', publicPath: '/dist/' }
        }
      ]
    },

    plugins: [
      new VueLoaderPlugin(),
      (isProduction || isDevOutput) && new MiniCssExtractPlugin({ filename: './css/all.css' }), // ✅
      new CopyWebpackPlugin([{ from: './src/fonts', to: './fonts' }, { from: './src/img', to: './img' }]),
      new webpack.DefinePlugin({ 'process.env.NODE_ENV': JSON.stringify(mode) }),
      mode === 'development' && !isDevOutput && new webpack.HotModuleReplacementPlugin(), // только для start
      isProduction && new CleanWebpackPlugin()
    ].filter(Boolean),

    resolve: {
      alias: { vue: isProduction ? 'vue/dist/vue.min.js' : 'vue/dist/vue.js' },
      extensions: ['.js', '.vue', '.json']
    }
  };
};
