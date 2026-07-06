ignore-ui-styles-loader.js

// ignore-ui-styles-loader.js
module.exports = function(source) {
  this.cacheable && this.cacheable();
  console.log('[ignore-ui-styles-loader] Обрабатываем файл:', this.resourcePath);
  console.log('[ignore-ui-styles-loader] Контекст:', this.context);
  
  // Возвращаем пустой модуль
  return '';
};


{
      test: /\.(scss|css)$/,
      include: /@ringsites[\\/]ui-components[\\/]components/,
      use: [
        { loader: path.resolve(__dirname, 'ignore-ui-styles-loader.js') }
      ]
    },
