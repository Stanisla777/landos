npm install --save-dev sass-loader@8.0.2


{
  test: /\.js$/,
  exclude: /node_modules/,
  use: [
    'cache-loader',
    {
      loader: 'babel-loader',
      options: {
        cacheDirectory: true, // ← включите кэш внутри babel-loader тоже!
      }
    }
    // source-map-loader можно убрать в production — он нужен только для дев-режима
  ]
}


