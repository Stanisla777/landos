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
