## React Life Cycle Method (Forbidden Jutsu)

![](https://c.tenor.com/VXJeIRgdC_oAAAAM/naruto-rasengan.gif)

### Knowing the** Lifecycle method** can be very handy not only in the development lifecycle but also in** Interviews.**

Generally, Life Cycles method exist in class-based component but, you can also use them in function-based component also by the help of react Hooks.


![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1628655538631/HrDZZuznH.png)

These are the things happen from top to bottom when your react component runs.

 [Here is the sandBox link To see LifeCycle Methods in action](https://codesandbox.io/s/beautiful-river-ri5om?file=/src/index.js)

The above LifeCycle Method is from the creation phase when your react component is rendered the first time


![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1628656501116/6M9AB6pBQ.png)

and these lifecycle method runs in update phase,means when something is updating in your react tree

## useEffect LifeCycle Method

### ComponentDidMount
Good for making network calls and things you want to do at start time of app
~~~
useEffect(() => {
  /* ComponentDidMount code */
}, []);
~~~

### ComponentDidUpdate
Update thing when new props comes
~~~
useEffect(() => {
  /* componentDidUpdate code */
}, [var1, var2]);
~~~

### ComponentWillUnmount
use for cleanUp
~~~
useEffect(() => {
  return () => {
   /* componentWillUnmount code */
  }
}, []);
~~~

### All Three Combined
~~~
useEffect(() => {

  /* componentDidMount code + componentDidUpdate code */

  return () => {
   /* componentWillUnmount code */
  }
}, [var1, var2]);
~~~

#### Reference: Academind

## What do you think about these LifeCycles Methods ??