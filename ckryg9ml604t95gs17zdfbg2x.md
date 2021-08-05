## Easy Explanation Of USE REDUCER Hook

If you are familiar with Use State Hook then use Reducer Hook will be a child Play For you.

As we all know useState is simply updating our state, in the same way, useReducer is also manages our state or we can say that it can manage a more complex state.

Let's Go and see side by side comparison of Use State and Use Reducer Hook

##some similarities between these two hooks

| useState 🔴IMPORTANT❗🔴      | UseReducer🔥Cool🔥  |
| ----------- | ----------- |
| **[state,setUpdateState=usestate()]**      | **[state,dispatch]=useReducer()  **    |
| here state is used in our component to render output   | here also state is used in our| 
|                                                                                 | component to render output |
|                                                                                 |                                                             |
| setUpdateState is used to update the state | dispatch is used to update the state|
|                                                                                 |                                                             |
| useState takes **initialState** as an input |  useReducer takes 2 input|
|                                                                                 | 1- **ReducerFunction** |
|                                                                                 |2- **initialState**  |


## UseReducerHook


<iframe src="https://codesandbox.io/embed/determined-thunder-nihul?fontsize=14&hidenavigation=1&theme=dark"
     style="width:100%; height:500px; border:0; border-radius: 4px; overflow:hidden;"
     title="determined-thunder-nihul"
     allow="accelerometer; ambient-light-sensor; camera; encrypted-media; geolocation; gyroscope; hid; microphone; midi; payment; usb; vr; xr-spatial-tracking"
     sandbox="allow-forms allow-modals allow-popups allow-presentation allow-same-origin allow-scripts"
   ></iframe>

## Here is a simple counter app using UseReducer hook

Here are some 6 steps that you should follow to use UseReduceHook
1. Import it from react </br>
```import { useReducer } from "react";
```
2. use your useReducerHook</br>
```  const [state, dispatch] = useReducer(reducerFunction, initialState);
```
3. Define your **initial state**
```const initialState = {
    count: 0
  };
```

~~~
4. Define Your reducerFunction so that you can pass it in your useRducerHook
```
 const reducerFunction = (state, action) => {
    switch (action.type) {
      case "increment":
        return {
          ...state,
          count: state.count + 1
        };
      case "decrement":
        return {
          ...state,
          count: state.count - 1
        };

      default:
        return state;
    }
  };

```
~~~





5-> 
For increment or Decrement value Onclick use Your **dispatch method**

~~~
```
<button onClick={() => dispatch({ type: "increment" })}>Increment</button>
      <button onClick={() => dispatch({ type: "decrement" })}>Decrement</button>
``` 
~~~
 


6->
To use state in your component use your state
```
~~~
      <h3>value:{state.count}</h3>
``
~~~


##  Conclusion 
follow above 6 steps to use USEREDUCER HOOK
















