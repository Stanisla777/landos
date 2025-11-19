npm install --save-dev sass-loader@8.0.2
npm install --save-dev style-loader@1.3.0 --legacy-peer-deps
npm install --save-dev postcss@7 --legacy-peer-deps
npm install --save-dev css-loader@4.3.0 --legacy-peer-deps


2092 modules
i ｢wdm｣: Compiled successfully.

----------------------------------------------------------

npm install --save-dev cross-env

"scripts": {
  "dev": "cross-env NODE_ENV=development webpack --mode development && prettier --print-width=120 --parser html --write dist/*.html",
  "build": "cross-env NODE_ENV=production webpack --mode production",
  "start": "cross-env NODE_ENV=development webpack-dev-server --mode development --open",
  "lint": "eslint --ext .js, --ignore-path .gitignore ."
}


// postcss.config.js
module.exports = {
  plugins: process.env.NODE_ENV === 'production'
    ? [
        require('cssnano')({
          preset: ['default', { discardComments: { removeAll: true } }]
        })
      ]
    : []
};

{
  loader: 'postcss-loader',
  options: {
    ident: 'postcss', // ← обязательно для v3.0.0
    sourceMap: !isProduction
    // plugins — НЕ нужно, берётся из postcss.config.js
  }
}
