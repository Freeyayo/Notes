# Headless Chrome
- - - -

## What is that?
It’s a way to run the Chrome browser in a headless environment — running Chrome without Chrome. It brings all modern web platform features provided by Chromium and Blink rendering engine to the CLI

## Why is that useful
* Great tool for automated testing and server environments where you don’t need a visible UI shell.
> You may want yo run some tests against a real web page, create PDF of it, or just inspect how a browser renders an URL  

## How to start
```
alias chrome="/Applications/Google\ Chrome.app/Contents/MacOS/Google\ Chrome"

alias chrome-canary="/Applications/Google\ Chrome\ Canary.app/Contents/MacOS/Google\ Chrome\ Canary"

alias chromium="/Applications/Chromium.app/Contents/MacOS/Chromium"
```

* Printing the DOM

`chrome --headless --disable-gpu --dump-dom https://www.chromestatus.com/`

* Create a PDF

`chrome --headless --disable-gpu --print-to-pdf https://www.chromestatus.com/`

*  Taking screenshots

```
chrome --headless --disable-gpu --screenshot https://www.chromestatus.com/

# Size of a standard letterhead.
chrome --headless --disable-gpu --screenshot --window-size=1280,1696 https://www.chromestatus.com/

# Nexus 5x
chrome --headless --disable-gpu --screenshot --window-size=412,732 https://www.chromestatus.com/
```

* REPL mode

```
$ chrome --headless --disable-gpu --repl --crash-dumps-dir=./tmp https://www.chromestatus.com/
[0608/112805.245285:INFO:headless_shell.cc(278)] Type a Javascript expression to evaluate or "quit" to exit.
>>> location.href
{"result":{"type":"string","value":"https://www.chromestatus.com/features"}}
>>> quit
$
```


[more info](https://developers.google.com/web/updates/2017/04/headless-chrome)
 