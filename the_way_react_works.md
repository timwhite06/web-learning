# React

All React does is create a tree of elements:
- This is very fast, because React elements are just plain JavaScript objects.
- This is happening when we call the render() method.

## Reconcilitation - Virtual DOM
Reconciliation is the process of reflecting changes from a frameworks' virtual DOM into the DOM.

- This tree of elements is kept in memory, it is the virtual DOM.
- The next thing to do is to sync the virtual DOM with the real DOM.
- During the initial render, there is no other way than to insert the full tree - this is very expensive.

### What if the tree of elements changes?
- React can't just re-render the whole tree - this is very expensive.
-   React gets the old tree and generates the new tree...
-   ...it then compares the trees and finds the smallest number of operations to transform one tree into the other...
-   ...this is handled by the diffing algorithm.

### Diffing Algorithm
#### Normally
- O(n^3)
-   n = the number of elements.
-   So if you had 1000 elements, it would require 1,000,000,000 operations to render the whole tree (not ideal).

#### React does it like this
- n elements => n operations
-   1,000 elements => 1,000 operations...
-   ...but React does it in a smaller amount on operations than this because of the diffing algorithm's assumptions.

#### Diffing Algorithm's assumptions
- Two elements of different types will produce different trees.
- When we have a list of child elements which often changes, we should provide a unique "key" as a prop [Note](./react-performance.md).

### Rendering
- Most of the actual implementation lives in the renderers...
- ...they begin the reconciliation process...
- ...they generate the tree of elements and insert it wherever it has to be inserted.

- React doesn't know that the elements are going to end up in a web browser...
- ...therefore, React is compatible with any renderer and you can even create your own - using the react-test-renederer package!

#### React communicating with the renderers
##### Setting a state | A brief intro
```
// Inside React DOM
const instance = new Component() // Instance if a class is created
instance.props = props;
instance.updater = ReactDOMUpdater; // Afterwards, the updater field is being set to something that is part of React DOM


// Then whenever we call setState, a function similar to this is called:
setState(partialState, callback) {
  this.updater.enqueueSetState(this, partialState, callback); // this is very complicated and is something to research as a whole topic.
}
```

##### React Hooks | A brief intro
- Instead of an updater field, they use a dispatcher object
```
const React = {
  // ...
  __currentDispatcher: null,
  
  useState(initialState) {
    return React.__currentDispatcher.useState(initialState);
  },
  // ...
};
```
- When useState is called, the call is forwarded to something called current dispatcher...
- ...and this dispatcher, is once again, set by React DOM.

Whenever a component is rendered, React DOM sets the dispatcher similarly to this:
```
// Inside React DOM
const previousDispatcher = React.__currentDispatcher;
React.__currentDispatcher = ReactDOMDispatcher;
let result;
try {
  result = Component(props);
} finally {
  React.__currentDispatcher = previousDispatcher;
}
```
- This time around, the component is called, because it is a function.

# Conclusion
- React on its own only gives us the means of expression, so we can define components and so on, and it does the diffing part.
