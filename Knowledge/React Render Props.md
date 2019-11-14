# Render Props

- A technique for sharing code between React components **using a prop whose value is a function**
```jsx
<Component render={data >(
	<h1>Hello {data.name}</h1>
)}
```
## Use Render Props for Cross-Cutting Concerns
If `<Animal>` has some states and methods that `<Bird>` needs, we can hard code like this:
```jsx
class Bird extends React.Component{
	render(){
		const need = this.props.need
		return(
			...
		)
	}
}
class Animal extends React.Component{
	constructor(props){
		super(props);
		this.state = {
			need: 'food'
		}
	}
	...
	render(){
		return(
			<div>
				<Bird need={this.state.need}/>
			</div>
		)
	}
}
```
As we see, `<Bird>` was embeded in `<Animal/>` what make the `<Animal/>`, which has the states(or methods) should be shared between many components, hard to reuse.
Use **Render Props**, we can refactory the code above:
```jsx
class Bird extends React.Component{
	render(){
		const need = this.props.state.need	// +
		return(
			...
		)
	}
}
class Animal extends React.Component{
	constructor(props){
		super(props);
		this.state = {
			need: 'food'
		}
	}
	...
	render(){
		return(
			<div>
				{this.props.render(this.state)}	// diff
			</div>
		)
	}
}

//now <Animal/> is reusable, like this:
<Animal render={data => (
	<Bird state={data}/>
)}/>

//We can also create:
<Animal render={data => (
	<Dog state={data}/>
)}>

//Even in these ways:
// 1.
<Animal anyName={data => (
	<Cat state={data}/>
)}>
//Accordingly, in <Animal/>:
{this.props.anyName(this.state)}

// 2.
<Animal>
	{state => (
		<p>{this.state.need}</p>
	)}
</Animal>
...
Animal.propTypes = {
  children: PropTypes.func.isRequired
};
```

> 💡Using a render prop can negate the advantage that comes from using [`React.PureComponent`](https://reactjs.org/docs/react-api.html#reactpurecomponent) **if you create the function inside a `render` method**. This is because **the shallow prop comparison will always return `false` for new props, and each `render` in this case will generate a new value for the render prop**.

Solution:
```jsx
class Animal extends React.Component {
  // Defined as an instance method, `this.renderTheCat` always
  // refers to *same* function when we use it in render
  renderTheCat(mouse) {
    return <Cat mouse={mouse} />;
  }

  render() {
    return (
      <div>
        <h1>Move the mouse around!</h1>
        <Mouse render={this.renderTheCat} />  🤔
      </div>
    );
  }
}
```
In cases where you cannot define the prop statically (e.g. because you need to close over the component’s props and/or state) `<Animal>` should extend `React.Component` instead.
