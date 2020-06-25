---
template: post
title: Train Your First NLP Node.JS app
slug: eighty-nineth-post
draft: true
date: 2019-06-25T13:30:00.000Z
description: This is a post about how to build your very first natural language
  processing application using Wink-ner and Node.js.
category: NLP
tags:
  - Wink-ner
  - NLP
---

Hello!

Today, we're going to build our first Natural Language Processing application using Wink-ner, an npm package. This tutorial assumes a basic knowledge of Node.js, npm, and javascript. 

Let's get started! 
First, build your node app by running `npm init` in an empty directory. 

Then, let's install the wink-ner node module by running `npm install wink-ner` in our root directory. 

Now, let's create an `index.js` file in our root directory with the following code. I will explain what the code is doing further down in comments in the code below. 
 
```javascript 
var ner = require( 'wink-ner' );
// Create your instance of wink ner & use defualt config.
var myNER = ner();
// Define training data, which categorizes different words according to what they are. Here we are training the recognition of names.
var trainingData = [
  { text: 'Mike Smith', entityType: 'name', uid: 'name' },
  { text: 'Lisa Haung', entityType: 'name', uid: 'name' },
  { text: 'Arthur Wilson', entityType: 'name', uid: 'name' }
];
// Learn from the training data.
myNER.learn( trainingData );
// Since recognize() requires tokens, use wink-tokenizer.
var winkTokenizer = require( 'wink-tokenizer' );
// Instantiate it and extract tokenize() api.
var tokenize = winkTokenizer().tokenize;
// Tokenize the sentence.
var oneToken = tokenize(`Arthur Wilson
Software Engineer
Decision & Security Technologies
ABC Technologies
123 North 11th Street
Suite 229
Arlington, VA 22209
Tel: +1 (703) 555-1259
Fax: +1 (703) 555-1200
awilson@abctech.com`);

 var twoToken = tokenize(`Foobar Technologies
Analytic Developer
Lisa Haung
1234 Sentry Road
Columbia, MD 12345
Phone: 410-555-1234
Fax: 410-555-4321
lisa.haung@foobartech.com`)

 var thirdToken = tokenize(`ASYMMETRIK LTD
Mike Smith
Senior Software Engineer
(410)555-1234
msmith@asymmetrik.com`)
// Simply Detect entities!
tokens = myNER.recognize( oneToken );
token = myNER.recognize( twoToken );
tok = myNER.recognize( thirdToken );

console.log( oneToken, twoToken, thirdToken );
```
 
Now, we're going to run our program by running `node index.js`. 

You should see a console.log output of the entire string of all three of the tokens, along with what they are recognized to be! 

Congratulations! You just built your first natural language processing program in Node.js with npm and wink-ner!