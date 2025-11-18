npm install --save-dev sass-loader@8.0.2
npm install --save-dev style-loader@1.3.0 --legacy-peer-deps

{
  loader: 'postcss-loader',
  options: {
    ident: 'postcss', // ← для webpack 4 + postcss-loader@3 это важно!
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
