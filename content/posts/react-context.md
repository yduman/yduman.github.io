+++
authors = ["Yadullah Duman"]
title = "How to use the Context API in React with Hooks"
date = "2019-02-15"
description = "A short guide about the Context API in combination with the Hooks API in React"
categories = ["React"]
tags = [
    "React",
    "JS",
    "Frontend"
]
+++

In one of my last posts, I shared the announcement of Hooks by the React team with you. About two weeks ago, the React team finally released the Hooks API with version 16.8. In this blogpost I want to share one approach of how to use the `useContext` Hook along with `useReducer` in order to create and update context. I will not explain how the Context or Hooks API work. If you are not familiar with them, then please read the [documentation](https://reactjs.org/docs/getting-started.html). Furthermore, this post is also just a reference for me, since I was exploring this Hook lately and was fascinated of its ease.

## **Creating Context**

Context is a great way to provide “global” data across your application. It is also a great method to eliminate for example deep Prop-Drilling.

To create Context, we will create a file `AppContext.js` with the following content:

```jsx
// AppContext.js
import React, { useReducer } from "react";

const AppContext = React.createContext();

const initialState = { count: 0 };

const reducer = (state, action) => {
  switch (action.type) {
    case "increment":
      return { ...state, count: state.count + 1 };
    case "decrement":
      return { ...state, count: state.count - 1 };
    case "reset":
      return initialState;
    default:
      return { ...state };
  }
};

const AppContextProvider = props => {
  const [state, dispatch] = useReducer(reducer, initialState);
  const value = { state, dispatch };

  return (
    <AppContext.Provider value={value}>
      {props.children}
    </AppContext.Provider>
  );
}

const AppContextConsumer = AppContext.Consumer;

export { AppContext, AppContextProvider, AppContextConsumer };
```

Here, we have our data `count` that we want to share accross our application. In our `AppContextProvider` function we are using the `useReducer` Hook in order to create a reducer that will handle state management for our context. Then we pass the reducer to our `Provider` object, which we will use to wrap our application.

## **Wrapping our Application**

This step is straight-forward. We will wrap our application root with the `Provider` object, so we can access the context data.

```jsx
// App.js
import React from "react";
import { Router } from "@reach/router";
import { AppContextProvider } from "./AppContext";
import { Home } from "./Home";

const App = () => (
  <AppContextProvider>
    <Router>
      <Home path="/" />
    </Router>
  </AppContextProvider>
);

export default App;
```

## **Making use of our Context**

Here comes the fun part. We will now consume our context and provide functions to update it.

```jsx
// Home.js
import React, { useContext } from "react";
import { AppContext } from "./AppContext";

export const Home = () => {
  const { state, dispatch } = useContext(AppContext);

  const increment = () => dispatch({ type: "increment" });

  const decrement = () => dispatch({ type: "decrement" });

  const reset = () => dispatch({ type: "reset" });

  return (
    <div>
      <p>{"Count is: " + state.count}</p>
      <button onClick={increment}>+</button>
      <button onClick={reset}>reset</button>
      <button onClick={decrement}>-</button>
    </div>
  )
};
```

With `useContext` we consume our context object. Any updates on the `Provider` will trigger a re-render with the updated context value. In order to update our context, we make use of the `dispatch` functions, passing in the type of action we want it to make.