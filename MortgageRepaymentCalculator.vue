npm install --save-dev sass-loader@8.0.2
npm install --save-dev style-loader@1.3.0 --legacy-peer-deps
npm install --save-dev postcss@7 --legacy-peer-deps
npm install --save-dev css-loader@4.3.0 --legacy-peer-deps


{
  loader: 'postcss-loader',
  options: {
    ident: 'postcss',
    sourceMap: !isProduction,
    plugins: isProduction
      ? [
          require('cssnano')({
            preset: ['default', { discardComments: { removeAll: true } }]
          })
        ]
      : []
  }
}
