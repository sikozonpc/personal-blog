---
title: "Setting up Webpack with React"
date: 2019-10-10T20:43:04+01:00
draft: false
description: "What is webpack ? Why use it ? Setting up your own"
tags: [react, javascript]
---


![What is webpack](https://res.cloudinary.com/ddfkf8qha/image/upload/v1578085091/what-is-webpack_gx73cc.png)

> "At its core, **webpack** is a **static module bundler** for modern JavaScript applications. When webpack processes your application, it internally builds a dependency graph which maps every module your project needs and generates one or more bundles." -[Webpack](https://webpack.js.org/concepts/)

  

That quote is the original description that's on the webpack official website.

  

If you don't understood it well and you don't even know what a module bundler is don't worry, by the end of this article I will do my best to try to explain everything.

  

> Note that I'm just a fellow student of the web, just like you. There is a lot stuff I learned writing this story.

  

## What is webpack then ?

  

Webpack is **tool** that works by feeding it a **configuration file**, this file explains the builder **how to load and bundle** specific things (such as JavaScript files, images, stylesheets...) and an **entry point**, the root of your application.

  

Then, when you run webpack, it'll **dive from the entry point** down to the end of your program, figuring out what it needs and what it depends on.

  

To then create a few optimized **bundles** that is your **final static application**, ready to be used on the the browser.

  

Note that webpack runs during development, not in your finished app.

  

## Cool... but why ?

  

Before I knew about module builders and build tools I just coded my websites and deployed them, just like they were, unoptimized, without new JavaScript features, multiple `.html` and `.js` files, you could even see my whole file structure on the sources tab on Google Chrome's dev tools.

  

Before I realized I was sending 20 MB for a small static website.

  

Then I researched optimization techniques and **minified** my css and js. Added another tool for **prefixing** my css, another for **image optimization**, you name it.

  

Ofc I had to run a few npm commands everytime I wanted to ship my code and my development experience wasn't great.

  

Thats where module builders come to save the day.

  
  

## The Incosistency when loading scripts problem

  

When loading scripts from a webpage, sometimes they depend on each other, so they **need to be in the right order!** not to mention that they are **loaded asynchronously** by default, and scripts that depend on others might be loaded before them.

  

The module proposal solves this issue, where a **encapsulated piece of code** can be referred as a **package** and be loaded from another script.

  

I won't go much deeper in theory, but if you have the time I highly recommend this resources:

  

-  [What is Webpack, from SurviveJS](https://survivejs.com/webpack/what-is-webpack/)

-  [What is webpack and why should I care series](https://medium.com/the-self-taught-programmer/what-is-webpack-and-why-should-i-care-part-1-introduction-ca4da7d0d8dc)

-  [Official webpack core concepts](https://webpack.js.org/concepts/)

  

### The setup

  

I'm going to setup a webpack environment that will support [React](https://reactjs.org/), [TypeScript](https://www.typescriptlang.org/), [Sass](https://sass-lang.com/), going to use [Babel](https://babeljs.io/) to compile newer JavaScript to a compatible version with the browsers, ESLint for linting, and some other cool stuff.

  

Ok let's start the project.

  

Create a new folder for your boilerplate project:

  
  

```bash
mkdir webpack-sass-react-ts-boilerplate
```

  

Then, our project file structure will look something like this:

  

```bash
|- dist (built and bundled code)
|- public
  | index.html
|- src
  |- assets (to store images, videos, icons, fonts...)
  |- components (React components)
  |- containers (React pages/component with state)
  |- utils (utiliyu functions)
  |- store (Redux store)[optional]
```

  

You're welcome to change the architecture however you want, I like to use a container/component based structure.

  

Make sure you have [Node](https://nodejs.org/en/) installed on your machine with `npm -v`

  

Run `npm init` inside the root folder of the project to initialize npm.

  

Now lets install:

- Webpack:

```bash
npm i webpack webpack-cli --save-dev
```

- React and React DOM:

  

```bash
npm i react react-dom --save
```

- TypeScript:

```bash
npm i typescript --save-dev
```

  

- Babel:

  

```bash
npm i @babel/core babel-loader @babel/preset-react @babel/preset-typescript @babel/preset-env @babel/plugin-proposal-class-properties @babel/plugin-proposal-object-rest-spread @babel/plugin-syntax-dynamic-import --save-dev
```

  

- Eslint:

  

```bash
npm i eslint --save-dev
```

  
  
  

We are still not done installing dev dependencies, we still need to install **webpack loaders**, they allow you to **bundle any static resource way beyond JavaScript.**

  

> Webpack needs to be taught on how to process your files, and that's the job of loaders, they search the project for specific files and do their job.

  

-  [Webpack Loaders](https://webpack.js.org/loaders/)

  

- Css loader:

  
```bash
npm i style-loader css-loader --save-dev
```

  

- Sass Loader/Post css loader

  

```bash
npm i sass-loader postcss-loader --save-dev
```

  

- Html Loader

  

```bash
npm install html-webpack-plugin --save-dev
```
  

- Webpack dev server (watches our changes in the code and refreshes the webpage)

  

```bash
npm install webpack-dev-server --save-dev
```



### The configuration

  

Now let's put it all together and configure Webpack, TypeScript, Babel and Eslint.

  

First let's setup ESLint, if you have the eslint cli tool (using npm install eslint --global) you can run `eslint --init` otherwise run `./node_modules/.bin/eslint --init` and setup as you like.

I won't go into much detail about each configuration variable since they're all better documented at the the [eslint docs]([https://eslint.org/docs/user-guide/configuring](https://eslint.org/docs/user-guide/configuring)).

```js
module.exports = {
  parser: "@typescript-eslint/parser",
  parserOptions: {
    project: "./tsconfig.json",
    ecmaVersion: 2018, // Allows for the parsing of modern ECMAScript features
    sourceType: "module",
    ecmaFeatures: {
      jsx: true
    }
  },
  plugins: ["@typescript-eslint", "react", "jsx-a11y" ],
  env: {
    browser: true,
    es6: true,
    jest: true,
    node: true,
  },

  extends: [
    'standard',
    "eslint:recommended",
    "plugin:react/recommended",
    "plugin:@typescript-eslint/eslint-recommended",
    "plugin:@typescript-eslint/recommended",
    "plugin:jsx-a11y/recommended"
  ],
  globals: {
    Atomics: 'readonly',
    SharedArrayBuffer: 'readonly'
  },
  settings: {
    "react": {
      "version": "detect"
    }
},
  rules: {
    'semi': ['error', 'never'],
    'prefer-const': 'error',
    'no-var': 'error',
    '@typescript-eslint/member-delimiter-style': 'off',
    'comma-dangle': ['error', 'always-multiline'],
    "@typescript-eslint/indent": ["error", 2],
    "react/prop-types": "off",
    "node/no-missing-import": "off",
    "@typescript-eslint/explicit-function-return-type": "off",
    "@typescript-eslint/no-explicit-any": "off",
    "@typescript-eslint/ban-ts-ignore": ["warn"],
    "react/display-name": "off",
  },
}
```

Now on the the [webpack configuration]([https://github.com/sikozonpc/Webpack-React-Sass-boilerplate/blob/master/webpack.config.js](https://github.com/sikozonpc/Webpack-React-Sass-boilerplate/blob/master/webpack.config.js)) we configure and start all of the loaders we installed so our code can be properly compiled.

We set up the **entry file of our application**, usually in a React application, you would have an `index.js` file that contains the root of the application with all of the wrappers and router setup.
Well, we have to tell webpack to start from there. And then it will traverse the whole app and compile it according to our config.
```js
entry:  path.resolve(__dirname, 'src', 'index.js'),
```

Something super duper interesting in this config is the `resolve: {...}` , here we can decide how our modules are resolved and even create alias.
This means that instead of calling: 

```js
import Button from '../../../components/Button'
```

we could just call it like a npm module:

```js
import Button from 'components/Button'
```

This is how I've setup mine:

```js
resolve: {
	extensions: ['.ts', '.tsx', '.js', 'jsx'],
	modules: [
		path.resolve(__dirname, 'src'),
		'node_modules',
	],
},
```

Basically all files under `/src/**` can be used under an alias.

And finally it's on `modules: {}` where we setup all of our loaders.

Well this blog post is already large enough so I will explain in more detail later the Babel and Typescript configuration, but for now you can explore their docs and read the one I wrote in my [boilerplate]([https://github.com/sikozonpc/Webpack-React-Sass-boilerplate](https://github.com/sikozonpc/Webpack-React-Sass-boilerplate)).

That's all for now folks, and as always you can reach me if you wish to have a chat or talk about code 😉.

Take care,
