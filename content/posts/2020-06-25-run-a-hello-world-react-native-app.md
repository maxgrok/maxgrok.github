---
template: post
title: Run A 'Hello World' React Native App
slug: eighty-eighth-post
draft: true
date: 2019-03-13T09:00:00.000Z
description: "This is a post about how to build your very first native mobile
  application using React Native: Hello World."
category: React Native
tags:
  - React Native
  - React
  - Mobile
  - Development
---

This post assumes you already have Node.js and npm, as well as the latest version of XCode  installed, a favorite code editor, and are running on a the latest Mac OS X. 

Run the command `npm install -g expo-cli` to install the Expo CLI. 

Initialize your first project by running `expo init AwesomeProjectTitle`. 

Run `cd AwesomeProjectTitle` to go to your AwesomeProjectTitle directory. 

Run `npm start` within this directory. 

This will start the development server and open a browser window with options on the left hand side such as 'run on an ios simulator'. Click on this link. 

It will open XCode after bundling your javascript properly and display a welcome screen and three tabs : "Home", "Settings"...etc. 

Let's open up our project directory in your favorite code editor and go to the `screens/HomeScreen.js` file. 

Delete all the JSX related to the welcome screen text, then type "Hello World" in between <Text> JSX elements. 

It will then display "Hello World" and a rocket icon as the javascript will hot reload in XCode!

Congratulations! You've built your first native iOS and Android application!