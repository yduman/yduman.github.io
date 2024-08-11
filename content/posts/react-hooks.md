+++
authors = ["Yadullah Duman"]
title = "Get Hooked with React Hooks"
date = "2018-10-26"
description = "Insight of the React Hooks API, after it was presented in ReactConf"
categories = ["React"]
tags = [
    "React",
    "JS",
    "Frontend"
]
+++

So yesterday [ReactConf](https://conf.reactjs.org/) started with the opening keynote of Sophie Alpert and Dan Abramov about “React Today and Tomorrow”. Dan Abramov introduced [Hooks](https://reactjs.org/docs/hooks-intro.html), which is currently in React v16.7.0-alpha and in a RFC status. I was really hooked (pun intended) about the new possibilites with the new API and many library developers immediately started to try it out and they were amazed too.

## **Why Hooks?**

The React team observed over time, that it is hard to reuse stateful logic between components. Many developers used patterns like render props and higher-order components to solve this, but most of the time you ended up in a “wrapper hell”. The Hooks API will allow developers to reuse stateful logic without changing the component hierarchy.

Another problem to solve was that if your class components grew more and more, you had logic in `componentDidMount`, `componentDidUpdate`, `componentWillUnmount` etc. Over time your components become more complex and you have to stay in sync with all of your lifecycle methods in order to prevent bugs. Furthermore you couldn’t just extract smaller components from your complex component because the stateful logic is spreaded accross it. To solve this issue, the Hooks API will let you split a component into smaller ones because everything is a function and you simply extract your logic into another function.

## **What are Hooks?**

The new Hooks API enables developers to *use* state and side effects in functional components. There are several Hook functions you can use right away and there will be probably more to come until a stable release. Below you will see a simple counter component using the `useState` Hook.

### **State Hooks**

```jsx
import React, { useState } from 'react'

function Counter() {
  const [count, setCount] = useState(0)
  
  function increment() {
    setCount(count + 1)
  }
  
  return <button onClick={increment}>{count}</button>
}
```

Here `useState` gets an argument, which is the initial state and returns a pair containing the current state and the function to update the state.

### **Effect Hooks**

To perform side effects in your components you will make use of the `useEffect` Hook. It behaves like `componentDidMount`, `componentDidUpdate` and `componentWillUnmount` combined in one function (crazy right?). If you want to clean up (e.g. removing event listeners) you just return a function and do your clean up in the function body.

```jsx
import React, { useState, useEffect } from 'react'

function Counter() {
  const [count, setCount] = useState(0)
  
  function increment() {
    setCount(count + 1);
  }

  useEffect(() => {
    document.title = `You clicked ${count} times`
  })

  return <button onClick={increment}>Click me</button>
}
```

### **Reuse Stateful Logic with Custom Hooks**

Now here comes the fun part. You can write custom Hooks and reuse it everywhere you need them! The convention for writing a custom Hook is that the name starts with “use” and that it may call other Hooks. Below we will create a custom `useCounter` Hook which you can use across components.

```jsx
import React, { useState } from 'react'

function useCounter(initialCount, step) {
  const [count, setCount] = useState(initialCount)

  function increment() {
    setCount(count + step);
  }

  return { count, increment }
}

function CounterOne() {
  const { count, increment } = useCounter(5, 1)
  return <button onClick={increment}>{count}</button>
}

function CounterTwo() {
  const { count, increment } = useCounter(5, 2)
  return <button onClick={increment}>{count}</button>
}
```

## **Which Hooks are available?**

The current [Hooks API Reference](https://reactjs.org/docs/hooks-reference.html) lists the following Hooks:

- Basic Hooks
    - `useState`
    - `useEffect`
    - `useContext`
- Additional Hooks
    - `useReducer`
    - `useCallback`
    - `useMemo`
    - `useRef`
    - `useImperativeMethods`
    - `useMutationEffect`
    - `useLayoutEffect`

## **Conclusion**

To sum it up, I believe that Hooks will enable React developers to write code which is more expressive and more declarative. Even the fact that you can now simply write custom Hooks to enable reusability among your components makes testing also a lot easier. Furthermore I believe that things like static code analysis, tooling and bundling capabilities will benefit from it. All in all I really appreciate the dedication of the React team trying to observe problems we developers encounter and try to provide the best solution possible in order to ensure great experience using React. Ryan Florence mentioned in his talk after something that I could really relate to, which was that to this day he never experienced boredom when writing React and that he’s even more hyped about future stuff coming to the React Ecosystem.