## Forwarding Ref

- It's a technique for automatically pass a `ref` **through a component to one of its child**
- It can be useful for some kinds of components, especially in reusable **component libraries**. 

### Code:
In three using cases below, we can set `ref` on components as same as on the DOM component
```jsx
<FunctionalChild ref={this.functionalChildRef} />

<ClassChild ref={this.classChildRef} />

<Wrapper ref={this.hocChildRef}/>
```

- In `React` there're two special `props`:
		-  `key`
		-  `ref`
	They won't be got by argument props

### In Functional Component:
Once we **1**.wrap functional component in `React.forwardRef` and **2**.pass an additional argument `ref` to it, this component can be set `ref` as a DOM component as shown above.
```jsx
function FunctionalChild(props, ref){ 👈
	return(
		<>
			<p>Function</p>
			<input type='text' ref={ref}/> 👈
		</>
	)
}
export default React.forwardRef(FunctionalChild)
```

### In Class Component:
The only **difference** between Class and Functional component is the exporting step :
```jsx
export default React.forwardRef((props, ref)  =>  {
	return  <ClassChild classRef={ref}/>
});
```
⛔ We can't pass extra arguments to Class component and regular Functional component, so the only way forwarding `ref` to Class' child is that setting a middle **custom ref** on the Class component. Inside the component, just pass **custom ref** to `ref` 
```jsx
return(
	<>
		<p>Class</p>
		<input type='text' ref={this.props.classRef}/>
	</>
)
``` 

### In HOC:
Returning `WrappedComponent` by `React.forwardRef` Similar as `Class` we past **custom ref** through nested components until we reach the destination. The final one receives the **custom ref** as its `ref`
```jsx
import React from 'react';

function  FancyInput(props){
	return(
			<>
				<p>HOC</p>
				<input ref={props.forwardRef} type='text' />  ✌🏻
			</>
		)
}

function  Wrapper(Component){

	class  WrappedComponent  extends  React.Component{
		constructor(props){
			super(props)
		}

		render(){
			const {forwardRef,  ...rest} = this.props;
			return(
			☁️🚁	<Component forwardRef={forwardRef}  {...rest}/> 
				)
		}
	}
	
	return  React.forwardRef((props, ref)  =>  {
  ☁️🚁	return  <WrappedComponent forwardRef={ref}  {...props} />
	})	
	
}
export default Wrapper(FancyInput);
```

[Source Code](https://stackblitz.com/edit/react-forwardingref)