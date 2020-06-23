---
template: post
title: "Gatsby: Sharp Package Install Issue"
slug: one-hundred-and-fifth-post
draft: false
date: 2019-10-27T13:37:00.000Z
description: This is a post about how to fix issue with package 'Sharp' in 'npm
  install' in Gatsby.
category: Gatsby
tags:
  - Gatsby
  - Debugging
---


In case anyone else has this issue, I decided to write this post about the `Sharp` package error install using npm with Gatsby. 

I was updating my blog and realized that I'd set NVM to a new Node version (v.12.12.0) and hadn't installed Gatsby command line tools when trying to run 'gatsby build' to test out a new post before deploying.  

Upon installing Gatsby CLI using `npm install -g gatbsy-cli` for the latest version, I discovered an error with installing `Sharp` package when running `npm install` within my blog directory. 

Here is the error I got: 
```js
gyp ERR! build error
gyp ERR! stack Error: `make` failed with exit code: 2
gyp ERR! stack     at ChildProcess.onExit (/Users/maxgrok/.nvm/versions/node/v12.12.0/lib/node_modules/npm/node_modules/node-gyp/lib/build.js:194:23)
gyp ERR! stack     at ChildProcess.emit (events.js:210:5)
gyp ERR! stack     at Process.ChildProcess._handle.onexit (internal/child_process.js:272:12)
gyp ERR! System Darwin 18.7.0
gyp ERR! command "/Users/maxgrok/.nvm/versions/node/v12.12.0/bin/node" "/Users/maxgrok/.nvm/versions/node/v12.12.0/lib/node_modules/npm/node_modules/node-gyp/bin/node-gyp.js" "rebuild"
gyp ERR! cwd /Users/maxgrok/.nvm/versions/node/v12.12.0/lib/node_modules/sharp
gyp ERR! node -v v12.12.0
gyp ERR! node-gyp -v v5.0.5
gyp ERR! not ok
npm ERR! code ELIFECYCLE
npm ERR! errno 1
npm ERR! sharp@0.21.0 install: `(node install/libvips && node install/dll-copy && prebuild-install) || (node-gyp rebuild && node install/dll-copy)`
npm ERR! Exit status 1
npm ERR!
npm ERR! Failed at the sharp@0.21.0 install script.
npm ERR! This is probably not a problem with npm. There is likely additional logging output above.

npm ERR! A complete log of this run can be found in:
npm ERR!     /Users/maxgrok/.npm/_logs/2019-10-27T13_19_57_984Z-debug.log
```

Here is the link to the issue on <a href="https://github.com/gatsbyjs/gatsby/issues/11026"> Github</a>.

I read one of the solutions was to run `npm upgrade` and it worked. It upgraded the version of `Sharp` to `0.23.1` to work well with the newest version of Gatsby. It also updated a bunch of other packages as well. 

Hopefully, this solves your issue!
