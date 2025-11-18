npm install --save-dev sass-loader@8.0.2


{
  test: /\.(sass|scss)$/i,
  include: path.resolve(__dirname, 'src/scss'),
  use: [
    'cache-loader',
    MiniCssExtractPlugin.loader,
    {
      loader: 'css-loader',
      options: { sourceMap: true, url: false }
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
        // 👇 Ключевое: явно указываем реализацию и опции
        implementation: require('sass'),
        sassOptions: {
          quietDeps: true,   // ← подавляет warnings из node_modules
          // verbose: false, // по умолчанию
        }
      }
    }
  ]
}
