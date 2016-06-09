#Functional programming? In my React Component?

We've seen __React__ components written using React.createClass and more recently as ES2015 classes (preferable in the eyes of many devs). Now we're going to see a simplified, shorthand version of components written as pure functions that returns JSX. Introducing the __Stateless Component__ released with React .14. Before we begin, we should talk a little about pure functions and some modern styles of javascript that will you will likely see used in conjunction with these Stateless Components.

Let's write some __pure functions__ so we feel comfortable with the goal. First, a typical function expression in ES5:
```javascript
var adder = function(a,b) {
  return a + b;
}
```

Functional programmers care about __immutibility__, so these days you may see the same function assigned to a __const__ (ES2015) so that the same varable can't be reassigned at a later time in your program. The more protection the better.

```javascript
const adder = function(a,b) {
  return a + b;
}
```

we can shorten (and pontentially fix 'this' context issues if applicable) by using the ES2015 __arrow function__ instead.

```javascript
const adder = (a,b) => {
  return a + b;
}
```

That's great but actually we can do it in one line.

```javascript
const adder = (a,b) => a + b
```

The return statement is now unneeded but instead implied. It simply returns the value of a + b. We don't even need the brackets. It's a beautiful one-line function expression that has no side affects. It takes arguments and returns a value.

How would we make this unpure?
```javascript
const adder = (a,b) => {
 console.log(a);
 return a + b;
}
```
Why is this __impure__? Calling console.log(a) could've created a __side effect__ in our app. Sure we returned a value still but who knows what else changed in our app. That's the take away. _Who knows_. It's better to know and if we write pure functions we can get _predictable results_ and know exactly how our app has changed. Back to our pure function.

```javascript
const adder = (a,b) => a + b
```

We may have assumed that the function above accepts Numbers as arguments and returns a value that is a Number. Now we want to change things. We want our function to take an Object and add the sum of two of its properties.

```javascript
/**
* @param {object} props
* @returns {number}
*/
const adder = (props) => props.pastRevenues + props.newRevenues
```

well there's actually a shorthand for the above that lets you remove `props` from `props.pastRevenues` and `props.newRevenues`. Goes like this:

```javascript
const adder = ({pastRevenues, newRevenues}) => pastRevenues + newRevenues
```

How is that the same thing? Well with object destructuring ES2015 we can create a function that takes an object that immediately assign variables that map to the value of the property of the object with a matching name:

```javascript
const x = { pastRevenues: 545, newRevenues: 100,000,000 };
const adder = ({pastRevenues, newRevenues}) => pastRevenues + newRevenues;
adder(x); // 100,000,545
```

One line is great, but sometimes people make some space for clarity:

```javascript
const MyComponent = (
  {
    pastRevenues,
    newRevenues,
    clickAction,
  }
) => {
  return (
    <a onClick="clickAction">
      <span>
        Past Revenues: {pastRevenues}
      </span>
      <span>
        New Revenues: {newRevenues}
      </span>
    </a>
  );
};
```

See what we did just did there? We __wrote a Stateless Component__ in React. It takes an object and returns JSX, filling in fields and callbacks with the properties of the object that is passed in. Object? Here's how to use the above component when writting JSX.

```javascript
const clickActionFunc = () => console.log('hi');
const valueOne = 500000;
const valueTwo = 352;

return (
  <MyComponent clickAction={clickActionFunc} pastRevenues={valueOne} newRevenues={valueTwo} />
);
```

Now compare this, a typical React component:

```javascript
import React from 'react';

class MyComponent extends React.Component {
  render() {
    return (
      <a onClick="this.props.clickAction">
        <span>
          Past Revenues: {this.props.pastRevenues}
        </span>
        <span>
          New Revenues: {this.props.newRevenues}
        </span>
      </a>
    );
  }
}
```

To this, our Stateless Component written as a pure function:

```javascript
const MyComponent = (
  {
    pastRevenues,
    newRevenues,
    clickAction,
  }
) => {
  return (
    <a onClick="clickAction">
      <span>
        Past Revenues: {pastRevenues}
      </span>
      <span>
        New Revenues: {newRevenues}
      </span>
    </a>
  );
};
```

Clean. Pure. Simple.

A pure function that doesn't rely on Class inheritance, declares the arguments/props at the top (easier to read, understand), doesn't require the importing of the react library (at least in this component's file), and has a shortened syntax. It may not be the right choice for all components but for dumb/display components it is a great option.

-------
Contributors: Brant Barger, Allan Cutler

Catch a typo or have a change? Would love it if you would make a pull request to this repo:

https://github.com/brantphoto/tutorials

Thanks!!

