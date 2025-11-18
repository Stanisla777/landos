npm install --save-dev sass-loader@8.0.2
npm install --save-dev style-loader@1.3.0 --legacy-peer-deps
npm install --save-dev postcss@7 --legacy-peer-deps
npm install --save-dev css-loader@4.3.0 --legacy-peer-deps


{
  test: /\.(sass|scss)$/i,
  include: path.resolve(__dirname, 'src/scss'),
  use: [
    // 'cache-loader',
    isProduction ? MiniCssExtractPlugin.loader : 'style-loader',
    { loader: 'css-loader', options: { sourceMap: !isProduction, url: false } },
    // ↓↓↓ ОСТАВИТЬ postcss-loader, но с пустыми плагинами в dev ↓↓↓
    {
      loader: 'postcss-loader',
      options: {
        ident: 'postcss',
        sourceMap: !isProduction,
        plugins: () => isProduction
          ? [
              require('cssnano')({
                preset: ['default', { discardComments: { removeAll: true } }]
              })
            ]
          : [] // ← пустой массив в dev — быстро и без ошибок
      }
    },
    // ↑↑↑ это НЕ вызывает ошибку, если plugins — статический массив ↑↑↑
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
}
