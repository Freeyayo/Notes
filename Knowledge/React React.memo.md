## React.memo

### What is React.memo 🧠
 - It's straightforward as it is exactly similar to `React.PureComponent`
  `React.PureComponent` <=> class component
  `React.memo` <=> functional component

### Example:
Tip here: 🙃 Don't bind DOM events to custom component, bind them on the DOM. 
```jsx
import React ,{ useState } from 'react';
import Counter from './Counter';

function Table(){
	console.log('table is rendered')
	const [count1, setCount1] = useState(0);
	const [count2, setCount2] = useState(0);
	const increaseCounter1 = () => {	
	setCount1(count1 => count1 +  1)
}
	return(
			<>
				<Counter value={count1} name='C1'/>
				<Counter value={count2} name='C2'/>
				-------------------------<br/>
				<div onClick={increaseCounter1}  >This is Table</div> 
			</>
		)
}

export default ({ name }) => <h1><br/><Table /></h1>;


//Counter.jsx
import React from 'react';

function Counter({value, name}){
	console.log('counter is rendered')
	return(
			<>
				counter=><br/>
				now {name} is: {value}<br/>
			</>
		)
}

export default Counter;
```
Here I created two `counters`, and when only bound `increaseCounter1` to the `<Table/>` to update `counter1`'s value. But when click the `<Table/>`, React will render `<Counter/>` **twice** and `<Table/>` once. Because `<Counter/>` is the child of  `<Table/>`, once `<Counter/>` has an update, `<Table/>` will be updated as well. It's inevitable, ☹️ but the problem is another `<Counter />` has no change but still be updated. 
To **cut off redundant rendering**, here comes `React.memo` :
```jsx
export default React.memo(Counter);
```
By default, `React.memo` will compare **all** props passed to the component by **referential equality**

😁 Now we redo the previous operation, there's only one rendering information logs out in the console.

✏️ We can go further:
```jsx
const areEqual = (prevProps, nextProps) => {
	console.log(prevProps,nextProps);
	return true;
}
export default React.memo(Counter,areEqual);


//outputs 🔻

//click 1st time
//table is rendered
//{value: 0, name: "C1"} {value: 1, name: "C1"}
//{value: 0, name: "C2"} {value: 0, name: "C2"}

//click 2nd time
//table is rendered
//{value: 0, name: "C1"} {value: 2, name: "C1"}
//{value: 0, name: "C2"} {value: 0, name: "C2"}

//click 3rd time
//table is rendered
//{value: 0, name: "C1"} {value: 3, name: "C1"}
//{value: 0, name: "C2"} {value: 0, name: "C2"}
```

UI will never change because I always return `true` from a **compare function** that is passed to the `React.memo` as it's second argument

Notice that every time after click, `prevProps` remains `value: 0` meanwhile `nextProps` increases. The reason is `prevProps` **memorized** the previous props which never be changed by our `areEqual` function.