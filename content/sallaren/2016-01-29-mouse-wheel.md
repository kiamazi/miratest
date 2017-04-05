---
utid: 20160129183530
date: 2016-01-29 18:35:30
_index: mouse-wheel
title: 'Developing for The Real Web - Mouse Wheel Programming'
tags:
  - Javascript
---
<blockquote>
**tl;dr**: Calculating mouse wheel value across browsers is a real pain in the ass. Avoid it as long as you can.
</blockquote>

So nowadays there’s all these talks about React, Redux and so many other cool things to do. But usually at our day jobs,
we can’t just start using all these modern things. We have IE9 customers to think about and we have to make sure our
stuff won’t break for them, otherwise we’d be out of business.

Recently I’ve been given a task at work which involves mouse wheel/trackpad scrolling. I’d have to capture that event,
change it’s speed and velocity, and animate an element using that information.

I started developing this using Chrome on OS X, with Magic Mouse, and everything was smooth and easy. In it’s simplest
form:

	container.addEventListener("mousewheel", (e) => {
		el.style.transform = `translate3d(0, ${e.deltaY / 3}px, 0)`;
	});

You’d think this would work nicely everywhere, but nope. There are several things wrong with this code:

- The event is not called `mousewheel` in every browser.
- `e.deltaY` is not available in all browsers.
- `transform` and `translate3d` are not available in all browsers.

## Fixing it
First, let’s add an event listener for other browsers:

	container.addEventListener("mousewheel", handleScroll);
	container.addEventListener("DOMMouseScroll", handleScroll);

This will cover IE and couple of other browsers. Then now the real trouble begins:

- Some browsers report `e.deltaY` in Wheel Event
- Some browsers report `e.wheelDelta` and they are multiplied by 120
- Some browsers report `e.detail` and sometimes they are multiplied by 3
- Some browsers report both `e.wheelDelta` and `e.detail`
- Number signs in mouse wheel event are different across browsers (+/-)
- Some browsers don’t pass the event to the callback at all! And you’d have to read the event from `window.event`!

My first attempt to solve this was a solution like this:

	// Determine the speed
	if (typeof e.deltaY !== "undefined") {
	    travel = 0 - parseInt(e.deltaY / 3, 10);
	} else if (typeof e.wheelDelta !== "undefined") {
	    travel = parseInt((e.wheelDelta / 120) / 3, 10);
	} else if (typeof e.detail !== "undefined") {
	    travel = 0 - parseInt(e.detail / 3, 10);
	}

Which is quite terrible and doesn’t work as expected. The numbers are not the same so the animation speed would be
really different across browsers. Which is not something that we want.

After some Googling, I came across [this page](http://phrogz.net/js/wheeldelta.html) which was mentioned in a
Stack Overflow issue. I cleaned up the code a bit:

	function wheelDistance(evt) {
	    if (!evt) {
	        evt = window.event;
	    }

	    let w = evt.wheelDelta,
	        d = evt.detail;

	    if (d) {
	        if (w) {
	            return ((w / d / 40 * d) > 0) ? 1 : -1; // Opera
	        }
	        return -d / 3; // Firefox;
	    }

	    return w / 120; // IE, Safari & Chrome
	}

This function tries to solve all issues I mentioned earlier and normalize the output. Then I could just use the output
and adjust the speed as needed. To solve the `transform` and `translate3d` issues I added prefixes:

	container.addEventListener("mousewheel", handleScroll);
	container.addEventListener("DOMMouseScroll", handleScroll);

	function handleScroll(e) {
	    let deltaY = wheelDistance(e);

	    el.style.transform = `translate3d(0, ${deltaY}px, 0)`; // Modern browsers
	    el.style.webkitTransform = `translate3d(0, ${deltaY}px, 0)`; // Webkit
	    el.style.msTransform = `translate(0, ${deltaY}px)`; // Use 2D transform for IE9
	}

## References
In case you have to do the same thing, here are some links:
- [Mouse wheel programming using Javascript](http://www.adomas.org/javascript-mouse-wheel/)
- [Wheel data normalization (Demo)](http://phrogz.net/js/wheeldelta.html)
- [Normalizing mousewheel speed across browsers](http://stackoverflow.com/questions/5527601/normalizing-mousewheel-speed-across-browsers)
