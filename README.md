# simple-jquery-implement-for-toturial-point
Simpe jQuery implement for toturial point

For another extension like ajax can use Zeptojs ...


```js
	(function( window ) {

		function jQuery( a ) {

			if( typeof a === "function" ) {
				return this.set_selector( window ).load( a );
			} else {
				return this.set_selector( a );
			}
		}

		jQuery.fn = jQuery.prototype;

		jQuery.fn.set_selector = function( a ) {

			this.selector = a;

			if( typeof this.selector == "string" ) {
				this.selected = document.querySelectorAll( this.selector );
			} else if( typeof this.selector == "object" ) {
				this.selected = [this.selector];
			} else {
				this.selected = this.selector;
			}

			return this;
		}

		jQuery.fn.find = function( selector ) {

			var finded = [];
			var i = 0;

			this.each(function( k ) {

				var pselect = this.querySelectorAll( selector );

				var x;
				for( x in pselect ) {
					if( typeof pselect[x] === 'object' )
						finded[i++] = pselect[x];
				}
			});
		
			this.selected = finded;

			return this;
		}

		jQuery.each = function( object, callback ) {

			var len = object.length;
			for( var i = 0; i < len; i++ ) {
				if( callback.call( object[i], i, object[i] ) === false ) {
					break;
				}
			}

			return this;
		}


		jQuery.fn.each = function( a ) {

			if( typeof a !== 'undefined' )
				jQuery.each( this.selected, a );		

			return this;
		}

		jQuery.fn.eq = function( a ) {
			this.selector = this.selected[a];
			return this;
		}

		jQuery.fn.append = function( a ) {
			this.each(function() {
				this.innerHTML = this.innerHTML + a;
			});
		}
		
		jQuery.fn.prepend = function( a ) {
			this.each(function() {
				this.innerHTML = a + this.innerHTML;
			});
		}
		
		jQuery.fn.html = function( a ) {

			if( typeof a === 'undefined' ) {
				var ret = '';

				this.eq(0).each(function() {
					ret = this.innerHTML;
				});

				return ret;			
			} else {

				return this.each(function() {
					this.innerHTML = a;
				});
			}
		}

		jQuery.fn.attr = function( a, b ) {
			if( typeof b === 'undefined' ) {
				var ret = '';
				this.eq(0).each(function() {
					ret = this.getAttribute( a );
				});

				return ret;			
			} else {

				return this.each(function(  ) {
					this.setAttribute( a, b );
				});

			}
		}

		jQuery.fn.parent = function() {
			var that = this;
			return this.eq(0).each(function() {
				that.set_selector( this.parentElement );
			});
		}

		jQuery.fn.next = function() {
			var that = this;
			return this.eq(0).each(function() {
				that.set_selector( this.nextElementSibling );
			});
		}

		jQuery.fn.prev = function() {
			var that = this;
			return this.eq(0).each(function() {
				that.set_selector( this.previousElementSibling );
			});
		}

		jQuery.fn.css = function( a, b ) {
			if( typeof b === 'undefined' ) {
				var ret = '';
				this.eq(0).each(function() {
					ret = this.style[ a ];
				});

				return ret;			
			} else {

				return this.each(function(  ) {
					this.style[ a ] =  b;
				});

			}
		}

		jQuery.fn.data = function( a, b ) {

			if( typeof a === 'undefined' ) {
				var ret;
				this.each(function() {
					ret = this.data || "";
				});

				return ret;
			} else if( typeof b === 'undefined' ) {

				var ret;
				this.eq(0).each(function() {
					if( typeof this.data === 'undefined'  ) {
						this.data = {};
					}

					ret = this.data[ a ];

				});

				return ret;	
			} else {

				return this.each(function() {
					if( typeof this.data === 'undefined'  ) {
						this.data = {};
					}
					
					this.data[a] = b;
				});				
			}
		}

		jQuery.fn.bind = function( event, callback ) {
			return this.each(function() {
				var that = this;	
				function p_callback( e ) {
					if( ! callback.call( that, e ) ) {
						e.preventDefault();
					}
				}

				var p_event = $(this).data( "event_"+event ) || [];
				p_event[ p_event.length ] = p_callback;

				$(this).data( "event_"+event,  p_event );

				this.addEventListener( event, p_callback);
			});				
		}

		jQuery.fn.unbind = function( event ) {
			return this.each(function() {
				var events = $(this).data( "event_"+event );

				var x;
				for( x in events ) {
					this.removeEventListener( event, events[x] );
				}				
			});
		}

		jQuery.fn.trigger = function( a ) {
			var event = new MouseEvent( a , {
				'view': window,
				'bubbles': true,
				'cancelable': true
			});

			return this.each(function() {
				return this.dispatchEvent( event );
			});
		}

		jQuery.each( [ "focusin", "focusout", "load", "beforeunload", "unload", "change", "click", "dblclick", "focus", "blur", "reset", "submit", "resize", "scroll", "mouseover", "mouseout", "mouseup", "mousedown", "mouseenter", "mousemove", "mouseleave", "contextmenu", "wheel", "keydown", "keypress", "keyup", "select" ], function( k, name ) {
			jQuery.fn[ name ] = function( a ) {
				return this.bind( name, a );
			};
		});

		function $( a ) {
			return new jQuery( a );
		}

		$.fn = jQuery.fn;

		window.$ = window.jQuery = $;

	})( window );

	(function( $ ) {
		$.inArray = function( val, array ) {
			for( x in array ) {
				if( array[x] === val ) {
					return x;
				}
			}

			return -1;
		}

		$.fn.hasClass = function( a ) {
			var ret;
			this.each(function() {
				var classes = $(this).attr("class").split(/\s+/g);

				if( $.inArray( a, classes ) !== -1 ) {
					ret = true;
				} else {
					ret = false;	
				}

			});

			return ret;
		}

		$.fn.serialize = function() {
			var ret = {};
			this.each(function(){
				var data = new FormData( this );

				for( var pair of data.entries() ) {
				   ret[ pair[0] ] = pair[1];
				}				
			});

			return ret;
		}

	})( jQuery );



	//Define plugin outsize closure
	$.fn.test = function() {
		//console.log('in test');
	};

	//Use with jQuery instead $
	jQuery(".test").test();


	//Dom ready
	$(function() {

		$(".test").css( "background", "red" ).click(function() {
			console.log( this );

			//console.log( $(this).attr('class') );

			if( $(this).hasClass("ppp") ) {
				//console.log('Class ppp exist');
			}

			//console.log( $(this).html() );

			$(this).html('changed');

			//console.log( $(this).attr('class') );

		}).eq(1).trigger("click");

		$(".test").each(function() {

			//console.log('in each');
			//console.log( $(this).html() );

			//$(this).unbind('click');
		
		});


		$("form").submit(function() {

			console.log( $(this).serialize() );

			return false;
		});


		$(".test1").find("span").each(function() {
			console.log( $(this).html() );
		})

		console.log( $("#test").next().html() );
		console.log( $("#test").prev().html() );

		$("#test").parent().prev().next().find("span").each(function() {
			console.log( this );
		});


		$("#test").append("<div>aaaa</div>");
		$("#test").prepend("<div>bbbbbb</div>");
	});
```
