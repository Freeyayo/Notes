# Scraping Information on Pages

## Description
There're 4 different languages can be choosen for Ashford web pages. 
- need text content of every language
- configuration structure: "lan1":"lan2"

## Tech & Tools
- Puppeteer
- Node fs module
- Node arguements ( process.argv )

## Code
```javascript
const puppeteer = require('puppeteer');  
const fs = require('fs');  
  
const URL1 = process.argv[2];  
const URL2 = process.argv[3];  
const selector = process.argv[4];  
  
function output(a1, a2){  
    if(a1.length !== a2.length){  
        new Error('not equal')  
    }  
    const length = a1.length;  
    const table = {};  
    for(let i = 0; i < length; i++){  
        table[a1[i]] = a2[i]  
    }  
    return table;  
}  
  
let table = null;  
let tableLan = null;  
  
const p1 = puppeteer.launch().then(async browser => {  
    const page = await browser.newPage();  
    await page.goto(URL1,{waitUntil: 'domcontentloaded'});  
    page.on('console', msg => console.log(msg.text()));  
    await page.exposeFunction('getData', data => table = data );  
    await page.exposeFunction('setNode', () => selector );  
    const result = await page.evaluateHandle(async () => {  
        function walk(node) {  
            let root = node;  
            const textNodes = [];  
            for(let i = 0; i < root.childNodes.length; i++){  
                const child = root.childNodes[i];  
                if(child.nodeType === 1 && child.nodeName !== 'SCRIPT' && child.nodeName !== 'STYLE' && child.nodeName !== 'NOSCRIPT'){  
                    textNodes.push(child.innerText)  
                }else if(child.nodeType === 3){  
                    walk(child)  
                }  
            }  
            return textNodes;  
        }  
  
        function format(arr){  
            return arr.map(item => item.split('\n')).flat(Infinity)  
        }  
  
        function filter(arr){  
            return arr.filter(item => item !== '')  
        }  
  
        function compose(f, g, h){  
            return function(x){  
                return f(g(h(x)))  
            }  
        }  
  
        const c = compose(filter, format, walk)  
        const selector = await setNode();  
        const node = await document.querySelector(selector);  
        const data = await c(node);  
        getData(data);  
    });  
    await browser.close();  
    return new Promise((resolve,reject) => {  
        resolve(table)  
    })  
});  
  
  
const p2 = puppeteer.launch().then(async browser => {  
    const page = await browser.newPage();  
    await page.goto(URL2,{waitUntil: 'domcontentloaded'});  
    page.on('console', msg => console.log(msg.text()));  
    await page.exposeFunction('getData', data => tableLan = data );  
    await page.exposeFunction('setNode', () => selector );  
    const result2 = await page.evaluateHandle(async () => {  
        function walk(node) {  
            let root = node;  
            const textNodes = [];  
            for(let i = 0; i < root.childNodes.length; i++){  
                const child = root.childNodes[i];  
                if(child.nodeType === 1 && child.nodeName !== 'SCRIPT' && child.nodeName !== 'STYLE' && child.nodeName !== 'NOSCRIPT'){  
                    textNodes.push(child.innerText)  
                }else if(child.nodeType === 3){  
                    walk(child)  
                }  
            }  
            return textNodes;  
        }  
  
        function format(arr){  
            return arr.map(item => item.split('\n')).flat(Infinity)  
        }  
  
        function filter(arr){  
            return arr.filter(item => item !== '')  
        }  
  
        function compose(f, g, h){  
            return function(x){  
                return f(g(h(x)))  
            }  
        }  
  
        const c = compose(filter, format, walk)  
        const selector = await setNode();  
        const node = await document.querySelector(selector);  
        const data = await c(node);  
        getData(data);  
    });  
    await browser.close();  
    return new Promise((resolve,reject) => {  
        resolve(tableLan)  
    })  
});  
  
Promise.all([p1, p2]).then(result => {  
    const data = Buffer.from(JSON.stringify(output(result[0], result[1])));  
    fs.writeFile(`./outputs/translation${new Date().getTime()}.txt`, data, (err) => {  
        if (err) throw err;  
        console.log('file saved');  
    });  
    console.log(output(result[0], result[1]))  
});
```

## Problems

- Use `Puppeteer.launch()` is slow, maybe should try `Puppeteer.connect()`.Slowly connection may cause timeout 30000ms limitation.
- I defined functions outside the headless instance. With `page.exposeFunction()` , function will works inside the web incetance. But if there's another funtion in the outter one, will occur some errors, so I just copy all of them into web instance.
- The data I need is fetched in an asynchronous way and I want fetch data in both instances, so I create two variables `table` `tablelan` .  If return them in a `Promise.resolve`, I can use `Promise.all([promise1, promise2])` to get them in one time.
