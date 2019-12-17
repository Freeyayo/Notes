# Expand the Full Width of Submenu

## Description
We bought a custom theme as the skin of the original one, and we expect at least 90% similarity between the duplicated version and original site.
- Original site has a quite complicated DOM structure and lots of weird class names. 
- The theme we bought that seems introduced some UI plugins that add class names and **inline style** property dynamically when mouse hovers on the corresponding items, so it's impossible to change a certain style through modifying CSS.

## Code

```js
const checkDomByClass = (dom,callback) => {
    let node = null;
    const checking = setInterval(function(){
        node = document.querySelector(dom);
        if(node){
            clearInterval(checking);
            callback();
        }
    },500)
}
//JavaScript file will be loaded ahead of page's rendering, so we need to keep checking if specific node is mounted
checkDomByClass(".ui-menu.ui-widget.ui-widget-content.ui-corner-all", resetSubmenu)

function resetSubmenu(){
	//Key point: I noticed that plugin(jquery-ui or something else) would change nodes' class names then do something with jquery.I think the most important step is to stop plugins from modifying class names.
	//I can't find a way to prevent modification on HTMLDom node for remaining nodes' class names unchanged
	//The essential mechanism of changing class name in jquery is to use setAttribute(), so I try to make this method a null pointer that will ensure modification on class won't work. 
	const NULL_REFERENCE = new Function;
	const ROOT = '.level0.submenu.sectioned.ui-menu.ui-widget.ui-widget-content.ui-corner-all';
	const BAR = '.ui-menu.ui-widget.ui-widget-content.ui-corner-all';
	const bar = document.querySelector(BAR);
	const subs = document.querySelectorAll(ROOT);
	const styleNode = (node, styles) => {
        for(prop in styles){
            node.style[prop] = styles[prop];
        }
    }

    const attachListener = (node, [type, func, opt]) => {
    	node.addEventListener(type, func, opt);
    	return attachListener.bind(null, node);
    }
	//In order to pass arguments to callback in listerner, I use closure
	const enter = el => {
		return e => {
		//Preventing other events bound on the children 
			e.stopPropagation();
			e.preventDefault();
			styleNode(el, {
				//Obtaining current data dynamically
				left: `${bar.offsetLeft}px`,
				width: `${bar.offsetWidth}px`,
				top: '44px',
				display: 'block'
			});
		}
	}

	const out = el => e => styleNode(el, {display: 'none'});

	for(let i=0;i<subs.length;i++){
		const sub = subs[i];
		const prevS = sub.previousSibling;
		const parent = sub.parentElement;
		prevS.setAttribute = NULL_REFERENCE;
		prevS.removeAttribute = NULL_REFERENCE;

		attachListener(parent,['mouseenter', enter(sub), false])
					         (['mousemove',enter(sub),false])
					         (['mouseover',enter(sub),false])
					         (['mouseout',out(sub),false]);
	}
}
```
