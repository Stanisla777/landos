http://localhost:8080/
Cannot GET /instruction.html

{
  "name": "start-webapck-template",
  "version": "2.0.0",
  "description": "Start template for generate css, js and html file",
  "main": "src/index.js",
  "scripts": {
    "dev": "rspack build --mode development && node scripts/compile-pug.js && prettier --print-width=120 --parser html --write dist/*.html",
    "build": "rspack build --mode production && node scripts/compile-pug.js",
    "start": "rspack serve --mode development",
    "lint": "eslint --ext .js, --ignore-path .gitignore ."
  },
  "devDependencies": {
    "@babel/core": "^7.29.0",
    "@babel/preset-env": "^7.29.0",
    "@rspack/cli": "^1.7.7",
    "@rspack/core": "^1.7.7",
    "@vue/compiler-sfc": "^3.5.29",
    "babel-loader": "^8.4.1",
    "clean-webpack-plugin": "^3.0.0",
    "copy-webpack-plugin": "^5.0.4",
    "css-loader": "^3.1.0",
    "cssnano": "^4.1.10",
    "eslint": "^6.8.0",
    "eslint-config-airbnb-base": "^14.2.1",
    "eslint-loader": "^2.2.1",
    "html-webpack-plugin": "^3.2.0",
    "mini-css-extract-plugin": "^0.8.0",
    "postcss-loader": "^3.0.0",
    "prettier": "^1.18.2",
    "pug": "^2.0.4",
    "pug-loader": "^2.4.0",
    "pug-plain-loader": "^1.1.0",
    "sass-loader": "^7.1.0",
    "source-map-loader": "^1.0.0",
    "terser-webpack-plugin": "^1.4.5",
    "webpack": "^4.38.0",
    "webpack-bundle-analyzer": "^4.8.0",
    "webpack-cli": "^3.3.6",
    "webpack-dev-server": "^3.7.2"
  },
  "dependencies": {
    "@fancyapps/ui": "^4.0.31",
    "@vkontakte/superappkit": "^1.60.0",
    "apexcharts": "3.54.1",
    "autoprefixer": "^9.6.1",
    "axios": "^0.21.4",
    "axios-jsonp-pro": "^1.1.8",
    "buffer-json": "^2.0.0",
    "cache-loader": "^4.1.0",
    "Datepicker.js": "^0.1.1",
    "eslint-plugin-import": "^2.22.1",
    "html2canvas": "1.3.3",
    "imask": "^6.6.3",
    "js-cookie": "^3.0.1",
    "jspdf": "2.5.1",
    "nouislider": "^14.6.3",
    "nvm": "^0.0.4",
    "pdfmake": "0.2.7",
    "sass": "^1.80.4",
    "scroll-behavior-polyfill": "^2.0.13",
    "swiper": "^6.4.5",
    "vanilla-calendar-pro": "^2.9.10",
    "vue": "^3.4.0",
    "vue-apexcharts": "1.6.2",
    "vue-axios": "^3.2.2",
    "vuex": "^4.0.0"
  }
}



-----------------------------
compile-pug.js

const fs = require('fs');
const path = require('path');
// eslint-disable-next-line import/no-extraneous-dependencies
const pug = require('pug');

const templateDir = path.resolve(__dirname, '../src/pug/views');
const outputDir = path.resolve(__dirname, '../dist');

if (!fs.existsSync(outputDir)) {
  fs.mkdirSync(outputDir, { recursive: true });
}

const files = fs.readdirSync(templateDir).filter((f) => f.endsWith('.pug'));

files.forEach((file) => {
  const name = file.split('.')[0];
  const templatePath = path.join(templateDir, file);
  const html = pug.renderFile(templatePath);
  fs.writeFileSync(path.join(outputDir, `${name}.html`), html);
  console.log(` ${name}.html`);
});

