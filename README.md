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

---
Now, let's create the markdown file we want to use in our app.

We'll name this file `example.md`, and place it in the same `src` directory as `App.js`.

```markdown
# React Markdown Example **(h1)**

Lorem ipsum dolor sit amet. **(p)**

[A link to the GitHub repository](https://github.com/rahrang/react-markdown-example) **(a)**

## The First Part **(h2)**

Hello! Welcome to the first part. **(p)**

### What we're doing **(h3)**

We're making our react app use markdown files! **(p)**

### Why we're doing it **(h3)**

So that when we're writing a LOT of text, we can write and style quickly using markdown instead of HTML! This allows us to write several markdown files with content that we need, and then use simple logic to determine which markdown file should be fed into our markdown component! **(p)**

### How we're doing it

Using the `markdown-to-jsx` component! **(p)** & **(code)**

--- **(hr)**

ADD AN IMAGE HERE
```

---
Now, let's get the markdown to show on-screen.

Start the development server.
```bash
npm run start
```
This will open up a browser tab `localhost:3000`. Navigate to it, if necessary.
You should see the default `create-react-app` fillers.

In `App.js`, remove all the default tags and text, and the `logo` import. You should be left with this:

```javascript
import React, { Component } from 'react';
import './App.css';

class App extends Component {
  render() {
    return (
      <div className="App">

      </div>
    );
  }
}

export default App;
```

Let's import our markdown module. Add the following.

```javascript
import Markdown from 'markdown-to-jsx';
```

Now, we'll import our markdown file.

```javascript
const mdFile = require('./example.md');
```

Modify the render function as such:

```javascript
render() {
    return (
        <div className="App">
            <Markdown>
                {mdFile}
            </Markdown>
        </div>
    )
}
```
You should see your markdown file generated in `localhost:3000`.

---
Now, let's style our markdown!

Import the styled-components module into `App.js`.
```javascript
import styled from 'styled-components';
```

We'll define the following styles at the bottom of the file.

```javascript
const MainTitle = styled.h1`
  color: #2980b9;
  border-bottom: 3px solid #333;
`

const SectionTitle = styled.h2`
  color: #895fad;
  border-bottom: 2px dashed #333;
  text-align: right;
  margin: 0 20px;
`

const SubSectionTitle = styled.h3`
  color: #2980b9;
  text-transform: uppercase;
`

const ParaText = styled.p`
  color: green;
  text-align: left;
  margin: 0 20px;
  line-height: 1.375rem;
`

const StrongText = styled.strong`
  color: black;
  padding: 2px;
  text-decoration: underline;
`

const ExtLink = styled.a.attrs({
  target: '_blank'
})`
  color: #2980b9;
  &:hover {
    color: #ffd700;
  }
`

const Code = styled.code`
  color: red;
  font-size: 1.125rem;
`

const DividerLine = styled.hr`
  border: 1px solid #2980b9;
`

const Image = styled.img`
  border: 5px solid #895fad;
  border-left: none;
  border-right: none;
`
```

Then, we'll create an `options` Object below our styled component declarations.

```javascript
const options = {
    overrides: {
        h1: {
            component: MainTitle
        },
        h2: {
            component: SectionTitle
        },
        h3: {
            component: SubSectionTitle
        },
        p: {
            component: ParaText
        },
        strong: {
            component: StrongText
        },
        a: {
            component: ExtLink
        },
        code: {
            component: Code
        },
        img: {
            component: Image
        },
        hr: {
            component: DividerLine
        }
    }
}
```

Then, we'll pass `options` into our Markdown component.

```javascript
render() {
    return (
        <div className="App">
            <Markdown options={options}>
                {mdFile}
            </Markdown>
        </div>
    )
}
```

And now we're done! You'll obviously want better styles than this, but this is how you can use markdown in your React application.

For a better example on how it's used, refer to my website. All project descriptions are written in markdown files.

[My Website](http://rahrang.xyz/projects/) (Click on a project here)
[Corresponding Code](https://github.com/rahrang/rahrang/blob/master/src/containers/Projects/ProjectPage.jsx)