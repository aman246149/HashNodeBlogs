## Redux is so simple ? Wanna know How I Learned Redux

## Most of the people directly learning redux, and got confused
1.  how it works?
2. what is the reducer function?
3. What are the actions?
4. what are the constants?

Instead of  directly goes into these topics, an individual should know the concept of global scope and component level scope,

He should also know about the concept of context Api.
###  By learning the Concept of Context Api with the  Reducer function you will get knowledge about
1. Concept of global state or Store.
2. What is the initial State and what is the responsibility of useReducer Function
3.You also learn about useReducer Hook
4.How to dispatch things.

After Learning Concept Of CONTEXT API. You should understand the Cycle of Redux,
Like from where it starts and where it ends.
For beginners, the cycle is like </br>
** Store hold state->Actions have data what is going to update->Reducers update state**


![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1628695749016/HfHnst4hX.png)

### To use Redux install react-redux package and follow these steps
**first step create a store**
~~~
import { createStore } from "redux";
// create store
const reducer=combineReducers({
  
}) //reducer is responsible for manipulationg the state

const initialState={}

const store=createStore(reducer,initialState)

export default store;
~~~
 
**Second Step create constant file**
These constants are just strings and have to be same in reducer function and action functions .So to prevent any typo we create this constant file

~~~
export const PRODUCT_LIST_SUCCESS="PRODUCT_LIST_SUCCESS"
export const PRODUCT_LIST_REQUEST="PRODUCT_LIST_REQUEST"
export const PRODUCT_LIST_FAIL="PRODUCT_LIST_FAIL"
~~~

**Third step create reducer function**
~~~
import {
  PRODUCT_LIST_REQUEST,
  PRODUCT_LIST_SUCCESS,
  PRODUCT_LIST_FAIL,
} from "../Constant/Constant";

//REDUCER IS BASICALLY A FUNCTION WHICH WE USED
// TO MANIPULATE THE STATE

export const ProductListReducer = (state = { products: [] }, action) => {
  switch (action.type) {
    case PRODUCT_LIST_REQUEST:
      return {
        isLoading: true,
        products: [],
      };

    case PRODUCT_LIST_SUCCESS: {
      return {
        isLoading: false,
        products: action.payload,
      };
    }

    case PRODUCT_LIST_FAIL: {
      return {
        isLoading: false,
        error: action.payload,
      };
    }
    default:
      return state;
  }
};
~~~

As you can see reducer functions doesn't do anything it just telling our react app what to render based according to  conditions

**Fourth step create Actions function**
~~~
import {
  PRODUCT_LIST_REQUEST,
  PRODUCT_LIST_SUCCESS,
  PRODUCT_LIST_FAIL,
} from "../Constant/Constant";

import axios from "axios";

//   dispatch actions
//action ->reducer ->state update
export const listProducts = () => async (dispatch) => {
  try {
    dispatch({
      type: PRODUCT_LIST_REQUEST,
    });

    const { data } = await axios.get("/api/products");

    dispatch({
      type: PRODUCT_LIST_SUCCESS,
      payload: data,
    });
  } catch (error) {
    dispatch({
      type: PRODUCT_LIST_FAIL,
      payload:
        error.response && error.response.data.message
          ? error.response.data.message
          : error.message,
    });
  }
};

~~~

Actions are the functions where we can implement some logic like I am doing network call here and pass data to my reducer function.

**Fifth step use UseDispatch and UseSelector Hooks from react-redux**
we use the useDispatch hook to call our action and useReducer Hook to call our state which we want to display.

~~~
const HomeScreen = () => {
  const dispatch = useDispatch();
  const productList = useSelector((state) => state.productList);
  const { loading, error, products } = productList;

  useEffect(() => {
    dispatch(listProducts());
  }, [dispatch]);

  return (
    <>
      <h1>Latest Products</h1>
      {loading ? (
        <h1>Loading...</h1>
      ) : error ? (
        <h3>{error}</h3>
      ) : (
        <Row>
          {products.map((product) => {
            return (
              <Col key={product._id} sm={12} md={6} lg={4} xl={3}>
                <Product product={product} />
              </Col>
            );
          })}
        </Row>
      )}
    </>
  );
};

export default HomeScreen;
~~~

### Here is your react-redux in five steps. what do you think about react-redux let me known




