npm install --save-dev sass-loader@8.0.2
npm install --save-dev style-loader@1.3.0 --legacy-peer-deps
npm install --save-dev postcss@7 --legacy-peer-deps
npm install --save-dev css-loader@4.3.0 --legacy-peer-deps
npm install --save-dev cross-env


2092 modules
i ｢wdm｣: Compiled successfully.

----------------------------------------------------------

npm install --save-dev cross-env

"scripts": {
  "dev": "node -e \"process.env.NODE_ENV='development'; require('child_process').spawnSync('webpack', ['--mode','development'], {stdio:'inherit'});\" && prettier --print-width=120 --parser html --write dist/*.html",
  "build": "node -e \"process.env.NODE_ENV='production'; require('child_process').spawnSync('webpack', ['--mode','production'], {stdio:'inherit'});\"",
  "start": "node -e \"process.env.NODE_ENV='development'; require('child_process').spawnSync('webpack-dev-server', ['--mode','development','--open'], {stdio:'inherit'});\"",
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
    ident: 'postcss',
    sourceMap: !isProduction
  }
}



