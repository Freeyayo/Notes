## Showcase
Before:

![](https://wx3.sinaimg.cn/mw690/8272d230gy1gbrcq2yu5kj21400l1wia.jpg)

After:

![](https://wx4.sinaimg.cn/mw690/8272d230gy1gbrcq2z5ikj21400l142u.jpg)

## Description
Default theme of our product just provides us a poor quality fixed navigation top bar, so I need to write a brand-new one to compatible with the current page.

## Idea
The first thing I thought about is to modify the class in different situations according to the current top which changed by scrolling.
Different class names correspond to certain styles.
It works! Thanks to all elements I need to change are contained in their common ancestor node.


## Code
```js
function activeFixTopNavMenu(){
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

  checkDomByClass('.section-items.nav-sections-items', fixTopNavMenu);

  function fixTopNavMenu(){
  		isIPad();
	  	var selector = document.querySelector('body');
	     if(window.innerWidth>768&&selector.className.indexOf('checkout-index-index') !== -1){
	          return;
	     }
		var fixed = false;
		var top_nav = document.querySelector('.section-items.nav-sections-items');
		var items = document.querySelectorAll('.navigation>ul>li');
		var input_container = document.querySelector('.container>.main-header.row>.col-md-8');
		var nav_form = document.querySelector('#search_mini_form');
		var search_box = document.querySelector('#search');
		var status = document.querySelector('.pull-left.af-changeCountry');
		var search_submit = document.querySelector('.atg_store_searchSubmit');
		var search_auto_complete = document.querySelector('#searchautocomplete');
		var search_box_block = document.querySelector('#targeting-data');

		var items_container = document.querySelector('.navigation>ul');
	    var logo = document.createElement('li');
	    logo.id = 'ashfordNavLogo';
	    logo.innerHTML = '<a href="/"><img id="logo" src="https://www.ashford.com/images/elements/logo_sm.png"></a>';
	    appendToFirst(items_container, logo)

		window.onscroll = function () {
		    var sTop = getScroll().top;	//180
		    if(!fixed && sTop>180){
		    	fix();
		    }else if(fixed && sTop<=180){
		    	unfix();
		    	search_box.className = 'input-text text';
		    	search_submit.className = 'atg_store_searchSubmit';
		    }
		}

		function fix(){
			fixed = true;
			search_submit.className += ' fix_nav_search_submit_blur';
			input_container.className += ' fix_nav_position_input_container';
			nav_form.className += ' fix_nav_form_size_blur';
			top_nav.className += ' fix_nav_position_top_nav';
			status.className += ' fix_nav_change_status';
			for(let i=0;i<items.length;i++){
				items[i].className += ' fix_nav_align_items';
			}
			if(innerWidth>999){
				styleNode(search_box, {
					display: "none"
				})
				styleNode(search_box_block,{
					display: "none"
				})
				styleNode(logo, {
		            display: "inline-block",
		            marginTop: "10px"
		        })
			}
		}

		function unfix(){
			fixed = false;
			search_submit.className = search_submit.className.replace(' fix_nav_search_submit_blur','');
			search_box.removeAttribute('style');
			search_box_block.removeAttribute('style');
			input_container.className = input_container.className.replace(' fix_nav_position_input_container','');
			nav_form.className = nav_form.className.replace(' fix_nav_form_size_blur','');
			top_nav.className = top_nav.className.replace(' fix_nav_position_top_nav','');
			status.className = status.className.replace(' fix_nav_change_status','');
			for(let i=0;i<items.length;i++){
				items[i].className = items[i].className.replace(' fix_nav_align_items','');
			}
			styleNode(logo, {
	            display: "none",
	            marginTop: "0"
	        })
		}

		function getScroll(){
	        return {
	            top: window.pageYOffset || document.documentElement.scrollTop || document.body.scrollTop || 0
	        };
	    }

	    function styleNode(node, styles){
	        for(prop in styles){
	            node.style[prop] = styles[prop];
	        }
	    }

	    function appendToFirst(parentNode, targetNode) {
	        if (parentNode.firstElementChild) {
	          parentNode.insertBefore(targetNode, parentNode.firstElementChild);
	        } else {
	          parentNode.appendChild(targetNode);
	        }
	    }

	    function isIPad(){
	        if(navigator.userAgent.indexOf("iPad") != -1){  
	            return;
	        }  
	    }  
	}

}

activeFixTopNavMenu();
```