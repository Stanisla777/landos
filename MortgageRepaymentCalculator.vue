npm install --save-dev sass-loader@8.0.2


// ———— Обновляем правило SCSS dinamically ————
  const scssRule = config.module.rules.find((rule) => rule.test && rule.test.toString().includes('sass|scss'));




