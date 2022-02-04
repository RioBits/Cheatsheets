# React.js Library

> **React** is a JavaScript library for building user interfaces.

## Add React to a Website

```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="UTF-8" />
    <title>Hello World</title>
    <script src="https://unpkg.com/react@17/umd/react.development.js"></script>
    <script src="https://unpkg.com/react-dom@17/umd/react-dom.development.js"></script>

    <!-- Don't use this in production: -->
    <script src="https://unpkg.com/@babel/standalone/babel.min.js"></script>
  </head>
  <body>
    <div id="root"></div>
    <script type="text/babel">
      ReactDOM.render(<h1>Hello, world!</h1>, document.getElementById('root'))
    </script>
    <!--
      Note: this page is a great way to try React but it's not suitable for production.
      It slowly compiles JSX with Babel in the browser and uses a large development build of React.

      Read this section for a production-ready setup with JSX:
      https://reactjs.org/docs/add-react-to-a-website.html#add-jsx-to-a-project

      In a larger project, you can use an integrated toolchain that includes JSX instead:
      https://reactjs.org/docs/create-a-new-react-app.html

      You can also use React without JSX, in which case you can remove Babel:
      https://reactjs.org/docs/react-without-jsx.html
    -->
  </body>
</html>
```

React Without JSX
```js
const e = React.createElement;

// Display a "Like" <button>
return e(
  'button',
  { onClick: () => this.setState({ liked: true }) },
  'Like'
);
```

React With JSX
```jsx
// Display a "Like" <button>
return (
  <button onClick={() => this.setState({ liked: true })}>
    Like
  </button>
);
```

## Create React App

using npm
```
npx create-react-app my-app
cd my-app
npm start

```
using yarn
```
yarn create react-app my-app
cd my-app
yarn start
```

## CDN Links

development
```html
<script crossorigin src="https://unpkg.com/react@17/umd/react.development.js"></script>
<script crossorigin src="https://unpkg.com/react-dom@17/umd/react-dom.development.js"></script>
```

production
```html
<script crossorigin src="https://unpkg.com/react@17/umd/react.production.min.js"></script>
<script crossorigin src="https://unpkg.com/react-dom@17/umd/react-dom.production.min.js"></script>
```

## The smallest React example
```jsx
ReactDOM.render(
  <h1>Hello, world!</h1>,
  document.getElementById('root')
);
```

## JSX

### Consider this variable declaration

```jsx
const element = <h1>Hello, world!</h1>;
```

### Embedding Expressions in JSX

```jsx
const name = 'Josh Perez';
const element = <h1>Hello, {name}</h1>;

ReactDOM.render(
  element,
  document.getElementById('root')
);
```

```jsx
function formatName(user) {
  return user.firstName + ' ' + user.lastName;
}

const user = {
  firstName: 'Harper',
  lastName: 'Perez'
};

const element = (
  <h1>
    Hello, {formatName(user)}!
    <!-- Hello, Harper Perez -->
  </h1>
);

ReactDOM.render(
  element,
  document.getElementById('root')
);
```

### JSX is an Expression Too
```jsx
function getGreeting(user) {
  if (user) {
    return <h1>Hello, {formatName(user)}!</h1>;
  }
  return <h1>Hello, Stranger.</h1>;
}
```

### Specifying Attributes with JSX

```jsx
const element = <a href="https://www.reactjs.org"> link </a>
```

```jsx
const element = <img src={user.avatarUrl}></img>;
```

> Since JSX is closer to JavaScript than to HTML, React DOM uses `camelCase` property naming convention instead of HTML attribute names.

> For example, `class` becomes `className` in JSX, and tabindex becomes tabIndex.

### Specifying Children with JSX

```jsx
const element = (
  <div>
    <h1>Hello!</h1>
    <h2>Good to see you here.</h2>
  </div>
);
```

### JSX Prevents Injection Attacks

```jsx
const title = response.potentiallyMaliciousInput;
// This is safe:
const element = <h1>{title}</h1>;
```
> By default, React DOM escapes any values embedded in JSX before rendering them. This helps prevent XSS (cross-site-scripting) attacks.

### JSX Represents Objects

Babel compiles JSX down to React.createElement() calls.

These two examples are identical:

```jsx
const element = (
  <h1 className="greeting">
    Hello, world!
  </h1>
);
```

```js
const element = React.createElement(
  'h1',
  {className: 'greeting'},
  'Hello, world!'
);
```

## Rendering Elements

### Rendering an Element into the DOM

Let’s say there is a `<div>` somewhere in your HTML file:

```html
<div id="root"></div>
```

To render a React element into a root DOM node, pass both to `ReactDOM.render()`:

```jsx
const element = <h1>Hello, world</h1>;
ReactDOM.render(element, document.getElementById('root'));
```

It displays “Hello, world” on the page.



### Updating the Rendered Element

```jsx
function tick() {
  const element = (
    <div>
      <h1>Hello, world!</h1>
      <h2>It is {new Date().toLocaleTimeString()}.</h2>
    </div>
  );
  ReactDOM.render(
    element,
    document.getElementById('root')
  );
}

setInterval(tick, 1000);
```

### React Only Updates What’s Necessary
React DOM compares the element and its children to the previous one, and only applies the DOM updates necessary to bring the DOM to the desired state.


