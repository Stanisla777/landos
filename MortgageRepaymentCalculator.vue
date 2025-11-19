npm install --save-dev sass-loader@8.0.2
npm install --save-dev style-loader@1.3.0 --legacy-peer-deps
npm install --save-dev postcss@7 --legacy-peer-deps
npm install --save-dev css-loader@4.3.0 --legacy-peer-deps


2092 modules
i ｢wdm｣: Compiled successfully.


App hot update...
log.js:24 [HMR] Checking for updates on the server...
VM4842 log.js:24 [HMR] Nothing hot updated.
VM4842 log.js:24 [HMR] App is up to date.
log.js:24 [HMR] Updated modules:
log.js:16 [HMR]  - ./src/scss/style.scss
log.js:24 [HMR] App is up to date.

You did not set any plugins, parser, or stringifier. Right now, PostCSS does nothing. Pick plugins for your case on https://www.postcss.parts/ and use them in postcss.config.js.
