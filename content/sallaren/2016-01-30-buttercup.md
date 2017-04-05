---
utid: 20160130191012
date: 2016-01-30 19:10:12
_index: buttercup
title: Buttercup 0.1.0-alpha has been released
tags:
  - Buttercup
  - Javascript
---
It’s been couple of months that [Perry](http://perrymitchell.net) and I have been working on our own password manager called [Buttercup](http://buttercup.pw). We released the very first alpha version today, and you can [get it from Github now](https://github.com/buttercup-pw/buttercup/releases/tag/v0.1.0-alpha). Mac and Linux versions are ready at the time of writing this post and a Windows version is underway.
{{ img /buttercup/buttercup-main.png [Buttercup Main] }}

## What’s Buttercup
The existing solutions out there are quite good, but not exactly what we wanted. We needed a free, well-made, open-source and secure password management solution, so we decided to build our own. We put all of our trust in Javascript and started coding it in Javascript using NodeJS and [Electron](http://electron.atom.io/).

### Security
Perry worked on the [buttercup-core](https://github.com/buttercup-pw/buttercup-core) and made a library to be used by the Electron app (or any interested party). The buttercup-core uses **AES 256bit CBC** algorithm to encrypt the data and prepares the password with **PBKDF2** at 1000 iterations. The buttercup-core basically handles all data-related operations like adding/removing groups and entries.

### Cross-Platform
I also worked on the client app using Electron and NodeJS, which was an exciting experience. The client is connected to buttercup-core through Electron’s `ipc` module and all operations are done real-time (the UI is just a graphical representation of what’s inside buttercup-core, it doesn’t have logic by itself).

### Mobile
We’re also planning to add mobile support using [Cordova](https://cordova.apache.org/) or [React Native](https://facebook.github.io/react-native/). Electron has been a good choice for the desktop clients and I’m pretty confident we can make a reliable client for mobile using only Javascript.

### Cloud and Importing
Two of most important features of Buttercup are Cloud-syncing and Importing existing archives from other managers, like Keepass, which are fairly popular. We’re working on these heavily and will release them ASAP.

## Made with Coffee in Helsinki
{{ img /buttercup/helsinki.jpg [Helsinki] }}

Perry and I have spend a lot of weekends coding and designing Buttercup in cafés in Helsinki, and we hope that you will enjoy using it as much as we enjoyed making it.

## Get Buttercup
You can head to our [Releases page on Github](https://github.com/buttercup-pw/buttercup/releases) to see the latest alpha releases and download the binary you need. If you want to contribute, you can do so by forking the project and sending a PR.

Have a safe day!
