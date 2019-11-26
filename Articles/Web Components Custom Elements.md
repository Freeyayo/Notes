## An Introduction to Custom Elements

> The `window` global object exposes a `customElements` property that gives us access to a `CustomElementRegistry` object

### The `CustomElementRegistry` object's methods 🔖:
- `define()` : Define a new Custom Element 
- `get()` : Get the constructor of a Custom Element (return `undefined` if not existing)
- `upgrade()` : Upgrade a Custom Element
- `whenDefined()` : Similarly to `get()` but it returns a **promise** instead, which resolves when the item is available

### How to Create a Custom Element

1. Define a new HTML element by creating a new **class that extends the HTMLElement** built-in class 
```js
class CustomTitle extends HTMLElement {
	constructor(){
		super()
		//...
	}
	//...
}
```
2. Use **Shadow DOM** to associate custom CSS, JavaScript, HTML to our new tag
```js
class CustomTitle extends HTMLElement {
  constructor() {
    super()
    //attachShadow is from HTMLElement
    //if it's open we can access the shadowRoot property of an element
    this.attachShadow({ mode: 'open' })	
    //...
  }
}
```
3. Set the `innerHTML` of it
```js
class CustomTitle extends HTMLElement {
  constructor() {
    super()
    this.attachShadow({ mode: 'open' })
    this.shadowRoot.innerHTML = `
      <h1>My Custom Title!</h1>
    `
  }
}
```

4. Add this newly defined element to `window.customElements` :
```js
window.customElements.define('custom-title', CustomTitle)
```
>- Note: you can’t use self-closing tags (in other words: this `<custom-title />` is not allowed by the standard)
>
>- `-` dash can help us to detect built-in tag from a custom one

### Provide a Custom CSS for the Element
```js
class CustomTitle extends HTMLElement {
  constructor() {
    super()
    this.attachShadow({ mode: 'open' })
    this.shadowRoot.innerHTML = `
      <style>
        h1 {
          font-size: 7rem;
          color: #000;
          font-family: Helvetica;
          text-align: center;
        }
      </style>
      <h1>My Custom Title!</h1>
    `
  }
}
```

### A Shorter Syntax
```js
window.customElements.define('custom-title', class extends HTMLElement {
  constructor() {
    ...
  }
})
```

### Add JavaScript
Define a click event in the Custom Element constructor : 
```js
class CustomTitle extends HTMLElement {
  constructor() {
    super()
    this.attachShadow({ mode: 'open' })
    this.shadowRoot.innerHTML = `
      <h1>My Custom Title!</h1>
    `
    this.addEventListener('click', e => {
      alert('clicked!')
    })
  }
}
```

### Alternative: Use Templates
Define its template in the HTML
```html
<template id="custom-title-template">
  <style>
    h1 {
      font-size: 7rem;
      color: #000;
      font-family: Helvetica;
      text-align: center;
    }
  </style>
  <h1>My Custom Title!</h1>
</template>

<custom-title></custom-title>
```
Reference it in Custom Element and add it to the shadow DOM:
```js
class CustomTitle extends HTMLElement {
  constructor() {
    super()
    this.attachShadow({ mode: 'open' })
    const tmpl = document.querySelector('#custom-title-template')
    this.shadowRoot.appendChild(tmpl.content.cloneNode(true))
  }
}

window.customElements.define('custom-title', CustomTitle)
```

### Lifecycle Hooks
-   `connectedCallback`  when the element is **inserted** into the DOM
-   `disconnectedCallback`  when the element is **removed** from the DOM
-   `attributeChangedCallback`  when an  **_observed_  attribute changed**, or is added or removed 
-   `adoptedCallback`  when the element has been  [moved to a new document](https://developer.mozilla.org/en-US/docs/Web/API/Document/adoptNode)

#### what is  _observed_  attribute 🥺?
> We must define them in an array returned by the `observedAttributes` **static method**:
```js
class CustomTitle extends HTMLElement {
  ...
  😊
  static get observedAttributes() {
    return ['disabled']
  }

  attributeChangedCallback(attrName, oldVal, newVal) {
    ...
  }
}
```

This action will trigger the ` attributeChangedCallback()`
```js
document.querySelector('custom-title').disabled = true
```

### Define Custom Attributes

You can add custom attributes by **adding getter and setter** for them
```js
class CustomTitle extends HTMLElement {
  static get observedAttributes() {
    return ['mycoolattribute']
  }

  get mycoolattribute() {
    return this.getAttribute('mycoolattribute')
  }

  set mycoolattribute(value) {
    this.setAttribute('mycoolattribute', value)
  }
}
```

> You can use this [polyfill](https://github.com/webcomponents/polyfills/tree/master/packages/custom-elements) to add a better support also for older browsers.