# Refs and the Dom

- We can use Refs to access :
   1. DOM nodes 
  2. React element

   in the `render()` method
- Compare with `props`, props are the way that parent components interact with their chidren: Modify a child, you re-render it with new props. In this case, all is happened on the typical React dataflow (states, methods...). What if child is a DOM element?

### Creating Refs

Refs are created using  `React.createRef()`  and attached to React elements via the  `ref`  attribute. Refs are commonly assigned to an instance property when a component is constructed so **they can be referenced throughout the component**.

```jsx
class MyComponent extends React.Component {
  constructor(props) {
    super(props);
    this.myRef = React.createRef();
  }
  render() {
    return <div ref={this.myRef} />;
  }
}
```

### Accessing Refs

When a ref is passed to an element in  `render`, a reference to the node becomes accessible at the  `current`  attribute of the ref.

```jsx
const node = this.myRef.current;
```
**The value of the ref differs depending on the type of the node**:

- **DOM**:  When the  `ref`  attribute is used on an HTML element, the  `ref`  created in the constructor with  `React.createRef()`  receives the underlying DOM element as its  `current`  property.
-  **React Component** When the  `ref`  attribute is used on a custom class component, the  `ref`  object receives the mounted instance of the component as its  `current`.
-   🤚**Function Component**You **may not** use the  `ref`  attribute on function components  **because they don’t have instances**.You should convert the component to a class if you need a ref to it, just like you do when you need lifecycle methods or state.

### 💡Example about Adding a Refs to A Class Component:
```jsx
class AutoFocusTextInput extends React.Component {
  constructor(props) {
    super(props);
    this.textInput = React.createRef();
  }

  componentDidMount() {
    this.textInput.current.focusTextInput();	// use refs here
  }

  render() {
    return (
      <CustomTextInput ref={this.textInput} />  // 😗 textInput is component event
    );
  }
}
```
> Ref in the `<CustomTextInput />` above,can be any other name **rather than ref** because it's just a `prop` which can be passed to descendants
> In React 16.2 and earlier we can do this as a replacement of `Forwarding Refs`
## ⚠️Adding a Refs to A Class Component:
$\color{Crimson}{This\ won't\ work :}$
```jsx
function MyFunctionComponent() {
  return <input />;
}

class Parent extends React.Component {
  constructor(props) {
    super(props);
    this.textInput = React.createRef();
  }
  render() {
    // ⚠️ This will *not* work! 
    return (
      <MyFunctionComponent ref={this.textInput} />
    );
  }
}
```
>😌You can, however, **use the  `ref`  attribute inside a function component** as long as you refer to a DOM element or a class component
>You should convert the component to a class if you need a ref to it, just like you do when you need lifecycle methods or state.

**example** : *use child ref in grandparent component ( pass in props )*
```jsx 
const Parent = props => <div><Child inputRef={props.inputRef}/></div>

const Child = props => <div><input type='text' inputRef={props.inputRef}/></div>

class GrandParent extends Component{
	constructor(props){
	    super(props)
		this.childRef = React.createRef();
	}
	componentDidMount(){
		const childInput = this.childRef.current;
		// do something with childInput here 🤓
	}
	render(){
		return(
			<div>
				<Parent inputRef={this.childRef}/>
			</div>
		)
	}
}
```


## Callback Refs
Instead of pass an object created by `React.createRef()`, we pass a **function**
The function receives the **React component instance** or **HTML DOM element** as its argument, *which can be stored and accessed elsewhere*
```jsx
class CustomTextInput extends React.Component {
  constructor(props) {
    super(props);
    this.textInput = null;
    this.setTextInputRef = element => {
      this.textInput = element;
    };

    this.focusTextInput = () => {
      // Focus the text input using the raw DOM API
      if (this.textInput) this.textInput.focus();
    };
  }
  componentDidMount() {
    // autofocus the input on mount
    this.focusTextInput();
  }
  render() {
    // Use the `ref` callback to store a reference to the text input DOM
    // element in an instance field (for example, this.textInput).
    return (
      <div>
        <input
          type="text"
          ref={this.setTextInputRef}
        />
        <input
          type="button"
          value="Focus the text input"
          onClick={this.focusTextInput}
        />
      </div>
    );
  }
}
```

- 💡 Refs are guaranteed to be up-to-date before `componentDidMount` or `componentDidUpdate` fires.
- 💡 React will call the `ref` callback with the DOM element when the component mounts
- 💡 React will call the `ref` with `null` when the component unmounts

You can **pass callback refs between components like you can with object refs** that were created with `React.createRef()`.
```jsx
function CustomTextInput(props) {
  return (
    <div>
      <input ref={props.inputRef} />
    </div>
  );
}

class Parent extends React.Component {
  render() {
    return (
      <CustomTextInput
        inputRef={el => this.inputElement = el} // pass Ref Callback to child
      />
    );
  }
}
```
>## 💩💩💩
>When define a Ref Callback as an inline function, it will get called **twice** during updates, first with `null` and then again with the DOM element.This is because **a new instance of the function is created with each render, so React needs to clear the old ref and set up the new one**. You can avoid this by defining the `ref` callback as a bound method on the class, but note that it shouldn’t matter in most cases.