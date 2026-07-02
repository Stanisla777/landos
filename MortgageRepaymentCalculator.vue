showBlock() {
      // Показываем, ТОЛЬКО если:
      // 1. Кука Cookies действительно закрыта
      // 2. Мы на мобильном (опционально, если хотите показывать только на мобильном)
      const cookiesClosed = this.getCookieCookies('cookiewindowclosed');
      if (cookiesClosed === 'yes') {
        this.isHidden = false;
      }
    },


    watch: {
    screenWidth(newWidth) {
      if (newWidth <= 480) {
        const cookiesClosed = this.getCookieCookies('cookiewindowclosed');
        if (cookiesClosed !== 'yes') {
          this.isHidden = true;
        }
        // Если кука есть — isHidden не меняем (показано через showBlock)
      } else {
        this.isHidden = false;
      }
    }
  },

        mounted() {
    if (this.getCookie(this.cookieKey)) {
      return;
    }

    this.active = true;

    const cookiesClosed = this.getCookieCookies('cookiewindowclosed');
    const isMobileNow = window.innerWidth <= 480;

    if (isMobileNow) {
      if (cookiesClosed === 'yes') {
        this.isHidden = false;
      }
      // ❌ УБИРАЕМ: window.addEventListener(...) отсюда
    } else {
      this.isHidden = false;
    }

    // ✅ ✅ ✅ ГЛАВНОЕ ИСПРАВЛЕНИЕ: добавляем слушатель ВСЕГДА
    window.addEventListener('cookies-closed-mobile', this.showBlock);

    // Resize-слушатель
    this._resizeHandler = this.updateScreenWidth;
    window.addEventListener('resize', this._resizeHandler);
  },

  beforeUnmount() {
    // ✅ Чистим ВСЕ слушатели
    window.removeEventListener('cookies-closed-mobile', this.showBlock);
    if (this._resizeHandler) {
      window.removeEventListener('resize', this._resizeHandler);
    }
  }
