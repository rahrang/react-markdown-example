# react-markdown-example
A guide on how to use markdown files in your react app.

We use the following modules:
- `create-react-app`: https://github.com/facebookincubator/create-react-app
- `markdown-loader`: https://github.com/peerigon/markdown-loader/
- `html-loader`: https://github.com/webpack-contrib/html-loader
- `markdown-to-jsx`: https://github.com/probablyup/markdown-to-jsx
- `styled-components`: https://github.com/styled-components/styled-components

This guide will assume you have node and npm (or yarn) installed.

**A few things to keep in mind:**

These commands are equivalent.
```bash
npm install --save MODULE
yarn add MODULE

npm install --global MODULE
yarn add global MODULE

npm ...
yarn ...
```

We will need to `eject` the React application. [Read more here](https://github.com/facebookincubator/create-react-app#converting-to-a-custom-setup) about the implications of doing so.


**Let's get started!**

If you don't have `create-react-app` installed, please do so.
```bash
npm install --global create-react-app
```

Now, create your React application.

```bash
create-react-app markdown-example
```

Eject your React application.
```bash
npm eject
```

This will grant you access to the webpack build files: `webpack.config.dev.js` and `webpack.config.prod.js`.

---

Install `markdown-loader` and `html-loader`.

```bash
npm install --save markdown-loader html-loader
```

Now, open up `webpack.config.dev.js` and `webpack.config.prod.js`. These files will be in the `config` directory.

These two files have the same object structure, but with slightly different content. The content __you add__, however, is the same.

In both files, add the following.