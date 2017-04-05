---
utid: 20151223213500
date: 2015-12-23 21:35:00
title: Making an iOS app using Javascript vs. Swift
_index: app-journey
tags:
  - Javascript
  - Swift
---
We decided to do a small hackaton at [Kiosked](http://www.kiosked.com/) during summer which was all kinds of fun. Couple of colleagues and me decided to make an app for finding the best restaurant to go to for lunch, which as you know isn’t an easy thing to do ;-) Our lunch app was called Lunchify and it would do these things:

- Show the list of venues based on location proximity
- Show lunch menu for each restaurant
- Show directions to the venue (navigation)

## Cordova/AngularJS
As a strong believer in Javascript, I suggested we build the app using Javascript so it would be easier to implement and also be cross-platform. The guys worked out the API behind the app and scraped some data from the web to show in the app, and I coded the app using JS, [Cordova](https://cordova.apache.org/) and [Ionic](http://ionicframework.com/) Framework. The whole thing went pretty well and we had a working app by the end of second day.
The amount of effort I put into making the app was really minimal and many things were already being handled by Ionic and [AngularJS](https://angularjs.org/), so what I was actually doing was just connecting things together. This is the result:

{{ img /app-journey/cordova.jpg [Cordova AngularJS] }}

The result was more than OK for what it was (a hackaton project) and everybody was happy with it. But as a perfectionist that I am, I wanted it to be more than that. So I registered a new domain and decided to do a real app to be released in the App Store. I could just continue working on the existing project, but it had a few issues:

- Cordova apps are wrapped in a UIWebView, so basically they run a bit slower than normal browsers
- UI elements are not native. They are MADE to look native using CSS, but it’s not quite the same
- Animation performance (specially during page transitions) is not great and far from native-looking
- AngularJS. Nothing personal though.

## React Native
At the time, [React Native](https://facebook.github.io/react-native/) was under heavy development, but it looked really promising. And again as a strong believer in Javascript, I decided to git it a shot. React Native has major benefits:

- Coded in ES6 (JSX actually, but whatever), so yay Classes, Destructuring and every awesome feature of ES6. Since it’s transpiled using [Babel](https://babeljs.io/)
- Mostly native, so no UIWebView anymore. React Native is just a bridge between Javascript and Objective-C
- Cross-Platform, recent versions of React Native work for Android too
- Polyfills awesome APIs in browsers, like `fetch` API

I rebuilt most of what I had done in Cordova using React Native, and while the process wasn’t all sunshine and candies (since React Native was and is in Beta and changes a lot), the resulting app was *native* and smooth. Here it is:

{{ img /app-journey/react-native.jpg [React Native] }}

Although I put nearly a month of my free time into this, but I wasn’t quite happy with the result. Mainly because:

- JSX! Tried to be open-minded but HTML in JS? No way.
- Performance issues. Maps and animations don’t play well with each other.
- Takes time to startup since it needs to read a huge Javascript file and do them all in Objective-C (or Java)
- The API is not complete and for many things I had to submit issues, play with their code or write plugins.
- Errors are not sometimes clear and takes hours to debug a simple issue. Node dependency issues are terrible here.

## Swift
OK, at this point I had two options. Choose between React Native and AngularJS/Cordova, or just give up and forget about the project. It was a nice experiment after all. Then I thought of a third option: Learn [Swift](https://swift.org/) and build the app with that. How hard could it possibly be? Turns out: Not hard at all.
I never liked Objective-C’s syntax. It’s too verbose in my opinion. But Swift on the other hand looked really clean and had a modern syntax, much like the languages I’m already familiar with. So I followed some online courses, read the Swift docs and a book, and then started coding the same thing in XCode. Pros:

- Clean and familiar syntax
- All Objective-C libraries, APIs and tools are available
- Great debugging with XCode
- Genunie native without any bridges
- Great performance

As I’m writing this post, the app looks like this:

{{ img /app-journey/swift.jpg [Swift] }}

And the source code is [on Github](https://github.com/sallar/lunchify-swift) at your disposal. I’m a rookie Swift developer so excuse any bad practices or repetative code, I will make it better as I go further.
I also put the [Cordova](https://github.com/sallar/lunchify-cordova) and [React Native](https://github.com/sallar/lunchify-react-native) source codes on Github if you want to check them out too for any reason. I haven’t cleaned the code up though.

## Now What?
I will continue working on the Swift version of the app and release it on App Store as soon as I have enough data to make it useful. Play with the code and send a pull request if you will :-)
