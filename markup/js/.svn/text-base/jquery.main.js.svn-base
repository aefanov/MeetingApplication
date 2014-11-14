// page init
jQuery(function(){
	initAccordion();
	initBackgroundResize();
	initSameHeight();
	jQuery('input, textarea').placeholder();
	initStickyHeader();
	initAnchorLinks();
	initLayout();
	initCustomHover();
});

// page layout init
function initLayout() {
	var win = jQuery(window);
	var calcPosition = win.height()/2 + 50;
	jQuery('#block-fixed').fixedSlideBlock({
		slideBlock: '#switcher',
		extraHeight: calcPosition,
	});
}

// smooth anchor links
function initAnchorLinks() {
	var animSpeed = 650;
	var win  = jQuery(window);
	var page = jQuery('html,body');
	var isWP8 = /MSIE 10.*Touch/.test(navigator.userAgent);
	var header = jQuery('#header');
	var links = jQuery('.switcher a');
	var heightHeader = header.outerHeight();
	var body = jQuery('body');
	var fixedClass = 'fixed-block';


	jQuery('a.btn-down, .switcher a, a.go').click(function(e) {
		e.preventDefault();
		var targetId = this.getAttribute('href');
		if(jQuery(this).hasClass('btn-scroll')) body.addClass(fixedClass);
		var calcOffset = targetId.length > 1 ? jQuery(targetId).offset().top - header.outerHeight(true) + 1: 0;

		if (isWP8) {
			win.scrollTop(calcOffset);
		} else {
			page.animate({
				scrollTop: calcOffset
			}, animSpeed);
		}
	});

	links.each(function(i) {
		var self = jQuery(this);
		var holder = jQuery(self.attr('href'));
		var heightHolder = holder.outerHeight();
		win.on('scroll', function(e) {
			var offsetHolder = holder.offset().top;
			e.preventDefault();
			var scroll = jQuery(this).scrollTop();
			var scrollTopCalc = offsetHolder - heightHeader - 10;
			if (i===0) {
				scrollTopCalc = offsetHolder - heightHeader - 10;
			}
			if (scroll > scrollTopCalc && scroll  < offsetHolder - header.outerHeight(true) - 20 + holder.outerHeight()) {
				// links.parent().removeClass('active');
				if (!self.hasClass('active')) {
					self.parent().addClass('active');
				}
			} else {
				self.parent().removeClass('active');
			}
		});
	});
}

// initStickyHeader
function initStickyHeader() {
	var win = jQuery(window);
	var body = jQuery('body');

	function scrollHandler() {
		var scroll = jQuery(this).scrollTop();

		if (scroll > 10) {
			body.addClass('fixed-block');
		} else {
			body.removeClass('fixed-block');
		}
	}

	win.on('scroll', scrollHandler);
}

// accordion menu init
function initAccordion() {
	jQuery('ul.accordion').slideAccordion({
		opener: 'a.opener',
		slider: 'div.slide',
		animSpeed: 300
	});
}

// stretch background to fill blocks
function initBackgroundResize() {
	jQuery('.bg-stretch').each(function() {
		ImageStretcher.add({
			container: this,
			image: 'img'
		});
	});
}

// align blocks height
function initSameHeight() {
	jQuery('.quotes-wrapper').sameHeight({
		elements: '.column',
		flexible: true,
		multiLine: true
	});
}

// add classes on hover/touch
function initCustomHover() {
	jQuery('.categories-area .category').touchHover();
}

/*
 * jQuery Accordion plugin
 */
