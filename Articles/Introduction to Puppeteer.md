# Introduction to Puppeteer
**Programmatically controlling Chrome from Node.js**
- - - -
## What is Puppeteer ?
Puppeteer is a Node library that we can use to control a  [headless Chrome](https://developers.google.com/web/updates/2017/04/headless-chrome)  instance. We are basically using Chrome, but programmatically using JavaScript. It’s built by Google.

**Using it, we can:**
* scrape web pages
* automate form submissions
* perform any kind of browser automation
* track page loading performance
* create server-side rendered versions of single page apps
* make screenshots
* create automating testing
* generate PDF from web pages

It’s using the actual browser under the hood

## How to use it ?
* we can use the `const browser = await puppeteer.launch()` method to **create a browser instance**

* `puppeteer.launch({ headless:false })`  to show Chrome while Puppeteer is performing its operations

* we can use the `newPage()` method on the browser object to **get the page object**

* we call the `await page.goto(‘https://website.com’)` method on the page object to **load that page**

* Once we have a page loaded with a URL, we can **get the page content** calling the `const result = await page.evaluate(() => { //… })` method of page

* `page.$(el) page.$$(el) page.$eval(el, el => ...)` **select the DOM**

* `page.click(el)`  **manipulate the DOM**

* **Get the HTML source of a page** `const source = await page.content()`

* **Emulates a device**  `const device = require(‘puppeteer/DeviceDescriptors’)[‘iPhone X’]; await page.emulate(device)`

* **We can call any DOM API** 
```
const result = await page.evaluate(() => {
    return document.querySelectorAll('.footer-tags a').length
  })
```

[more info](https://flaviocopes.com/puppeteer/)