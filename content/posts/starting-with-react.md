---
title: "Starting with React, Building an application & Tips"
date: 2019-05-09T20:43:04+01:00
draft: false
description: "What to expect from React, the basics and further resources."
tags: [react, javascript]
---

> NOTE: This article was origanily [posted on Medium](https://medium.com/@tiago.taquelim.ferreira/starting-with-react-building-an-application-tips-dac62e347230).

![Starting with React](https://res.cloudinary.com/ddfkf8qha/image/upload/v1578085344/floating%20point%20images/startingwithreact_hrp2qy.jpg)

So I’ve been learning React for a few months and I wanted to share some of the knowledge I acquired during this journey.

Building software with React is quite a pleasure for me, I learned new and better ways to do it everyday. However, at the start, especially if you are new to software development it can be a bit overwhelming and complex to understand the abstractions made by these frameworks.

And so I’ve decided to write this article, however **I don’t teach everything about React here**, my objective with this article is **give you an overview of what React can be**, how fun it is and **redirect you to some good resources** for your own journey.

Also, I recommend taking a look at a few things before diving into the React world:

1. Notions of HTML & CSS.
2. Basics of JavaScript.
3. ES6+ Syntax (Optional, but recommended).

## What is React anyways ? 🤔

> “A JavaScript library for building user interfaces”

React is a **JavaScript** library created & maintained by **Facebook** to create User Interfaces.

If you have done web development before and built a medium sized website or application you know that **things can get out of hand** pretty quickly which leads to **bugs**, mainly because the codebase starts to grow. Part of the reason the code gets messy is **repetition**: imagine building a blog and having to copy-paste a blog post to **only then change a few things**.

Well, that’s where React shines 😎. React uses a **Component based model**, meaning that you can create **reusable pieces of code** that **render** the same code and can receive **arguments**, that's where it gets even better.

Although, this is only possible because of a few **React abstractions** which I’ll explain in a bit.

## A Blog

In this article we’ll be **creating a simple blog** that displays a list of posts. You will learn **how to think with React**.

![This is the blog we will be building](https://res.cloudinary.com/ddfkf8qha/image/upload/v1578085357/floating%20point%20images/blogimg_wcegtm.png)

> Note: To get the most of this article I recommend tinkering around or even build the project on your own.

### Overview

The first thing we can do before start coding is **dividing the interface into components & containers** or (**stateless components & stateful components**). You can think of a container as a page (home, settings, about…) that holds multiple components together and it’s from there where we should hold the **state** of the page.

Note #1: **Containers** are written with the class syntax and extend from `React.Component`.

```js
class HomePage extends React.Component {...}
```

Note #2: It’s a good practice to make **Components functions** that are responsible for returning specific pieces of the application.

```js
const DumbComponent = (props) => {
    return (...)
}
```

Also, rendering components in React looks something like this `<HomePage />` or `<HomePage>…</HomePage>`.

So, **state** is what allows us to create **containers that are dynamic** and interactive. It’s a JavaScript object where we store component information like the posts data and maybe the option for a dark mode.

Note: It’s a good practice to **use the state only in containers**.

If we have, for example, five components that belong to a specific page, that need data from the state, we should create **one container** component that will store the state for all of them.

Now let's **divide our interface into components**:

_( Yes, it was made in Paint )_

![Interface divided into components](https://res.cloudinary.com/ddfkf8qha/image/upload/v1578085363/floating%20point%20images/blogimg__2_yr5hc8.png)

Since we will be only creating a **home page**, one container is enough, I called it **Home.js** and it’s where our post list will live. From the beautiful illustration above you can see that we have a Navbar, the Home container(with the posts) and the Footer on the bottom.

Something very interesting that I usually do is create a **Layout.js** component that wraps the page, the home page in this case, as it holds **static content** like the **navbar** and the **footer** so if I visited another page on the app I wouldn’t need to write them again (it’s also a clean way to do it).

## Coding

The code shown below is on my [Github](https://github.com/sikozonpc/React-simple-blog-app) and you can clone it and follow along if you want so.

```bash
git clone https://github.com/sikozonpc/React-simple-blog-app.git
```

Then install all the required dependencies like React:

```bash
npm install
```

Ok, we will start coding, as I said before there might be things in the code that I won’t have time or space to compress here but I can explain to you in the comments or in a next article, that's why some basic React/JavaScript knowledge is good.

This is what the **Render** method in **App.js** looks like, the entry point of our application.

```js
render() {
  return (
    <div className="App">
       <Layout appTitle={this.state.apptitle}>
           {/* Inside the Layout tags are it's childs */}
           <Home posts={this.state.blogPosts} />
       </Layout>
    </div>
 );
}
```

The render method is where React transforms our **JSX** code into **DOM nodes** that the browser can understand, also it’s called multiple times when data is changed in the application that needs to be re-rendered.

When reading the code above, if it’s your first time with React, you might had found a familiar syntax, JavaScript inside HTML elements ? Yes! This is called **JSX** a React **specific abstraction** where you write a HTML-ish syntax with JS, but there are some major differences from the real HTML for instance it’s “className ” instead of “class”. I won’t go to deep but you can read more about JSX, note that this is a **core feature of React** so I recommend learning it.

> [ LINK: Introducing JSX official docs](https://reactjs.org/docs/introducing-jsx.html)

Ok, now that you know what JSX is, take a look at the code below, the first 3 things that I want you to note is the **Layout** that is wrapping the Home container, the **“appTitle”** and **“posts”**. I’m sending **props** from the App.js to the Layout.js, props are the **main way of sending data between components** in React. I’m sending the “appTitle” property from App.js state to the Layout which is sending to one of it’s child components(the navbar). Also, the App.js sends “posts”, array containing the blog posts data, to the Home.js that is responsible for transforming and rendering them.

### Layout.js

```js
const Layout = (props) => {
	return (
		<>
			<Navbar appName={props.appName} />
			<main>{props.children}</main>
			<Footer />
		</>
	);
};
```

Don’t worry if this sounded confusing, it will make sense once you get the hang of React.

Note the `props.appName` prop is where the “Blogy” title ends up. Also, I can use the Layout as a wrapper because of the `props.children`, this is everything that is between the opening and closing tags, in this case it’s the Home component.

This is a rough sketch of the **Props flow** in the app:

![Props flow in the app](https://res.cloudinary.com/ddfkf8qha/image/upload/v1578085352/floating%20point%20images/propsflow_bfn9mb.png)

> The data flows down the component line, from the root to a more specialized component

And this is the **Home.js** container:

```js
class Home extends Component {
	render() {
		const posts = this.props.posts.map((post) => {
			return (
				<BlogPost key={post.id} title={post.title} desc={post.desc} />
			);
		});
		return <div className={classes.Home}>{posts}</div>;
	}
}
```

If you are familiar with ES6+ JS syntax then you might know that in the **posts variable** I am mapping through the **posts prop** that I sent from the App.js which is an array containing each post and for each one it returns a `<BlogPost />` component that is responsible for displaying the blog post data.

> Note: Checkout the code on the repository for more detail on the components code.

This way I’m **dynamically rendering components**, cool right ? This is a very powerful feature of **React’s JSX**, it can accept an array of components and render them.

```js
return <div className={classes.Home}>{posts}</div>;
```

Also, JavaScript code inside JSX must be between {}.

If you are wondering how the state looks like in the **App.js**:

![App.js code](https://res.cloudinary.com/ddfkf8qha/image/upload/v1578085309/floating%20point%20images/stateimg_sva5v8.png)

> **componentDidMount** is a lifecycle method from React.Component that runs after the component output has been rendered to the DOM, and so it’s the perfect place to modify the state.

Before I talk about **Lifecycle methods** note that I used **componentDidMount** to set the state with **setState()** , this is a very important method in React because state in React is **immutable**, which means that state should never be altered or changed directly but rather changed through a copy of the current version of the state, that can easily be made with this.setState().

```js
// Wrong
this.state.appTitle = "App title here";
// Correct
this.setState({ appTitle: "App title here" });
```

So lifecycle methods:

> “Every component in React goes through a lifecycle of events. I like to think of them as going through a cycle of birth, growth, and death.”

This quote from Mosh describes it pretty good. The **lifecycle API is constantly changing** and it’s a huge topic on its’ own so the best I can do is redirect you to some good resources to learn more about them, they are a core feature of React so take your time with them.

1. ) [React Lifecycle Methods — A Deep Dive](https://programmingwithmosh.com/javascript/react-lifecycle-methods/)

2. ) [Official React API Reference](https://reactjs.org/docs/react-component.html?utm_source=caibaojian.com)

3. ) [React State and Lifecycle](https://reactjs.org/docs/state-and-lifecycle.html)

To finish off I would recommend you to **play with this example yourself** and maybe add more features to it or maybe build your application from scratch. There is much more things about React that I can’t fit them all here, so the best thing I can still give you is some places **to learn more about React** that I recommend.

1. ) [React — The Complete Guide](https://www.udemy.com/react-the-complete-guide-incl-redux/)

2. ) [Complete React Tutorial & Redux](https://www.youtube.com/watch?v=OxIDLw0M-m0&list=PL4cUxeGkcC9ij8CfkAY2RAGb-tmkNwQHG)

3. ) [freeCodeCamp React’s section(module 3)](https://learn.freecodecamp.org/)

Thank you very much for reading my first article and I’m available to explain in more detail anything related. Reviews and spelling errors are also welcome 😃. You can talk to me on twitter [@tiago_taquelim](https://twitter.com/tiago_taquelim) .

Happy coding :)