;(function($){
	$.fn.slideAccordion = function(opt){
		// default options
		var options = $.extend({
			addClassBeforeAnimation: false,
			allowClickWhenExpanded: false,
			activeClass:'active',
			opener:'.opener',
			slider:'.slide',
			animSpeed: 300,
			collapsible:true,
			event:'click'
		},opt);

		return this.each(function(){
			// options
			var accordion = $(this);
			var items = accordion.find(':has('+options.slider+')');

			items.each(function(){
				var item = $(this);
				var opener = item.find(options.opener);
				var slider = item.find(options.slider);
				opener.bind(options.event, function(e){
					if(!slider.is(':animated')) {
						if(item.hasClass(options.activeClass)) {
							if(options.allowClickWhenExpanded) {
								return;
							} else if(options.collapsible) {
								slider.slideUp(options.animSpeed, function(){
									hideSlide(slider);
									item.removeClass(options.activeClass);
								});
							}
						} else {
							// show active
							var levelItems = item.siblings('.'+options.activeClass);
							var sliderElements = levelItems.find(options.slider);
							item.addClass(options.activeClass);
							showSlide(slider).hide().slideDown(options.animSpeed);
						
							// collapse others
							sliderElements.slideUp(options.animSpeed, function(){
								levelItems.removeClass(options.activeClass);
								hideSlide(sliderElements);
							});
						}
					}
					e.preventDefault();
				});
				if(item.hasClass(options.activeClass)) showSlide(slider); else hideSlide(slider);
			});
		});
	};

	// accordion slide visibility
	var showSlide = function(slide) {
		return slide.css({position:'', top: '', left: '', width: '' });
	};
	var hideSlide = function(slide) {
		return slide.show().css({position:'absolute', top: -9999, left: -9999, width: slide.width() });
	};
}(jQuery));

/*
 * jQuery SameHeight plugin
 */
;(function($){
	$.fn.sameHeight = function(opt) {
		var options = $.extend({
			skipClass: 'same-height-ignore',
			leftEdgeClass: 'same-height-left',
			rightEdgeClass: 'same-height-right',
			elements: '>*',
			flexible: false,
			multiLine: false,
			useMinHeight: false,
			biggestHeight: false
		},opt);
		return this.each(function(){
			var holder = $(this), postResizeTimer, ignoreResize;
			var elements = holder.find(options.elements).not('.' + options.skipClass);
			if(!elements.length) return;

			// resize handler
			function doResize() {
				elements.css(options.useMinHeight && supportMinHeight ? 'minHeight' : 'height', '');
				if(options.multiLine) {
					// resize elements row by row
					resizeElementsByRows(elements, options);
				} else {
					// resize elements by holder
					resizeElements(elements, holder, options);
				}
			}
			doResize();

			// handle flexible layout / font resize
			var delayedResizeHandler = function() {
				if(!ignoreResize) {
					ignoreResize = true;
					doResize();
					clearTimeout(postResizeTimer);
					postResizeTimer = setTimeout(function() {
						doResize();
						setTimeout(function(){
							ignoreResize = false;
						}, 10);
					}, 100);
				}
			};

			// handle flexible/responsive layout
			if(options.flexible) {
				$(window).bind('resize orientationchange fontresize', delayedResizeHandler);
			}

			// handle complete page load including images and fonts
			$(window).bind('load', delayedResizeHandler);
		});
	};

	// detect css min-height support
	var supportMinHeight = typeof document.documentElement.style.maxHeight !== 'undefined';

	// get elements by rows
	function resizeElementsByRows(boxes, options) {
		var currentRow = $(), maxHeight, maxCalcHeight = 0, firstOffset = boxes.eq(0).offset().top;
		boxes.each(function(ind){
			var curItem = $(this);
			if(curItem.offset().top === firstOffset) {
				currentRow = currentRow.add(this);
			} else {
				maxHeight = getMaxHeight(currentRow);
				maxCalcHeight = Math.max(maxCalcHeight, resizeElements(currentRow, maxHeight, options));
				currentRow = curItem;
				firstOffset = curItem.offset().top;
			}
		});
		if(currentRow.length) {
			maxHeight = getMaxHeight(currentRow);
			maxCalcHeight = Math.max(maxCalcHeight, resizeElements(currentRow, maxHeight, options));
		}
		if(options.biggestHeight) {
			boxes.css(options.useMinHeight && supportMinHeight ? 'minHeight' : 'height', maxCalcHeight);
		}
	}

	// calculate max element height
	function getMaxHeight(boxes) {
		var maxHeight = 0;
		boxes.each(function(){
			maxHeight = Math.max(maxHeight, $(this).outerHeight());
		});
		return maxHeight;
	}

	// resize helper function
	function resizeElements(boxes, parent, options) {
		var calcHeight;
		var parentHeight = typeof parent === 'number' ? parent : parent.height();
		boxes.removeClass(options.leftEdgeClass).removeClass(options.rightEdgeClass).each(function(i){
			var element = $(this);
			var depthDiffHeight = 0;
			var isBorderBox = element.css('boxSizing') === 'border-box' || element.css('-moz-box-sizing') === 'border-box' || element.css('-webkit-box-sizing') === 'border-box';

			if(typeof parent !== 'number') {
				element.parents().each(function(){
					var tmpParent = $(this);
					if(parent.is(this)) {
						return false;
					} else {
						depthDiffHeight += tmpParent.outerHeight() - tmpParent.height();
					}
				});
			}
			calcHeight = parentHeight - depthDiffHeight;
			calcHeight -= isBorderBox ? 0 : element.outerHeight() - element.height();

			if(calcHeight > 0) {
				element.css(options.useMinHeight && supportMinHeight ? 'minHeight' : 'height', calcHeight);
			}
		});
		boxes.filter(':first').addClass(options.leftEdgeClass);
		boxes.filter(':last').addClass(options.rightEdgeClass);
		return calcHeight;
	}
}(jQuery));

