# Module 1: React Fundamentals

# Hello React

Let's start with a “hello world” React application, using pure JavaScript:

```jsx
// 1. Import dependencies
import React from 'react';
import { createRoot } from 'react-dom/client';

// 2. Create a React element
const element = React.createElement(
  'p',
  { id: 'hello' },
  'Hello World!'
);

// 3. Render the application
const container = document.querySelector('#root');
const root = createRoot(container);
root.render(element);
```

```jsx
<html>
<body>
  <div id="root"></div>
</body>
</html>
```

## 1. Importing dependencies

Note that there are 2 separate packages imported for React. This is because React is “platform agnostic” it can be used in different platforms. 

All platforms have the same core witch comes from the `React` platform, then, to turn the logic into the actual UI each platform has its corresponding package: 

- `react-dom` for the web
- `react-native` for mobile (iOS / Android) or desktop (Windows / macOS) applications
- `react-three-fiber` for 3D scenes using WebGL and Three.js

Each platform has different “primitives”, the built-in elements to create the views. For example, on the web, the primitives to create the UI are the HTML elements like `<div>` , `<p>` and `<button>`. However, React Native (mobile development) doesn’t have divs it has `Text`, `ViewPressable`.

## 2. **Create a React element**

`React.createElement` is a function that accepts 3 or more arguments:

1. The type of the element to create.
2. The properties we want this element to have.
3. The element's contents, what the element should have as children.

This function returns a “React element”. React elements are plain old JavaScript objects. If we inspect it with `console.log(element)`, we'll see something like this:

```json
{
  type:"p",
  key: null,
  ref: null,
  props: {
    id:'hello',
    children:'Hello World!',
  },
  _owner: null,
  _store: { validated: false }
}
```

# **Build Your Own React**

Here I built a simple implementation of the React’s `render` function. The key takeaway is that React is a JS library and uses the same JS methods as I would when doing things. There is no magic here. 

Also, Josh emphasises that all React elements are JS objects describing how the DOM should look like. Plain old JS objects that at the end are transformed in nodes in the real DOM. So with react, we are just describing how the DOM should look like and then it transforms those descriptions creating the pages we want.

# **Understanding JSX**

Writing components using `React.createElement` is time consuming and hard to read when there are long nested hierarchies. JSX is an HTML-like syntax more familiar to us developers and much more readable.

## Compiling vs transpiling

In formal computer-science lingo, "compiling" typically refers to the process of taking human-readable code and transforming it into machine-readable code.

The term "transpiling", by contrast, typically refers to taking one high-level language and transforming it into another high-level language. For example, we might "transpile" JavaScript into Python.

## Extension .jsx

In the early days of React, any file that included JSX had to use the `.jsx` file extension. This way, the compiler knew that a file contained JSX and had to be compiled.

This turned out to be a bit annoying. It was one more thing to think about, one more bit of friction in the development process. Having to rename a file whenever we add/remove JSX is a bit of a pain!

These days, compilers assume that any `.js` file can contain JSX, so, we can include JSX in a `.js` file, and everything will work perfectly.

One exception though, is if you use Vite, a bundler created by the Vue team. This tool still enforces the *no-JSX-in-JS-files* rule. 

Some developers *prefer* to continue using the `.jsx` extension, to make it clear which files actually contain JSX.

## Importing React

When [writing React components](Module%201%20React%20Fundamentals%2071ed585645214bd290aaea0016a19c76.md) we always import the React core package  `import React from 'react'` . Why?  When using JSX it becomes less obvious, remember that all the JSX is compiled to regular JS and ends up using the `React.createElement` function.

With React 17, a new “JSX transformer” was introduced, used by Babel and other compilers. Essentially, it *automatically injects the import* during the build process.

The following code:

```jsx
const element = (
  <p id="hello">
    Hello World!
  </p>
);
```

Would be compiled to:

```jsx
import { jsx as _jsx } from 'react/jsx-runtime';

const element = _jsx(
  'p',
  { id: 'hello' },
  'Hello World!'
);
```

`_jsx` is a fancy optimised version of `React.createElement`. It includes some shortcuts when we use certain React features like Fragments or Portals. Otherwise, it does the exact same thing as `React.createElement`: it creates a React element.

Side note from Josh:

Personally, though, I continue to import React whenever I work with JSX. It's partially that old habits are hard to break, and partially that we still *do* need to import React in order to use many React features, like hooks (covered in depth in Module 2 and Module 3). By always importing React, I know that it'll be there whenever I need it.

# Expression slots