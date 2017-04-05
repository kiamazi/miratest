---
utid: 20161120184300
date: 2016-11-20 18:43:00
title: Refactoring Buttercup with React/Redux
_index: refactoring-buttercup
tags:
  - Programming
  - Buttercup
  - Javascript
---
I started refactoring [Buttercup](https://github.com/buttercup-pw/buttercup)’s UI about [8 months ago](https://github.com/buttercup-pw/buttercup/pull/92/commits/839345b5c71e8d213535e56ced6775037654543f) I had no idea how difficult it’s going to be.

## Why I Refactored
Back in 2015 when [Perry](http://perrymitchell.net/) and I started making Buttercup, I decided to use [BackboneJS](http://backbonejs.org/) for coding the front-end part of things, and it went well for a while. But I quickly realized BackboneJS is not suitable for what Buttercup wants to achieve. Backbone does not scale well in large applications IMO. To name a few issues we had:

  - Very manual way of rendering and re-rendering. Rendering in BackboneJS is done by replacing the dom contents manually using a jQuery method (`$(...).html(CONTENT)`) and it’s not very efficient. Specially in case of forms and data-heavy interfaces.
  - Backbone is meant to be used with "Ajax" resources (a REST API for example). I wanted to use `Backnone.Model` but I had to create a hacky way to make it work with a local `ipc`-based communication.
  - Templates are totally separated from the view logic, and the only way to show/handle data is a very basic templating language (`_.template`) which is OK most of the times but not for Buttercup. Of course there are other options available but that requires integrating more custom/hacky things.

I was constantly feeling that I’m doing something wrong. So Perry and I decided to look into other options to see if there’s something more suitable to our needs.

## React/Redux Fatigue
I had used React before in some React Native applications, and I was kind of familiar with it’s capabilities. And we realized there are many large-scale projects using React in production and the community around it, is very active. So we said why not go with React.

If you look into ways of implementing React code into large applications, you naturally end up using Redux too: a single-state data store meant to be used with React. I won’t go into details about why it’s good and why you should use it, but instead I’m going to tell you how hard it was to get a hang of it.

The problem with React+Redux is, as a newcomer you have to go through learning curve, and that’s not usually a problem until you start searching online for best practices and suggested ways of organizing the app architecture. Every single developer has their own opinions and they call it "React/Redux Best Practices". So you end up being frustrated and pulling your hair out. I only started making progress when I decided to fuck it; I’m going to go with my guts.

## The Beauty of Redux
After I got comfortable with Redux, I fell in love with it. Not with Redux itself, but with the way of thinking behind it. The fact that it’s only possible to change data by emitting actions and creating a new single state every time is very appealing to me. That state is serializable, so you can easily make your app and all of it’s settings/data portable.

I suggest you look into it for your next project, if you haven’t already.

## Meet the new Buttercup
{{ img /refactoring-buttercup/screen.png [Buttercup] }}
Perry made it possible to use the `buttercup-core` module in browsers and it actually is faster than it’s NodeJS counterpart, thanks to Chrome’s built-in `crypto` implementation. So in the new Buttercup version, we decided to move the actual Buttercup logic from `main` to `renderer` process, which means it sits right beside React and Redux in the UI process.

You can read all about the new release [in Github Releases page](https://github.com/buttercup-pw/buttercup/releases/tag/v0.3.0-alpha). In short: it’s faster, better and a lot of bugs are fixed. The new Buttercup also uses a lot of native OS functions, so you can expect natural behavior in most places (like right clicks, drag and drop, resize, etc).

So go ahead, [download the latest release](https://download.buttercup.pw), give it a spin and [let us what you think](https://github.com/buttercup-pw/buttercup/issues).