/*
 * jQuery FontResize Event
 */
jQuery.onFontResize = (function($) {
	$(function() {
		var randomID = 'font-resize-frame-' + Math.floor(Math.random() * 1000);
		var resizeFrame = $('<iframe>').attr('id', randomID).addClass('font-resize-helper');

		// required styles
		resizeFrame.css({
			width: '100em',
			height: '10px',
			position: 'absolute',
			borderWidth: 0,
			top: '-9999px',
			left: '-9999px'
		}).appendTo('body');

		// use native IE resize event if possible
		if (window.attachEvent && !window.addEventListener) {
			resizeFrame.bind('resize', function () {
				$.onFontResize.trigger(resizeFrame[0].offsetWidth / 100);
			});
		}
		// use script inside the iframe to detect resize for other browsers
		else {
			var doc = resizeFrame[0].contentWindow.document;
			doc.open();
			doc.write('<scri' + 'pt>window.onload = function(){var em = parent.jQuery("#' + randomID + '")[0];window.onresize = function(){if(parent.jQuery.onFontResize){parent.jQuery.onFontResize.trigger(em.offsetWidth / 100);}}};</scri' + 'pt>');
			doc.close();
		}
		jQuery.onFontResize.initialSize = resizeFrame[0].offsetWidth / 100;
	});
	return {
		// public method, so it can be called from within the iframe
		trigger: function (em) {
			$(window).trigger("fontresize", [em]);
		}
	};
}(jQuery));

/*
 * Mobile hover plugin
 */
