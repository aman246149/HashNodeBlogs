## Context API + Use Reducer gives Redux Feeling😍😍

If you are a beginner to state management and want to manage your react app state like a pro but don't want to use Redux as  it has much complexity like:
1. take time to set up, 
2. create a store
3. dispatch action, make reducers,
4.  mapDispatchToProps or mapStateToProps etc

It is not a good idea in my opinion to use redux for a small application when you have an inbuilt state management solution provided by React that is **Context Api**.


![](https://easybase.io/assets/images/posts_images/5-great-react-libraries-1.png)

## You can use context Api with UseReducer in four steps
1.  **create context by using CreateContext()**,This gives us provider and consumer which we can use to provide and consume data in other components

~~~
import {createContext}  from "react"

const todoContext=createContext()

export default todoContext
~~~

2- create a **component** which uses your above **todocontext** and help you to wrap your top-level parent so that you can pass data to any child component and get data back from the child to the parent component.

~~~
import TodoContext from "./todoContext";
const AppState = (props) => {

  const addTodo = (todo) => {
  };

  const toggleTodo = (id) => {
  };

  const deleteTodo = (id) => {
  };

  const [state, dispatch] = useReducer(reducer, intialState);
  return (
    <TodoContext.Provider
      value={{ todos: state.todos, addTodo, toggleTodo, deleteTodo }}
    >
      {props.children}
    </TodoContext.Provider>
  );
};

export default AppState;

~~~

As you can see my TodoContext.provider has a value property that he uses to pass the data and update the data in components.

3- **Create a Reducer file** -- Basically, it is a function that has 2 parameters, state, and action. </br>
the state is used to update or manipulate the state where an action is an object where we have action type and payload.</br>
through action type, we can tell our reducer which state we want to change and for payload, it only contains data, which we want to update our state.</br>
To understand more about useReducer and Reducer read this.  [useReducer](https://amandevblogs.hashnode.dev/easy-explanation-of-use-reducer-hook) 

~~~
import { ADD_TODO, TOGGLE_TODO, DELETE_TODO } from "./Actions";

//as we know reducer is a function that is used to manipulate the state

const reducer = (state, action) => {
  switch (action.type) {
    case ADD_TODO:
      return {
        ...state,
        todos: [...state.todos, action.payload]
      };


    default:
      return state;
  }
};

export default reducer;

~~~

4- There is also a good practice where we put our Actions(actions means which operation we want to perform basically it's a String which we pass in our reducer ,so that our reducer can distinguish different operations)

~~~
 export const ADD_TODO = "ADD_TODO";
export const TOGGLE_TODO = "TOGGLE_TODO";
export const DELETE_TODO = "DELETE_TODO";
~~~

5- Now this is the last step where you learn how to update your state from child components and get state object to display output.
</br>
To use our context API in child component we only need

~~~
import { useContext } from "react";
import todoCtx from "../../context/todoContext";
~~~
then we pass our todoCtx in useContext  Hook
~~~
  const { todos, toggleTodo, deleteTodo } = useContext(todoCtx);

~~~

Here you can see we take the value from our todo context (step 2) and we can use these in our component.

~~~
import TodoItem from "../TodoItem/TodoItem";
import { useContext } from "react";
import todoCtx from "../../context/todoContext";

const TodoList = () => {
  const { todos, toggleTodo, deleteTodo } = useContext(todoCtx);
  return (
    <ul>
      {todos.map((todo) => {
        return (
          <TodoItem
            key={todo.id}
            text={todo.text}
            complete={todo.isComplete}
            clickToToggle={() => toggleTodo(todo.id)}
            clickToDelete={() => deleteTodo(todo.id)}
          />
        );
      })}
    </ul>
  );
};

export default TodoList;

~~~

 [CODE SANDBOX LINK](https://codesandbox.io/s/fancy-cdn-yw0jv) 

## Let me know what's your opinion about Redux vs Context API which is good in your opinion. 😅 😅 😅 😅
