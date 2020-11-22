# problem_solutions


---

# webpack + dotenv

require dotenv package with `.env` file

  ```js
  const dotenv = require('dotenv').config({path: __dirname + '/.env'});
  ```

 in webpack config plugins section use

  ```js
  new webpack.DefinePlugin({
    "process.env": dotenv.parsed
  }),
  ```

OR

```js
import dotenv from 'dotenv'
import { DefinePlugin } from 'webpack'


...

plugins: [
    new DefinePlugin({
      'process.env': JSON.stringify(dotenv.config().parsed)
    })
]

```

OR

```js
module.exports = (env) => {
  // create a nice object from the env variable
  const envKeys = Object.keys(env).reduce((prev, next) => {
    prev[`process.env.${next}`] = JSON.stringify(env[next]);
    return prev;
  }, {});

  return {
    plugins: [
      new webpack.DefinePlugin(envKeys)
    ]
  };
};
```

### That whole webpackDefinePlugin stuff was working pretty well earlier, so let’s use it again in our webpack configuration:

```js
const webpack = require('webpack');
const dotenv = require('dotenv');

module.exports = () => {
  // call dotenv and it will return an Object with a parsed key
  const env = dotenv.config().parsed;

  // reduce it to a nice object, the same as before
  const envKeys = Object.keys(env).reduce((prev, next) => {
    prev[`process.env.${next}`] = JSON.stringify(env[next]);
    return prev;
  }, {});

  return {
    plugins: [
      new webpack.DefinePlugin(envKeys)
    ]
  };
};
```

### Using npm scripts to set environment variables

Go to your package.json, check the scripts key and look for the command(s) that run webpack. It’ll probably look something similar like this:

<!-- the rest of your package.json -->
  ```json
  {
  "scripts": {
      "dev": "webpack --config webpack.config.dev.js",
      "build": "webpack --config webpack.config.build.js",
      "dev_test": "webpack --env.API_URL=http://localhost:8000 --config webpack.config.dev.js",
      "build_test": "webpack --env.API_URL=https://www.myapi.com --config webpack.config.build.js"
    }
  }
  ```


---
# Module not found: error can t resolve 'fs' in mime

Add the following to your Webpack config:

  ```json
  node: {
    "fs": "empty"
  }
  ```

---

# Reactjs set environment variables

react-scripts actually uses dotenv library under the hood.

With react-scripts@0.2.3 and higher, you can work with environment variables this way:

1. create .env file in the root of the project
2. set environment variables starting with REACT_APP_ there
3. access it by process.env.REACT_APP_... in components  

```properties
# .env
REACT_APP_BASE_URL=http://localhost:3000
```

---


# Error: Cannot find module 'webpack-cli/bin/config-yargs'

use the webpack-3.3.12 version `npm i webpack-cli@3.3.12` and it should work!

---


# TypeError: Cannot assign to read only property 'jsx' of object '#Object'

Here's how I solved this temporarily until `react-scripts@4.01` is released.

1. Place a file in your package root called `verifyTypeScriptSetup.js` that contains the following.

    ```js
    "use strict";
    function verifyTypeScriptSetup() {}
    module.exports = verifyTypeScriptSetup;
    ```

2. Add the following `prestart` to your `scripts` in `package.json`

    ```json
    "prestart": "cp verifyTypeScriptSetup.js node_modules/react-scripts/scripts/utils",
    ```

Now when you run `npm start` the errant `verifyTypeScriptSetup.js` in `node_modules` is patched.

---
