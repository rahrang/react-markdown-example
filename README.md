# react-markdown-example
A guide on how to use markdown files in your react app.

We use the following modules:
- `create-react-app`: https://github.com/facebookincubator/create-react-app
- `markdown-loader`: https://github.com/peerigon/markdown-loader/
- `html-loader`: https://github.com/webpack-contrib/html-loader
- `marked`: https://github.com/chjj/marked
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
cd markdown-example
```

Eject your React application.
```bash
npm eject
```

This grants you access to the webpack build files: `webpack.config.dev.js` and `webpack.config.prod.js`.

If you're using git, you'll have to commit your changes before ejecting the app.

---

Install `markdown-loader`, `html-loader`, and `marked`.

```bash
npm install --save markdown-loader html-loader marked
```

Now, open up `webpack.config.dev.js` and `webpack.config.prod.js`. These files are in the `config` directory.

These two files have the same object structure, but with slightly different content. The content *you add*, however, is the same.

In both files, add the following.

1. At the top of the file where the constants are declared, add:
```javascript
const marked = require('marked');
const renderer = new marked.Renderer();
```

2. Locate the `modules` object. Locate its `rules` property - an Array of Objects. An Object in this Array has a `oneOf` property (the key is `oneOf`, the value is an Array).

In this `oneOf` Array, add the following *before* the `file-loader` is run.

```javascript
{
    test: /\.md$/,
    use: [
        {
            loader: require.resolve('html-loader')
        },
        {
            loader: require.resolve('markdown-loader'),
            options: {
                renderer
            }
        }
    ]        
}
```

Upon webpack compilation, markdown (`.md`) files are found. These files are run through the `markdown-loader` pre-processor. The `marked` renderer converts the markdown content to html. The `html-loader` then pre-processes this html into a string.