;(function($){

	// detect device type
	var isTouchDevice = ('ontouchstart' in window) || window.DocumentTouch && document instanceof DocumentTouch,
		isWinPhoneDevice = /Windows Phone/.test(navigator.userAgent);

	// define events
	var eventOn = (isTouchDevice && 'touchstart') || (isWinPhoneDevice && navigator.pointerEnabled && 'pointerdown') || (isWinPhoneDevice && navigator.msPointerEnabled && 'MSPointerDown') || 'mouseenter',
		eventOff = (isTouchDevice && 'touchend') || (isWinPhoneDevice && navigator.pointerEnabled && 'pointerup') || (isWinPhoneDevice && navigator.msPointerEnabled && 'MSPointerUp') || 'mouseleave';

	// event handlers
	var toggleOn, toggleOff, preventHandler;
	if(isTouchDevice || isWinPhoneDevice) {
		// prevent click handler
		preventHandler = function(e) {
			e.preventDefault();
		};

		// touch device handlers
		toggleOn = function(e) {
			var options = e.data, element = $(this);

			var toggleOff = function(e) {
				var target = $(e.target);
				if (!target.is(element) && !target.closest(element).length) {
					element.removeClass(options.hoverClass);
					element.off('click', preventHandler);
					if(options.onLeave) options.onLeave(element);
					$(document).off(eventOn, toggleOff);
				}
			};

			if(!element.hasClass(options.hoverClass)) {
				element.addClass(options.hoverClass);
				element.one('click', preventHandler);
				$(document).on(eventOn, toggleOff);
				if(options.onHover) options.onHover(element);
			}
		};
	} else {
		// desktop browser handlers
		toggleOn = function(e) {
			var options = e.data, element = $(this);
			element.addClass(options.hoverClass);
			$(options.context).on(eventOff, options.selector, options, toggleOff);
			if(options.onHover) options.onHover(element);
		};
		toggleOff = function(e) {
			var options = e.data, element = $(this);
			element.removeClass(options.hoverClass);
			$(options.context).off(eventOff, options.selector, toggleOff);
			if(options.onLeave) options.onLeave(element);
		};
	}

	// jQuery plugin
	$.fn.touchHover = function(opt) {
		var options = $.extend({
			context: this.context,
			selector: this.selector,
			hoverClass: 'hover'
		}, opt);

		$(this.context).on(eventOn, this.selector, options, toggleOn);
		return this;
	};
}(jQuery));

