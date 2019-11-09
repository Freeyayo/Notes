# Hide or Show Content Depending on Text Length

## Description
We want to display the description about product on PDP page
- If description's text length is too big, just show an abbreviation
- A link should follow the text to duty the ability Read More and Hide when clicked

## Code

- style.scss
```scss
#content{
	border: 1px solid black;
	width: 200px;
	height: 100px;
	
	#format_link{
	display: inline-block;
	}
	#format_content{
		display: inline;
		word-break: break-all; /*text will shift to new line when reach the border of box*/
	}
}
```

- index.ts
```ts
import '../css/style.scss';
//node : The node you want to fill the text
//text : Text content
//limit : Initial text length you want to show when page is loaded
function format(node:HTMLElement, text: string, limit: number): void{

	let isShown:boolean = false; 
	const fullText = text;
	const limitedText = limit < text.length ? `${text.slice(0,limit)}...` : fullText;//if too many words, show limited text

	node.innerHTML = `<div id='format_content'></div><a href='#' id='format_link'></a>`; //create nodes before declared 👇

	//using type assertion to tell ts they have HTMLElement methods otherwise innerText will not gonna work
	const content = <HTMLElement>document.querySelector('#format_content');
	const link = <HTMLElement>document.querySelector('#format_link');

	content.innerText = limitedText;
	link.innerText = limit < text.length ? 'show' : '';//this code decides the display of link at the beginning

	//toggle handler 👇
	const displayHandler = ():void => {
		content.innerText = isShown ? limitedText : fullText;
		link.innerText = isShown ? 'show' : 'hide';
		isShown = !isShown;
	}

	link.addEventListener('click',displayHandler,false)

}

format(document.querySelector('#content'),'asdjkdfjkhdfdczxczxczxczxczxczxcxzcxczczas',16);
```

## Problem
- I think JS part is NOT elegant enough but it does work