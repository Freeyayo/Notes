## React Context

### Mainly Usage
- Share values between components without explictly pass prop(s) through every level of tree.
	> We can use it in these situation:
	> - current authenticated user
	> - theme
	> - prefered language 
	...
	
Context is primarily used when some data needs to be accessible by _many_ components at different nesting levels. Apply it sparingly because **_it makes component reuse more difficult._**

### API
-  **React.createContext**
```js
const MyContext = React.createContext(defaultValue);
```
>Passing  `undefined`  as a Provider value does not cause consuming components to use  `defaultValue`.

-  **Context.Provider**
```js
<MyContext.Provider value={/* some value */}>
```
All consumers that are *descendants* of a Provider will **re-render whenever the Provider’s  `value`  prop changes**. The propagation from Provider to its descendant consumers is **not subject to the  `shouldComponentUpdate`  method**, so the **consumer is updated even when an ancestor component bails out of the update.**

>Changes are determined by comparing the new and old values using the same algorithm as  `Object.is`

- **Class.contextType**

```js
class MyClass extends React.Component {
  componentDidMount() {
    let value = this.context;
    /* perform a side-effect at mount using the value of MyContext */
  }
  componentDidUpdate() {
    let value = this.context;
    /* ... */
  }
  componentWillUnmount() {
    let value = this.context;
    /* ... */
  }
  render() {
    let value = this.context;
    /* render something based on the value of MyContext */
  }
}
MyClass.contextType = MyContext;
```

This lets you **consume the nearest current value of that Context type** using `this.context`
> - You can only subscribe to a single context using this API
> - You can use a **static** class field to initialize your `contextType`.

- **Context.Consumer**

```js
<MyContext.Consumer>
  {value => /* render something based on the context value */}
</MyContext.Consumer>
```
This is a *React component* that **subscribes** to context changes. It lets you subscribe to a context within a **`function component`**.
> The `value` argument passed to the function will be equal to the `value` prop of the **closest Provider** for this context above in the tree.

### Example

- **Consuming Multiple Contexts**

To keep context re-rendering fast, React needs to make each context consumer a **separate node** in the tree.

```js
// Theme context, default to light theme
const ThemeContext = React.createContext('light');

// Signed-in user context
const UserContext = React.createContext({
  name: 'Guest',
});

class App extends React.Component {
  render() {
    const {signedInUser, theme} = this.props;

    // App component that provides initial context values
    return (
      <ThemeContext.Provider value={theme}>
        <UserContext.Provider value={signedInUser}>
          <Layout />
        </UserContext.Provider>
      </ThemeContext.Provider>
    );
  }
}

function Layout() {
  return (
    <div>
      <Sidebar />
      <Content />
    </div>
  );
}

// A component may consume multiple contexts
function Content() {
  return (
    <ThemeContext.Consumer>
      {theme => (
        <UserContext.Consumer>
          {user => (
            <ProfilePage user={user} theme={theme} />
          )}
        </UserContext.Consumer>
      )}
    </ThemeContext.Consumer>
  );
}
```

>If two or more context values are often used together, you might want to consider creating your own `render prop `component that provides both.


### Caveats

Because **context uses reference identity to determine when to re-render**, there are some gotchas that could trigger unintentional renders in consumers when a provider’s parent re-renders. For example, the code below will **re-render all consumers every time the Provider re-renders because a new object is always created for  `value`**:

```js
class App extends React.Component {
  render() {
    return (
      <Provider value={{something: 'something'}}>
        <Toolbar />
      </Provider>
    );
  }
}
```

To get around this, **lift the value into the parent’s state**:

```js
class App extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      value: {something: 'something'},
    };
  }

  render() {
    return (
      <Provider value={this.state.value}>
        <Toolbar />
      </Provider>
    );
  }
}
```
