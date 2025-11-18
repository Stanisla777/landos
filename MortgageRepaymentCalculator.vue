npm install --save-dev sass-loader@8.0.2
npm install --save-dev style-loader@1.3.0 --legacy-peer-deps

You did not set any plugins, parser, or stringifier. Right now, PostCSS does nothing. Pick plugins for your case on https://www.postcss.parts/ and use them in postcss.config.js.


WARNING in ./src/js/modules/video/play-video.js
Module Warning (from ./node_modules/eslint-loader/index.js):

C:\Users\sshchegolev\PhpstormProjects\sprosi.dom.rf\src\js\modules\video\play-video.js
  56:60  warning  Unexpected unnamed function  func-names

