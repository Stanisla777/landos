npm install --save-dev sass-loader@8.0.2
npm install --save-dev style-loader@1.3.0 --legacy-peer-deps


// В самом начале webpack.config.js, после импортов:
const cssnano = require('cssnano'); // ← добавьте эту строку

{
  loader: 'postcss-loader',
  options: {
    ident: 'postcss',
    sourceMap: !isProduction,
    plugins: isProduction
      ? [
          cssnano({
            preset: ['default', { discardComments: { removeAll: true } }]
          })
        ]
      : []
  }
}
