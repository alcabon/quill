Here's a step-by-step guide to set up esbuild for Quill:
Install esbuild
First, make sure you're in the Quill project directory, then install esbuild as a dev dependency:
bash

npm install --save-dev esbuild

Create a build script
Create a new file called build.js in the root of your project:
javascript

const esbuild = require('esbuild');
const path = require('path');

esbuild.build({
  entryPoints: ['./core.js'],
  bundle: true,
  outfile: 'dist/quill.js',
  format: 'iife',
  globalName: 'Quill',
  platform: 'browser',
  target: ['es6'],
  sourcemap: true,
  minify: process.env.NODE_ENV === 'production',
  define: {
    'process.env.NODE_ENV': JSON.stringify(process.env.NODE_ENV || 'development')
  },
}).catch(() => process.exit(1));

This script:
Uses core.js as the entry point (adjust if needed)
Bundles all dependencies
Outputs to dist/quill.js
Creates a global Quill variable
Targets ES6
Generates sourcemaps
Minifies in production mode
Defines process.env.NODE_ENV
Update package.json
Add new scripts to your package.json:
json
{
  "scripts": {
    "build": "node build.js",
    "build:prod": "NODE_ENV=production node build.js"
  }
}

Run the build
Now you can run the build:
bash
npm run build

For a production build:
bash
npm run build:prod

Adjust entry point (if necessary)
Make sure your core.js (or whatever your main entry file is) exports Quill correctly. It should look something like this:
javascript
import Quill from './quill';
export default Quill;

Handle CSS (optional)
If you need to bundle CSS, you can use a plugin or a separate build step. Here's an example of how to include CSS using esbuild:
javascript
const esbuild = require('esbuild');
const path = require('path');

esbuild.build({
  entryPoints: ['./core.js', './assets/snow.styl'],
  bundle: true,
  outdir: 'dist',
  format: 'iife',
  globalName: 'Quill',
  platform: 'browser',
  target: ['es6'],
  sourcemap: true,
  minify: process.env.NODE_ENV === 'production',
  define: {
    'process.env.NODE_ENV': JSON.stringify(process.env.NODE_ENV || 'development')
  },
  loader: {
    '.styl': 'css'
  }
}).catch(() => process.exit(1));

This assumes you're using Stylus for CSS. You might need to adjust based on Quill's actual CSS setup.
Testing the build
After building, you can test the output by creating a simple HTML file:
xml
<!DOCTYPE html>
<html>
<head>
  <script src="dist/quill.js"></script>
</head>
<body>
  <div id="editor"></div>
  <script>
    var quill = new Quill('#editor', {
      theme: 'snow'
    });
  </script>
</body>
</html>

Remember to include any necessary CSS files as well.
This setup should give you a good starting point for building Quill with esbuild. You may need to make adjustments based on Quill's specific requirements and structure. Also, ensure that all necessary dependencies are correctly imported and that the build output meets Quill's functionality requirements.
