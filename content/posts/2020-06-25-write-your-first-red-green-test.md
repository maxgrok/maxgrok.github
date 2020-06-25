---
template: post
title: Write Your First Red-Green Test
slug: eighty-fifth-post
draft: false
date: 2018-03-10T18:12:00.000Z
description: This is a post on writing your first red-green test with Jest and
  Enzyme using a simple create-react-app application.
category: TDD
tags:
  - Jest
  - Enzyme
  - React
  - EnzymeAdapter
---

In this blog post, we're going to create a basic react application that displays "hello world!" and test it. 

Please note: this blog post presumes a basic knowledge of javascript and React. 

First, let's create our app with `create-react-app` by running `npx create-react-app appnamehere`. 

Next, `cd` into that `appnamehere` directory. 

Next, we need to install jest, enzyme, and an enzyme adapter by running `npm install jest jest-enzyme enzyme enzyme-adapter-react-16 --save`

From here, in 7 easy steps, we're going to write our first red-green test using Jest and Enzyme. 

First, open `App.test.js` and import the following from the packages we just installed at the top of the file. 

```javascript
import Enzyme, {shallow} from 'enzyme';
import EnzymeAdapter from 'enzyme-adapter-react-16';
```

Second, we're going to configure Enzyme with a new Adapter after our imports. Please note you should also import React into your test file. 

```javascript
Enzyme.configure({adapter: new EnzymeAdapter()});
```

Third, we will begin writing our test and a helper function.  

```javascript
//helper function 
const findByTestAttr = (wrapper,val) =>{
    return wrapper.find(`[data-test="${val}"`)
}

//test
test('it renders div with id "container" without crashing', ()=>{
    const setup = shallow(<App/>)
    const container = findByTestAttr(setup, 'container')
    return container
});
```

Fourth, watch it fail. Run your tests using `npm test`. 

Fifth, write code to make it pass. We do not currently have a div with an id of container defined in our index.html file. Go to /public, index.html, and add a div with an data-test attribute of container with the following code inside of #root div.  

```javascript
<div data-test="container">
</div>
```

Sixth, save the index.html file and run your tests again. You should be passing your tests. 

Congratulations, you've written your first red-green test!