/*! http://mths.be/placeholder v2.0.7 by @mathias */
;(function(window, document, $) {

	// Opera Mini v7 doesn’t support placeholder although its DOM seems to indicate so
	var isOperaMini = Object.prototype.toString.call(window.operamini) == '[object OperaMini]';
	var isInputSupported = 'placeholder' in document.createElement('input') && !isOperaMini;
	var isTextareaSupported = 'placeholder' in document.createElement('textarea') && !isOperaMini;
	var prototype = $.fn;
	var valHooks = $.valHooks;
	var propHooks = $.propHooks;
	var hooks;
	var placeholder;

	if (isInputSupported && isTextareaSupported) {

		placeholder = prototype.placeholder = function() {
			return this;
		};

		placeholder.input = placeholder.textarea = true;

	} else {

		placeholder = prototype.placeholder = function() {
			var $this = this;
			$this
				.filter((isInputSupported ? 'textarea' : ':input') + '[placeholder]')
				.not('.placeholder')
				.bind({
					'focus.placeholder': clearPlaceholder,
					'blur.placeholder': setPlaceholder
				})
				.data('placeholder-enabled', true)
				.trigger('blur.placeholder');
			return $this;
		};

		placeholder.input = isInputSupported;
		placeholder.textarea = isTextareaSupported;

		hooks = {
			'get': function(element) {
				var $element = $(element);

				var $passwordInput = $element.data('placeholder-password');
				if ($passwordInput) {
					return $passwordInput[0].value;
				}

				return $element.data('placeholder-enabled') && $element.hasClass('placeholder') ? '' : element.value;
			},
			'set': function(element, value) {
				var $element = $(element);

				var $passwordInput = $element.data('placeholder-password');
				if ($passwordInput) {
					return $passwordInput[0].value = value;
				}

				if (!$element.data('placeholder-enabled')) {
					return element.value = value;
				}
				if (value == '') {
					element.value = value;
					// Issue #56: Setting the placeholder causes problems if the element continues to have focus.
					if (element != safeActiveElement()) {
						// We can't use `triggerHandler` here because of dummy text/password inputs :(
						setPlaceholder.call(element);
					}
				} else if ($element.hasClass('placeholder')) {
					clearPlaceholder.call(element, true, value) || (element.value = value);
				} else {
					element.value = value;
				}
				// `set` can not return `undefined`; see http://jsapi.info/jquery/1.7.1/val#L2363
				return $element;
			}
		};

		if (!isInputSupported) {
			valHooks.input = hooks;
			propHooks.value = hooks;
		}
		if (!isTextareaSupported) {
			valHooks.textarea = hooks;
			propHooks.value = hooks;
		}

		$(function() {
			// Look for forms
			$(document).delegate('form', 'submit.placeholder', function() {
				// Clear the placeholder values so they don't get submitted
				var $inputs = $('.placeholder', this).each(clearPlaceholder);
				setTimeout(function() {
					$inputs.each(setPlaceholder);
				}, 10);
			});
		});

		// Clear placeholder values upon page reload
		$(window).bind('beforeunload.placeholder', function() {
			$('.placeholder').each(function() {
				this.value = '';
			});
		});

	}

	function args(elem) {
		// Return an object of element attributes
		var newAttrs = {};
		var rinlinejQuery = /^jQuery\d+$/;
		$.each(elem.attributes, function(i, attr) {
			if (attr.specified && !rinlinejQuery.test(attr.name)) {
				newAttrs[attr.name] = attr.value;
			}
		});
		return newAttrs;
	}

	function clearPlaceholder(event, value) {
		var input = this;
		var $input = $(input);
		if (input.value == $input.attr('placeholder') && $input.hasClass('placeholder')) {
			if ($input.data('placeholder-password')) {
				$input = $input.hide().next().show().attr('id', $input.removeAttr('id').data('placeholder-id'));
				// If `clearPlaceholder` was called from `$.valHooks.input.set`
				if (event === true) {
					return $input[0].value = value;
				}
				$input.focus();
			} else {
				input.value = '';
				$input.removeClass('placeholder');
				input == safeActiveElement() && input.select();
			}
		}
	}

	function setPlaceholder() {
		var $replacement;
		var input = this;
		var $input = $(input);
		var id = this.id;
		if (input.value == '') {
			if (input.type == 'password') {
				if (!$input.data('placeholder-textinput')) {
					try {
						$replacement = $input.clone().attr({ 'type': 'text' });
					} catch(e) {
						$replacement = $('<input>').attr($.extend(args(this), { 'type': 'text' }));
					}
					$replacement
						.removeAttr('name')
						.data({
							'placeholder-password': $input,
							'placeholder-id': id
						})
						.bind('focus.placeholder', clearPlaceholder);
					$input
						.data({
							'placeholder-textinput': $replacement,
							'placeholder-id': id
						})
						.before($replacement);
				}
				$input = $input.removeAttr('id').hide().prev().attr('id', id).show();
				// Note: `$input[0] != input` now!
			}
			$input.addClass('placeholder');
			$input[0].value = $input.attr('placeholder');
		} else {
			$input.removeClass('placeholder');
		}
	}

	function safeActiveElement() {
		// Avoid IE9 `document.activeElement` of death
		// https://github.com/mathiasbynens/jquery-placeholder/pull/99
		try {
			return document.activeElement;
		} catch (err) {}
	}

}(this, document, jQuery));

/*
 * Image Stretch module
 */
var ImageStretcher = {
	getDimensions: function(data) {
		// calculate element coords to fit in mask
		var ratio = data.imageRatio || (data.imageWidth / data.imageHeight),
			slideWidth = data.maskWidth,
			slideHeight = slideWidth / ratio;

		if(slideHeight < data.maskHeight) {
			slideHeight = data.maskHeight;
			slideWidth = slideHeight * ratio;
		}
		return {
			width: slideWidth,
			height: slideHeight,
			top: (data.maskHeight - slideHeight) / 2,
			left: (data.maskWidth - slideWidth) / 2
		};
	},
	getRatio: function(image) {
		if(image.prop('naturalWidth')) {
			return image.prop('naturalWidth') / image.prop('naturalHeight');
		} else {
			var img = new Image();
			img.src = image.prop('src');
			return img.width / img.height;
		}
	},
	imageLoaded: function(image, callback) {
		var self = this;
		var loadHandler = function() {
			callback.call(self);
		};
		if(image.prop('complete')) {
			loadHandler();
		} else {
			image.one('load', loadHandler);
		}
	},
	resizeHandler: function() {
		var self = this;
		jQuery.each(this.imgList, function(index, item) {
			if(item.image.prop('complete')) {
				self.resizeImage(item.image, item.container);
			}
		});
	},
	resizeImage: function(image, container) {
		this.imageLoaded(image, function() {
			var styles = this.getDimensions({
				imageRatio: this.getRatio(image),
				maskWidth: container.width(),
				maskHeight: container.height()
			});
			image.css({
				width: styles.width,
				height: styles.height,
				marginTop: styles.top,
				marginLeft: styles.left
			});
		});
	},
	add: function(options) {
		var container = jQuery(options.container ? options.container : window),
			image = typeof options.image === 'string' ? container.find(options.image) : jQuery(options.image);

		// resize image
		this.resizeImage(image, container);

		// add resize handler once if needed
		if(!this.win) {
			this.resizeHandler = jQuery.proxy(this.resizeHandler, this);
			this.imgList = [];
			this.win = jQuery(window);
			this.win.on('resize orientationchange', this.resizeHandler);
		}

		// store item in collection
		this.imgList.push({
			container: container,
			image: image
		});
	}
};

