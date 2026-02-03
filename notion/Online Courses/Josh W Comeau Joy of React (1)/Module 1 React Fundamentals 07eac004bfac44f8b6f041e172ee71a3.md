# Module 1: React Fundamentals

# The magic of React

The truly magical thing about React is that we don't have to worry about keeping our state (held in JavaScript) and our UI (in the DOM) in sync. It takes care of it for us.

With React, we're describing what we want the UI to be, based on the current application state. React takes those descriptions and makes it real.

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