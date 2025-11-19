npm install --save-dev sass-loader@8.0.2
npm install --save-dev style-loader@1.3.0 --legacy-peer-deps
npm install --save-dev postcss@7 --legacy-peer-deps
npm install --save-dev css-loader@4.3.0 --legacy-peer-deps


2092 modules
i ｢wdm｣: Compiled successfully.


{
  test: /\.(sass|scss)$/i,
  include: path.resolve(__dirname, 'src/scss'),
  use: [
    // 'cache-loader',
    isProduction ? MiniCssExtractPlugin.loader : 'style-loader',
    { loader: 'css-loader', options: { sourceMap: !isProduction, url: false } },
    // ↓↓↓ ОСТАВИТЬ ВСЕГДА, НО В DEV — plugins: [] ↓↓↓
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
          : [] // ← ПУСТОЙ МАССИВ В DEV — быстро и без ошибок
      }
    },
    // ↑↑↑ это НЕ вызывает ошибок, если plugins — статический массив ↑↑↑
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

