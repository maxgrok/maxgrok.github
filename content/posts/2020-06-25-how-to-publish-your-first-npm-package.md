---
template: post
title: How to Publish Your First NPM Package
slug: ninety-second-post
draft: true
date: 2019-04-11T17:44:00.000Z
description: This is a post is about how to make a publish a package to NPM registry.
category: NPM
tags:
  - NPM
  - Node.js
  - NPM package
---

Today, we'll be going through how to publish an NPM package. It will be quick and easy! 
We will be presuming you know basic npm and have node and npm and git installed. 

## To Build Your First NPM Package, Follow These Steps:

(1) Create a git repository in a new folder by running `git init`. <br/>
(2) Create a git repository on Github using the name of the package you want to create. (Go to Github.com and click 'Create a New Repository' in your user account. Google around for Github help if you have trouble.)<br/>
(3) Run `git remote add origin` and then add the https address or ssh address of your github repo. This syncs the git repo with your local repo for all changes. <br/>
(4) Run `npm init` inside the same directory. This creates the `package.json` file for your npm package.<br/>
(5) Answer all the publishing questions about the description of the package, name of the package, version of the package (use semantic versioning), license, and author.<br/>
(6) Run `git add .` to stage the changes in the directory for commiting. <br/>
(7) Run `git commit -m "adding package.json"` in your command line, still in the same root directory. <br/>
(8) Run `git push origin master` to push the changes already made. <br/>
(9) Write your node module in your own index.js file. <br/>
(10) `git add .` , `git commit -m "changes"`, `git push origin master` to update with your code. <br/>
(10) When you are done with your code, run `npm publish` to publish your package. <br/>
(11) Lastly, add the tag with the same version that you started the repo with in your npm init. i.e. 1.0.0 is the default. Run `git tag 1.0.0` to tag that version. <br/><br/>

To verify that your package was published:<br/>
(1) Run `npm info packagenamehere` which will bring you to the package location in a browser. <br/>
(2) Run `npm repo packagenamehere` to go to the repository of the package. <br/>
<br/>
## Congratulations! You have published your first NPM package! 

Source: <br/>

<a href="https://app.pluralsight.com/library/courses/npm-playbook">"NPM Playbook"</a>by PluralSight.
