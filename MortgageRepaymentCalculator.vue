npm install --save-dev sass-loader@8.0.2
npm install --save-dev style-loader@1.3.0 --legacy-peer-deps

// postcss.config.js
module.exports = ({ env }) => ({
  plugins: env === 'production'
    ? [
        require('cssnano')({
          preset: ['default', { discardComments: { removeAll: true } }]
        })
      ]
    : []
});


{
  loader: 'postcss-loader',
  options: {
    sourceMap: !isProduction
    // ← больше НИЧЕГО не нужно: ни `ident`, ни `plugins`, ни `require`
  }
}