// Fixed Slide Block plugin
;(function(){
	jQuery.fn.fixedSlideBlock = function(options) {
		var options = jQuery.extend({
			slideBlock: '#sidebar',
			alwaysStick: true,
			animDelay: 100,
			animSpeed: 250,
			extraHeight: 0,
			positionType: 'auto' // 'fixed', 'absolute'
		}, options);
		
		return this.each(function(){
			var win = jQuery(window);
			var holder = jQuery(this);
			var fixedBlock = holder.find(options.slideBlock);
			var positionType = options.positionType, timer, d, stayStatic, prevHeight;
			
			// skip if block does not exists
			if(!fixedBlock.length) {
				return;
			}
			
			// detect positioning type
			if(positionType === 'auto') {
				positionType = !isFixedPositionSupported || isTouchDevice ? 'absolute' : 'fixed';
			}
			
			// recalculate all values
			function recalculateDimensions() {
				var origStyle = fixedBlock.attr('style');
				fixedBlock.css({position:'',left:'',top:''});
				d = {
					winHeight: win.height(),
					scrollTop: win.scrollTop(),
					scrollLeft: win.scrollLeft(),
					holderOffset: holder.offset(),
					holderHeight: holder.height(),
					blockPosition: fixedBlock.position(),
					blockOffset: fixedBlock.offset(),
					blockHeight: fixedBlock.outerHeight(true)
				};
				fixedBlock.attr('style',origStyle);
				
				// check for static position
				if(prevHeight !== d.holderHeight) {
					prevHeight = d.holderHeight;
					stayStatic = checkStaticPosition();
				}
			}
			
			// dont fix block if content too small
			function checkStaticPosition() {
				var origStyle = fixedBlock.attr('style');
				var origHeight = d.holderHeight;
				fixedBlock.css({position:'absolute'});
				var newHeight = holder.height();
				if(newHeight < origHeight) {
					fixedBlock.css({position:'', top:'', left:''});
					return true;
				} else {
					fixedBlock.attr('style',origStyle);
				}
			}
			
			function positionFixed() {
				if(d.scrollTop > d.blockOffset.top - options.extraHeight) {
					// check that block fits in holder
					if(d.scrollTop + options.extraHeight - d.holderOffset.top + d.blockHeight > d.holderHeight) {
						fixedBlock.css({position:'absolute', left: d.blockPosition.left, top: d.blockPosition.top + d.holderHeight - d.blockHeight - (d.blockOffset.top - d.holderOffset.top)});
					}
					// set default fixed position
					else {
						fixedBlock.css({position:'fixed', left: d.blockOffset.left - d.scrollLeft, top: options.extraHeight});
					}
				} else {
					// disable sticking
					fixedBlock.css({position:'', top:'', left:''});
				}
			}
			
			function positionAbsolute(noAnimation) {
				// default top position
				var top = d.blockPosition.top;
				if(d.scrollTop > d.blockOffset.top - options.extraHeight) {
					// check that block fits in holder
					if(d.scrollTop + options.extraHeight - d.holderOffset.top + d.blockHeight > d.holderHeight) {
						top = d.blockPosition.top + d.holderHeight - d.blockHeight - (d.blockOffset.top - d.holderOffset.top);
					}
					// set fixed position
					else {
						top = d.blockPosition.top + d.scrollTop - d.blockOffset.top + options.extraHeight;
					}
				}
				fixedBlock.stop().css({position:'absolute', left: d.blockPosition.left});
				
				// change block position animation
				if(noAnimation === true) {
					fixedBlock.css({top: top});
				} else {
					fixedBlock.animate({top: top},{duration: options.animSpeed});
				}
			}
			
			// reposition function
			function reposition(e) {
				// detect behavior
				var noAnimation = (e === true);
				
				// recalculate size and offsets
				recalculateDimensions();
				if(stayStatic) {
					return;
				}
				
				// disable when window is smaller then fixed block
				if(!options.alwaysStick && d.winHeight < d.blockHeight) {
					fixedBlock.css({position:'', top:'', left:''});
					return;
				}
				
				// call position handler
				if(positionType === 'fixed') {
					positionFixed();
				} else {
					if(noAnimation) {
						positionAbsolute(noAnimation);
					}
					clearTimeout(timer);
					timer = setTimeout(positionAbsolute, options.animDelay);
				}
			}
			
			// add event handlers
			setTimeout(function() {
				reposition(true);
				win.bind('scroll', reposition);
				win.bind('resize orientationchange', function(){
					reposition(true);
				});
			},10);
		});
	};
	
	// detect device type
	var isTouchDevice = (function() {
		try {
			return ('ontouchstart' in window) || window.DocumentTouch && document instanceof DocumentTouch;
		} catch (e) {
			return false;
		}
	}());
	
	// detect position fixed support
	var isFixedPositionSupported = (function(){
		var supported = false, container = document.createElement('div'), fixedBlock = document.createElement('div');
		container.style.cssText = 'position:absolute;top:9px;left:9px;width:100px;height:100px;background:red;';
		fixedBlock.style.cssText = 'position:fixed;top:7px;width:1px;height:1px;';
		container.appendChild(fixedBlock);
		document.documentElement.insertBefore(container, document.documentElement.childNodes[0]);
		supported = (fixedBlock.offsetTop == 7 || fixedBlock.offsetTop == -2 || (!fixedBlock.offsetWidth && typeof document.documentElement.style.maxHeight !== 'undefined'));
		document.documentElement.removeChild(container);
		fixedBlock = container = null;
		return supported;
	}());
}());

