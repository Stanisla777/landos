npm install --save-dev sass-loader@8.0.2
npm install --save-dev style-loader@1.3.0 --legacy-peer-deps
npm install --save-dev postcss@7 --legacy-peer-deps
npm install --save-dev css-loader@4.3.0 --legacy-peer-deps
npm install --save-dev cross-env


2092 modules
i ｢wdm｣: Compiled successfully.

----------------------------------------------------------

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



1) команда `npm run build` - сформирует минифицированный файл стилей + bundle.js (+ картинки и шрифты) в папку dist (в корне проекта)
2) команда `npm run dev` - сформирует html файлы (из src/pug/views) в папку dist + все assets (img, fonts, js, css)
3) команда `npm run start` - запустит локальный webpack-dev-server с хот релоадом - использовать команду для верстки.
