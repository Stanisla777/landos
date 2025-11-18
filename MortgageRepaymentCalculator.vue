npm install --save-dev sass-loader@8.0.2


{
  enforce: 'pre',
  test: /\.js$/,
  exclude: /node_modules/,
  loader: 'eslint-loader',
  options: {
    // cache: true  // можно раскомментировать, если хотите кэш eslint
  }
},
{
  test: /\.js$/,
  exclude: /node_modules/,
  use: [
    'cache-loader',
    {
      loader: 'babel-loader',
      options: {
        cacheDirectory: true  // ← включает кэш внутри babel-loader
      }
    },
    'source-map-loader'
  ]
}



