## Showcase
[See Final Result on Twitter](https://twitter.com/ineedahash/status/1210251012964962304?s=12)
The production version is more optimal than it.

## Description
When mouse hovers on the products' image, a magnifier will show on the right to observe details.
To compatible with variable browsers, I write code in ES5.

- The significant API in code below is [drawImg()](https://developer.mozilla.org/en-US/docs/Web/API/CanvasRenderingContext2D/drawImage)

	> void _ctx_.drawImage(_image_, _dx_, _dy_);
void _ctx_.drawImage(_image_, _dx_, _dy_, _dWidth_, _dHeight_);
void _ctx_.drawImage(_image_, _sx_, _sy_, _sWidth_, _sHeight_, _dx_, _dy_, _dWidth_, _dHeight_);

- And UIEvent.layerX(UIEvent.layerY)
	> The `UIEvent.layerX(UIEvent.layerY)` read-only property returns the horizontal coordinate of the event relative to the current layer.

## Idea
When mouse hovers on the image, the magnifier is drawn by canvas will display on the right side of image. In order to achieve intended effect, we need coordinates in real time by using layerX(layerY). 

We can use `_ctx_.drawImage(_image_, _sx_, _sy_, _sWidth_, _sHeight_, _dx_, _dy_, _dWidth_, _dHeight_)` to draw new image as a magnifier. 

How to draw?
Choosing a smaller scaled range on source image display as bigger one in destination area

[Reference from MDN](https://developer.mozilla.org/en-US/docs/Web/API/Canvas_API/Tutorial/Pixel_manipulation_with_canvas)

## Code
```js
function zoomer(root) {
        //will be disabled with ipad vertical or mobile
        if (window.innerWidth <= 999) return; 
        
        //check if pictures are loaded
        var checkDomByClass = function checkDomByClass(dom, callback) {
            var node = null;
            var checking = setInterval(function () {
                node = document.querySelector(dom);
                if (node) {
                    clearInterval(checking);
                    callback();
                }
            }, 500);
        };
		//Once img node loaded which indicating image has loaded, then excuting the activateZoomer function   
        checkDomByClass(root, activateZoomer);

        function activateZoomer() {
            var picsContainer = document.querySelector('.fotorama__stage');
            var canvasContainer = document.querySelector('.page-title-wrapper.product');
            var canvas = document.createElement('canvas');
            styleNode(canvas, {
                border: "1px solid",
                position: "absolute",
                background: "white",
                width: "388px",
                height: "350px",
                zIndex: "999",
                display: "none"
            });
            canvas.id = 'zoom';
            //This is important to set size to keep canvas right scaled
            canvas.width = 388;
            canvas.height = 350;
            appendToFirst(canvasContainer, canvas);
            //Adding events on parent node of img node accordingly
            picsContainer.addEventListener('mouseover', bindZoomer, false);
            picsContainer.addEventListener('mouseleave', unBindZoomer, false);

            function bindZoomer(e) {
                //every page will bind this event only once
                //activate the zoomer by detecting if is hovering on the specific pictures
                if (e.target.id === "magnifier-item-0" || e.target.className === "fotorama__img"){
                    canvas.style.display = 'block';
                    drawZoomer(e.target);
                }
            }

            function unBindZoomer(e) {
                canvas.style.display = 'none';
            }

            function drawZoomer(img) {
                var zoomctx = document.getElementById('zoom').getContext('2d'); 

				//anti-aliasing for variable browsers
                zoomctx.imageSmoothingEnabled = true;
                zoomctx.mozImageSmoothingEnabled = true;
                zoomctx.webkitImageSmoothingEnabled = true;
                zoomctx.msImageSmoothingEnabled = true;

                function zoom(event) {
                    var e = event || window.event;
                    var offset = detectIE() && detectIE() >= 11 ? 240 : 40; 
                    offset = navigator.userAgent.indexOf("Firefox") > 0 ? 220 : offset;//If using FF, offset should change dynamically

					//layerX/layerY is better than clientX/clientY in estimating location in this case
                    var x = e.layerX;
                    var y = e.layerY; 
                    //limit range of mouse in proper if not using IE or FF

                    if (!detectIE() && navigator.userAgent.indexOf("Firefox")===-1) {
                        if (y < -155) y = -155;
                        if (y > 160) y = 160;
                    } 
                    //ensure zoomer displaying zoomed images from the center of mouse


                    zoomctx.clearRect(0, 0, canvas.width, canvas.height);
                    if((detectIE()&&detectIE() === 11)){
                         zoomctx.drawImage(img,  1.1*x + 100, 1.1*y + 100, 320, 320, 0, 0, 400, 400);
                    }else{
                         zoomctx.drawImage(img, (img.width / 2 + x - offset) * 2, (img.height / 2 + y - offset) * 2, 300, 300, 0, 0, 440, 440);
                    }
                }

                ;
                img.addEventListener('mousemove', zoom);
            } 
            
            //**utils**//
            function appendToFirst(parentNode, targetNode) {
                if (parentNode.firstElementChild) {
                    parentNode.insertBefore(targetNode, parentNode.firstElementChild);
                } else {
                    parentNode.appendChild(targetNode);
                }
            }

            function styleNode(node, styles) {
                for (prop in styles) {
                    node.style[prop] = styles[prop];
                }
            } 
            //****//

        }
    }

    function detectIE() {
        var ua = window.navigator.userAgent; // Test values; Uncomment to check result …
        var msie = ua.indexOf('MSIE ');

        if (msie > 0) {
            // IE 10 or older => return version number
            return parseInt(ua.substring(msie + 5, ua.indexOf('.', msie)), 10);
        }

        var trident = ua.indexOf('Trident/');

        if (trident > 0) {
            // IE 11 => return version number
            var rv = ua.indexOf('rv:');
            return parseInt(ua.substring(rv + 3, ua.indexOf('.', rv)), 10);
        }

        var edge = ua.indexOf('Edge/');

        if (edge > 0) {
            // Edge (IE 12+) => return version number
            return parseInt(ua.substring(edge + 5, ua.indexOf('.', edge)), 10);
        } // other browser


        return false;
    }

    zoomer('.fotorama__img');
```