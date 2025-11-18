npm install --save-dev sass-loader@8.0.2
npm install --save-dev style-loader@1.3.0 --legacy-peer-deps
npm install --save-dev postcss@7 --legacy-peer-deps


{
  "overrides": {
    "postcss": "7.0.39",
    "@vue/compiler-sfc>postcss": "7.0.39"
  }
}

npm install --package-lock-only
npm install --no-fund --no-audit --prefer-offline

{
  "name": "start-webapck-template",
  "version": "2.0.0",
  "description": "Start template for generate css, js and html file",
  "main": "src/index.js",
  "scripts": {
    "dev": "webpack --mode development && prettier --print-width=120 --parser html --write dist/*.html",
    "build": "webpack --mode production",
    "start": "webpack-dev-server --mode development --open",
    "lint": "eslint --ext .js, --ignore-path .gitignore ."
  },
  "devDependencies": {
    "@babel/core": "^7.5.5",
    "babel-loader": "^8.0.6",
    "clean-webpack-plugin": "^3.0.0",
    "copy-webpack-plugin": "^5.0.4",
    "css-loader": "^3.1.0",
    "cssnano": "^4.1.10",
    "eslint": "^6.8.0",
    "eslint-config-airbnb-base": "^14.2.1",
    "eslint-loader": "^2.2.1",
    "html-webpack-plugin": "^3.2.0",
    "mini-css-extract-plugin": "^0.8.0",
    "postcss": "^7.0.39",
    "postcss-loader": "^3.0.0",
    "prettier": "^1.18.2",
    "pug": "^2.0.4",
    "pug-loader": "^2.4.0",
    "pug-plain-loader": "^1.1.0",
    "sass-loader": "^8.0.2",
    "source-map-loader": "^1.0.0",
    "style-loader": "1.3.0",
    "terser-webpack-plugin": "^1.4.5",
    "vue-loader": "^15.9.6",
    "vue-template-compiler": "^2.6.12",
    "webpack": "^4.38.0",
    "webpack-bundle-analyzer": "^4.8.0",
    "webpack-cli": "^3.3.6",
    "webpack-dev-server": "^3.7.2"
  },
  "dependencies": {
    "@fancyapps/ui": "^4.0.31",
    "@maxflex/v-number": "^1.0.9",
    "@vkontakte/superappkit": "^1.60.0",
    "apexcharts": "3.54.1",
    "autoprefixer": "^9.6.1",
    "axios": "^0.21.4",
    "axios-jsonp-pro": "^1.1.8",
    "buffer-json": "^2.0.0",
    "cache-loader": "^4.1.0",
    "Datepicker.js": "^0.1.1",
    "eslint-plugin-import": "^2.22.1",
    "imask": "^6.6.3",
    "js-cookie": "^3.0.1",
    "nouislider": "^14.6.3",
    "nvm": "^0.0.4",
    "sass": "^1.80.4",
    "scroll-behavior-polyfill": "^2.0.13",
    "swiper": "^6.4.5",
    "vanilla-calendar-pro": "^2.9.10",
    "vue": "^2.6.12",
    "vue-apexcharts": "1.6.2",
    "vue-axios": "^3.2.2",
    "vue-slider-component": "^3.2.20",
    "vue2-dropzone": "^3.6.0",
    "vuex": "^3.6.2"
  },
  "overrides": {
    "postcss": "7.0.39",
    "@vue/compiler-sfc>postcss": "7.0.39"
  }
}



