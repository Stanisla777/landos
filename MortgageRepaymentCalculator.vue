{
  test: /\.(sass|scss)$/i,
  include: path.resolve(__dirname, 'src/scss'),
  use: [
    'cache-loader', // ← самое начало!
    {
      loader: MiniCssExtractPlugin.loader,
      options: {}
    },
    {
      loader: 'css-loader',
      options: {
        sourceMap: true,
        url: false
      }
    },
    {
      loader: 'postcss-loader',
      options: {
        ident: 'postcss',
        sourceMap: true,
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
        sourceMap: true,
        // 👇 добавляем sassOptions:
        sassOptions: {
          quietDeps: true // ← подавляет warnings из node_modules
        }
      }
    }
  ]
}