/*
 * Browser Detect script
 */
BrowserDetect = (function() {
	// script settings
	var options = {
		osVersion: false,
		minorBrowserVersion: false
	};

	// browser data
	var browserData = {
		browsers: {
			chrome: uaMatch(/Chrome\/([0-9\.]*)/),
			firefox: uaMatch(/Firefox\/([0-9\.]*)/),
			safari: uaMatch(/Version\/([0-9\.]*).*Safari/),
			opera: uaMatch(/Opera\/.*Version\/([0-9\.]*)/, /Opera\/([0-9\.]*)/),
			msie: uaMatch(/MSIE ([0-9\.]*)/, /Trident.*rv:([0-9\.]*)/)
		},
		engines: {
			webkit: uaContains('AppleWebKit'),
			trident: uaMatch(/(MSIE|Trident)/),
			gecko: uaContains('Gecko'),
			presto: uaContains('Presto')
		},
		platforms: {
			win: uaMatch(/Windows NT ([0-9\.]*)/),
			mac: uaMatch(/Mac OS X ([0-9_\.]*)/),
			linux: uaContains('X11', 'Linux')
		}
	};

	// perform detection
	var ua = navigator.userAgent;
	var detectData = {
		platform: detectItem(browserData.platforms),
		browser: detectItem(browserData.browsers),
		engine: detectItem(browserData.engines)
	};

	// private functions
	function uaMatch(regExp, altReg) {
		return function() {
			var result = regExp.exec(ua) || altReg && altReg.exec(ua);
			return result && result[1];
		};
	}
	function uaContains(word) {
		var args = Array.prototype.slice.apply(arguments);
		return function() {
			for(var i = 0; i < args.length; i++) {
				if(ua.indexOf(args[i]) < 0) {
					return;
				}
			}
			return true;
		};
	}
	function detectItem(items) {
		var detectedItem = null, itemName, detectValue;
		for(itemName in items) {
			if(items.hasOwnProperty(itemName)) {
				detectValue = items[itemName]();
				if(detectValue) {
					return {
						name: itemName,
						value: detectValue
					};
				}
			}
		}
	}

	// add classes to root element
	(function() {
		// helper functions
		var addClass = function(cls) {
			var html = document.documentElement;
			html.className += (html.className ? ' ' : '') + cls;
		};
		var getVersion = function(ver) {
			return typeof ver === 'string' ? ver.replace(/\./g, '_') : 'unknown';
		};

		// add classes
		if(detectData.platform) {
			addClass(detectData.platform.name);
			if(options.osVersion) {
				addClass(detectData.platform.name + '-' + getVersion(detectData.platform.value));
			}
		}
		if(detectData.engine) {
			addClass(detectData.engine.name);
		}
		if(detectData.browser) {
			addClass(detectData.browser.name);
			addClass(detectData.browser.name + '-' + parseInt(detectData.browser.value, 10));
			if(options.minorBrowserVersion) {
				addClass(detectData.browser.name + '-' + getVersion(detectData.browser.value));
			}
		}
	}());

	// export detection information
	return detectData;
}());

