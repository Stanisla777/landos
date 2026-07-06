null-loader.js

// null-loader.js
module.exports = function() {
  // Возвращаем пустой модуль
  this.cacheable && this.cacheable();
  return '';
};

{
      test: /\.scss$/,
      include: /@ringsites[\\/]ui-components[\\/]components/,
      issuer: /(\?vue|\.vue\.(scss|css))/,
      use: [
        { loader: path.resolve(__dirname, 'null-loader.js') }
      ]
    },
