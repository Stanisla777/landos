npm install --save-dev sass-loader@8.0.2
npm install --save-dev style-loader@1.3.0 --legacy-peer-deps
npm install --save-dev postcss@7 --legacy-peer-deps


ERROR in ./src/scss/style.scss (./node_modules/css-loader/dist/cjs.js??ref--1-1!./node_modules/postcss-loader/src??postcss!./node_modules/sass-loader/dist/cjs.js??ref--1-3!./src/scss/style.scss)
Module build failed (from ./node_modules/postcss-loader/src/index.js):
ValidationError: PostCSS Loader Invalid Options

options.parser should be {String|Object} (https://github.com/postcss/postcss-loader#parser)
options.stringifier should be {String|Object} (https://github.com/postcss/postcss-loader#stringifier)

    at validateOptions (C:\Users\sshchegolev\PhpstormProjects\sprosi.dom.rf\node_modules\postcss-loader\node_modules\schema-utils\src\validateOptions.js:32:11)
    at Object.loader (C:\Users\sshchegolev\PhpstormProjects\sprosi.dom.rf\node_modules\postcss-loader\src\index.js:28:3)
i ｢wdm｣: Failed to compile.

