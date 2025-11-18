npm install --save-dev sass-loader@8.0.2
npm install --save-dev style-loader@1.3.0 --legacy-peer-deps



{
  test: /\.(sass|scss)$/i,
  include: path.resolve(__dirname, 'src/scss'),
  use: [
    'cache-loader',
    isProduction
      ? MiniCssExtractPlugin.loader
      : {
          loader: 'style-loader',
          options: { hmr: true } // ← ключевая настройка для HMR в webpack 4
        },
    { loader: 'css-loader', options: { sourceMap: !isProduction, url: false } },
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
  ]
}
