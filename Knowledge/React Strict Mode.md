# React Strict Mode

## What is Strict Mode

 - **Detect potential problems** in your code and gives you helpful warnings
 - **Run in development mode** only and doesn't affect your production build
 
 ###  Deprecated code and legacy APIs
 - `findDOMNode`
 - string refs 
 - legacy context API
 - ...
 
 ### Unsafe lifecycle methods
 - `componentWillMount`
 -  `componentWillReceiveProps` 
 -  `componentWillUpdate`

Strict Mode can help you identify unsafe lifecycle methods in your own code and third-party libraries, and also suggests alternative methods.

### Unexpected side effects
🤔
Strict Mode can't detect these side effects automatically, but uses a simple yet clever trick to make them easier to spot – the methods `constructor`, `render`, `setState` and `getDerivedStateFromProps` all get double invoked. If this leads to any weird behavior in your app, you know what to look for.

## How to Use
```jsx
//entire app
<React.StrictMode>
  <App>
    <Component1 />
    <Component2 />
  </App>
</React.StrictMode>


//single component
<App>
  <React.StrictMode>
    <Component1 />
  </React.StrictMode>
  <Component2 />
</App>
```