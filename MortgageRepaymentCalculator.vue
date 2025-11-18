npm install --save-dev sass-loader@8.0.2


{
  loader: 'sass-loader',
  options: {
    sourceMap: true,
    implementation: require('sass'),
    sassOptions: {
      quietDeps: true,
      silenceDeprecations: ['division-by-slash', 'import']
    }
  }
}

