C:\Users\sshchegolev\PhpstormProjects\sprosi.dom.rf>npm run start
(node:102536) Warning: Setting the NODE_TLS_REJECT_UNAUTHORIZED environment variable to '0' makes TLS connections and HTTPS requests insecure by disabling certificate verification.
(Use `node --trace-warnings ...` to show where the warning was created)

> start-webapck-template@2.0.0 start
> rspack serve --mode development

<e> [webpack-dev-middleware] Error: Failed to create Rspack compiler instance, check the Rspack configuration.
<e>     at #getInstance (C:\Users\sshchegolev\PhpstormProjects\sprosi.dom.rf\node_modules\@rspack\core\dist\index.js:11089:68)
<e>     at #build (C:\Users\sshchegolev\PhpstormProjects\sprosi.dom.rf\node_modules\@rspack\core\dist\index.js:10954:30)
<e>     at C:\Users\sshchegolev\PhpstormProjects\sprosi.dom.rf\node_modules\@rspack\core\dist\index.js:10937:91
<e>     at done (C:\Users\sshchegolev\PhpstormProjects\sprosi.dom.rf\node_modules\@rspack\lite-tapable\dist\index.cjs:426:13)
<e>     at AsyncSeriesHook.callAsyncStageRange (C:\Users\sshchegolev\PhpstormProjects\sprosi.dom.rf\node_modules\@rspack\lite-tapable\dist\index.cjs:432:46)
<e>     at AsyncSeriesHook.callAsync (C:\Users\sshchegolev\PhpstormProjects\sprosi.dom.rf\node_modules\@rspack\lite-tapable\dist\index.cjs:101:21)
<e>     at Compiler.compile (C:\Users\sshchegolev\PhpstormProjects\sprosi.dom.rf\node_modules\@rspack\core\dist\index.js:10935:38)
<e>     at C:\Users\sshchegolev\PhpstormProjects\sprosi.dom.rf\node_modules\@rspack\core\dist\index.js:10603:31
<e>     at done (C:\Users\sshchegolev\PhpstormProjects\sprosi.dom.rf\node_modules\@rspack\lite-tapable\dist\index.cjs:426:13)
<e>     at next (C:\Users\sshchegolev\PhpstormProjects\sprosi.dom.rf\node_modules\@rspack\lite-tapable\dist\index.cjs:469:55) {
<e>   [cause]: [Error: InvalidArg, Value is non of these types `HashMap`, `bool`, `ThreadsafeFunction`,  on RawHtmlRspackPluginOptions.templateParameters] {
<e>     code: 'GenericFailure'
<e>   }
<e> }
