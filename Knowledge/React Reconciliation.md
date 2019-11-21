## Reconciliation

React implements a heuristic O(n) algorithm based on **two assumptions**:
1.  Two elements of different types will produce different trees. 
2.  The developer can hint at which child elements may be stable across different renders with a `key` prop.

### The Diffing Algorithm
When diffing two trees, React first compares the two root elements:

-  👍 Elements Of Different Types :
     **root elements have different types, tear down the old tree and build the new tree**
When tearing down a tree, old DOM nodes are destroyed. Component instances receive `componentWillUnmount()`. When building up a new tree, new DOM nodes are inserted into the DOM. Component instances receive `componentWillMount()` and then `componentDidMount()`. Any components below the root will also **get unmounted and have their state destroyed**.
     
-  👍 DOM Elements Of The Same Type
Keeping the same underlying DOM node, and **only updates the changed attributes**.
When updating  `style`, React also knows to update only the properties that changed. For example:
```jsx
<div style={{color: 'red', fontWeight: 'bold'}} />
<div style={{color: 'green', fontWeight: 'bold'}} />
```

After handling the DOM node, React then recurses on the children.
     
   -   👍 Component Elements Of The Same Type
   1. When a component updates, **the instance stays the same,** so that state is maintained across renders. React updates the props of the underlying component instance to match the new element, and calls  `componentWillReceiveProps()`  and  `componentWillUpdate()`  on the underlying instance.
2. **Next, the  `render()`  method is called** and the diff algorithm recurses on the previous result and the new result.
-  👍 Recursing On Children
**By default**, when recursing on the children of a DOM node, React just iterates over both lists of children **at the same time** and generates a mutation whenever there’s a difference.
Without `key` :
1. works well
```jsx
<ul>
  <li>first</li>
  <li>second</li>
</ul>

<ul>
  <li>first</li>
  <li>second</li>
  <li>third</li>
</ul>
```

2.  works poorly
 ```jsx
<ul>
  <li>Duke</li>
  <li>Villanova</li>
</ul>

<ul>
  <li>Connecticut</li>
  <li>Duke</li>
  <li>Villanova</li>
</ul>
```

### keys
- **React uses the key to match children in the original tree with children in the subsequent tree**
- The key only has to be unique **among its siblings**, not globally unique
- Use `index` as key can work well if the items are never reordered

Rerender in this context means calling `render` for all components, it doesn’t mean React will unmount and remount them. It will **only** apply the differences following the rules stated in the previous sections.
React relies on heuristics, if the assumptions behind them are not met, performance will suffer.

1 . Try to keep components the same type as possible in alternating if the output is similar
2 . Keys should be stable, predictable, and unique  