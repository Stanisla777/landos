null-loader.js

{
      test: /\.scss$/,
      include: /@ringsites[\\/]ui-components[\\/]components/,
      issuer: /(\?vue|\.vue\.(scss|css))/,
      use: [
        { loader: path.resolve(__dirname, 'null-loader.js') }
      ]
    },
