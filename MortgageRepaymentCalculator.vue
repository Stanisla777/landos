ignore-ui-styles-loader.js

module.exports = function(source) {
  this.cacheable && this.cacheable();
  
  // Логируем ВСЕ файлы, чтобы увидеть реальные пути
  console.log('[LOADER] === SCSS файл ===');
  console.log('[LOADER] resourcePath:', this.resourcePath);
  console.log('[LOADER] context:', this.context);
  console.log('[LOADER] issuer:', this._module?.issuer?.resource);
  console.log('[LOADER] =================');
  
  // Возвращаем исходный код (пока не игнорируем)
  return source;
};
