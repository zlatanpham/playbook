# Best practices in Frontend development

## React

We use React for most projects, so this guide mostly leans toward best practices for React and libraries that use (or work well with) React.

## Project structure

We use the container/component architecture. containers/ contains React components which are connected to the redux store. components/ contains dumb React components which depend on containers for data. Container components care about how things work, while components care about how things look.

We've found that for many applications treating single pages (e.g. the LoginPage, the HomePage, etc.) as containers and their small parts (e.g. the Login form, the Navigation bar) as components works well, but there are no rigid rules. Bend the architecture to the needs of your app, nothing is set in stone!

```
app/
├── components/
├── containers/
├── translation/
├── styles/
└── utils/
```

## React

- Use hooks instead of classes whenever possible.
- Multipurpose Function Component

  If the component is purely presentational and does not require state and life cycle maintenance, the Function Component is preferred. It has the following benefits:

  1. The code is more concise, at a glance, it is purely display-type, without complex business logic
  2. Better reusability. As long as props of the same structure are passed in, the same interface can be displayed without the side effects.
  3. Smaller package size, higher execution efficiency

- Use PureComponent to avoid wasteful renders
- Avoid dynamically creating objects / methods in render, otherwise it will cause child components to render every time
  ```jsx
  // Bad
  render() {
    return(
        <Child obj={{num: 1}} onClick={()=>{...}} />
    );
  }
  -
  ```
- `useMemo` and `useCallback` to avoid expensive calculations on every render

  ```jsx
  import React, { useMemo, useCallback } from 'react';

  // Performance optimizing via useMemo()
  const ParentComponent = props => (
    <div>
      {/* Only re-renders if "a" change */}
      {useMemo(
        () => (
          <ChildComponent someProp={a} />
        ),
        [a],
      )}
    </div>
  );

  // Performance optimizing via useCallback()
  const ParentComponent = props => (
    <div>
      {/* Return a memorized callback that only changes if "a" changed */}
      {/* This is useful to prevent child component from unnecessary renders */}
      <ChildComponent
        callback={useCallback(() => {
          doSomething(a);
        }, [a])}
      />
    </div>
  );
  ```

- Use destructuring more often, such as props for Function Component
  ```jsx
  const MyComponent = ({ title, desc, onClick, ...rest}) => {
      return (
          ....
      );
  };
  ```
  - Use ErrorBoundary to have errors and crash in peace without causing an entire application-wide break

## Redux

## JavaScript

- Never use `var`
- Prefer `forEach`/`for/of` over `for (let i = 0; i < items.length; i++)`
- Prefer functional & immutable array methods .ie `map`/`filter`/`reduce`/`some`/`every` over any types of mutable `for` loop
- Prefer "return early" coding style, more about it [here](https://medium.com/@matryer/line-of-sight-in-code-186dd7cdea88)
- Don't obsess over performance of code, obsess over making it clear

## Styling

## Linting

## Code Splitting

Modern sites often combine all of their JavaScript into a single, large bundle. When JavaScript is served this way, loading performance suffers. Large amounts of JavaScript can also tie up the main thread, delaying interactivity. This is especially true of devices with less memory and processing power especially in routing rich applications like ours. To address the issue, we adopt code-splitting in order to split one large bundle into smaller chunks. While the main is loaded upfront, the rest can be loaded on demand. If you are unsure where to begin to applying code-splitting to our applications, follow these steps:

- Begin at route level. Routes are the simplest way to identify points of your application that can be split. Use React [`suspend`](https://reactjs.org/docs/code-splitting.html#route-based-code-splitting) to load the page chunk when needed.
- Use [`dynamic import`](https://webpack.js.org/guides/code-splitting/#dynamic-imports) to split any large module if that is offscreen and not critical for the user (like parts only rendered on certain user interactions like clicking a button).
- When using large libary like lodash or ant design, use [`babel-plugin-import`](https://github.com/ant-design/babel-plugin-import) to avoid pulling in unused modules while maintaining best Developer Experience.

## Testing

## Continous integration & deployment

Use [Netlify](https://www.netlify.com/) for web applications and static websites, it handles both CI and deployment for free, which is awesome.

## Monitoring

Use [Sentry](https://sentry.io/) to monitor errors and centralized stacktrace in production.