/*! matchMedia() polyfill - Test a CSS media type/query in JS. Authors & copyright (c) 2012: Scott Jehl, Paul Irish, Nicholas Zakas. Dual MIT/BSD license */
;window.matchMedia=window.matchMedia||(function(e,f){var c,a=e.documentElement,b=a.firstElementChild||a.firstChild,d=e.createElement("body"),g=e.createElement("div");g.id="mq-test-1";g.style.cssText="position:absolute;top:-100em";d.appendChild(g);return function(h){g.innerHTML='&shy;<style media="'+h+'"> #mq-test-1 { width: 42px; }</style>';a.insertBefore(d,b);c=g.offsetWidth==42;a.removeChild(d);return{matches:c,media:h}}})(document);

/*! Picturefill - Responsive Images that work today. (and mimic the proposed Picture element with span elements). Author: Scott Jehl, Filament Group, 2012 | License: MIT/GPLv2 */
;(function(a){a.picturefill=function(){var b=a.document.getElementsByTagName("span");for(var f=0,l=b.length;f<l;f++){if(b[f].getAttribute("data-picture")!==null){var c=b[f].getElementsByTagName("span"),h=[];for(var e=0,g=c.length;e<g;e++){var d=c[e].getAttribute("data-media");if(!d||(a.matchMedia&&a.matchMedia(d).matches)){h.push(c[e])}}var m=b[f].getElementsByTagName("img")[0];if(h.length){var k=h.pop();if(!m||m.parentNode.nodeName==="NOSCRIPT"){m=a.document.createElement("img");m.alt=b[f].getAttribute("data-alt")}if(k.getAttribute("data-width")){m.setAttribute("width",k.getAttribute("data-width"))}else{m.removeAttribute("width")}if(k.getAttribute("data-height")){m.setAttribute("height",k.getAttribute("data-height"))}else{m.removeAttribute("height")}m.src=k.getAttribute("data-src");k.appendChild(m)}else{if(m){m.parentNode.removeChild(m)}}}}};if(a.addEventListener){a.addEventListener("resize",a.picturefill,false);a.addEventListener("DOMContentLoaded",function(){a.picturefill();a.removeEventListener("load",a.picturefill,false)},false);a.addEventListener("load",a.picturefill,false)}else{if(a.attachEvent){a.attachEvent("onload",a.picturefill)}}}(this));