---
utid: 20160816214220
date: 2016-08-16 21:42:20
title: Javascript’s Unicode Problem
_index: javascripts-unicode-problem
tags:
  - Javascript
  - Unicode
---
> Disclaimer: A repost of my article for [CafeDev](https://cafedev.org/article/2016/08/javascript-unicode/)

A while ago I needed a sim­ple string pad func­tion, and since it’s a freak­ishly sim­ple task, I just wrote a sim­ple
func­tion:

	function limit(str, limit = 16, padString = "#", padPosition = "right") {
	    const strLength = str.length;

	    if (strLength > limit) {
	        return str.substring(0, limit);
	    } else if (strLength < limit) {
	        const padRepeats = padString.repeat(limit - strLength);
	        return (padPosition === "left") ? padRepeats + str : str + padRepeats;
	    }
	    return str;
	}

Pretty sim­ple, right? And it works too… Until you add in an emoji to your text. Then every­thing falls apart and worlds
col­lide! How? Simple:


	"💩".length // 2!


(If you’re read­ing this on Linux you might ac­tu­ally see 2 bro­ken uni­code char­ac­ters, and the re­sult makes sense un­less
you have in­stalled an emoji pack­age on your browser or OS)...

[Read the full story on CafeDev &rarr;](https://cafedev.org/article/2016/08/javascript-unicode/)
