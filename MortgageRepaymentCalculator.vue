null-loader.js

// null-loader.js
module.exports = function() {
  this.cacheable && this.cacheable();
  return '';
};


 // 🎯 НОВОЕ: Игнорируем CSS из dist/ UI-кита
    {
      test: /\.css$/,
      include: /@ringsites[\\/]ui-components[\\/]dist/,
      use: [
        { loader: path.resolve(__dirname, 'null-loader.js') }
      ]
    },
