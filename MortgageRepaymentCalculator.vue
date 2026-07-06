new rspack.IgnorePlugin({
  checkResource(resource, context) {
    // Убираем query string для проверки
    const cleanResource = resource.split('?')[0];
    
    // Проверяем, что это SCSS/CSS файл из UI-кита
    const isUiComponentStyle = /@ringsites[\\/]ui-components[\\/]components/.test(cleanResource) && 
                                /\.(scss|css)$/.test(cleanResource);
    
    if (isUiComponentStyle) {
      // Проверяем, что импорт идет из vue-loader
      // context может содержать путь к .vue файлу или к виртуальному модулю vue-loader
      const isFromVueLoader = /\.vue/.test(context) || 
                              /vue-loader/.test(context) ||
                              /\?vue/.test(context) ||
                              /\.vue\.(scss|css)/.test(context);
      
      if (isFromVueLoader) {
        console.log('[IgnorePlugin] Игнорируем стиль из UI-кита:', resource, 'из контекста:', context);
        return true;
      }
    }
    return false;
  }
})
