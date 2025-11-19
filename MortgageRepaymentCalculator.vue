npm install --save-dev sass-loader@8.0.2
npm install --save-dev style-loader@1.3.0 --legacy-peer-deps
npm install --save-dev postcss@7 --legacy-peer-deps
npm install --save-dev css-loader@4.3.0 --legacy-peer-deps
npm install --save-dev cross-env


2092 modules
i ｢wdm｣: Compiled successfully.

----------------------------------------------------------


C:\Users\sshchegolev\PhpstormProjects\sprosi.dom.rf\node_modules\copy-webpack-plugin\node_modules\schema-utils\src\validateOptions.js:32
    throw new ValidationError(ajv.errors, name);
    ^

ValidationError: CopyPlugin Invalid Options

options should be array

    at validateOptions (C:\Users\sshchegolev\PhpstormProjects\sprosi.dom.rf\node_modules\copy-webpack-plugin\node_modules\schema-utils\src\validateOptions.js:32:11)
    at new CopyPlugin (C:\Users\sshchegolev\PhpstormProjects\sprosi.dom.rf\node_modules\copy-webpack-plugin\dist\index.js:26:30)
    at module.exports (C:\Users\sshchegolev\PhpstormProjects\sprosi.dom.rf\webpack.config.js:195:7)
    at handleFunction (C:\Users\sshchegolev\PhpstormProjects\sprosi.dom.rf\node_modules\webpack-cli\bin\utils\prepareOptions.js:21:13)
    at prepareOptions (C:\Users\sshchegolev\PhpstormProjects\sprosi.dom.rf\node_modules\webpack-cli\bin\utils\prepareOptions.js:9:5)
    at requireConfig (C:\Users\sshchegolev\PhpstormProjects\sprosi.dom.rf\node_modules\webpack-cli\bin\utils\convert-argv.js:117:14)
    at C:\Users\sshchegolev\PhpstormProjects\sprosi.dom.rf\node_modules\webpack-cli\bin\utils\convert-argv.js:123:17
    at Array.forEach (<anonymous>)
    at module.exports (C:\Users\sshchegolev\PhpstormProjects\sprosi.dom.rf\node_modules\webpack-cli\bin\utils\convert-argv.js:121:15)
    at Object.<anonymous> (C:\Users\sshchegolev\PhpstormProjects\sprosi.dom.rf\node_modules\webpack-dev-server\bin\webpack-dev-server.js:84:40)
    at Module._compile (node:internal/modules/cjs/loader:1364:14)
    at Module._extensions..js (node:internal/modules/cjs/loader:1422:10)
    at Module.load (node:internal/modules/cjs/loader:1203:32)
    at Module._load (node:internal/modules/cjs/loader:1019:12)
    at Function.executeUserEntryPoint [as runMain] (node:internal/modules/run_main:128:12)
    at node:internal/main/run_main_module:28:49 {
  errors: [
    {
      keyword: 'type',
      dataPath: '',
      schemaPath: '#/type',
      params: { type: 'array' },
      message: 'should be array'
    }
  ]
}

Node.js v18.20.4
npm notice
npm notice New major version of npm available! 10.7.0 -> 11.6.2
npm notice Changelog: https://github.com/npm/cli/releases/tag/v11.6.2
npm notice To update run: npm install -g npm@11.6.2
npm notice
