{
  test: /\.pug$/,
  use: [
    'html-loader',
    {
      loader: 'pug-html-loader',
      options: {
        pretty: true
      }
    }
  ]
}
