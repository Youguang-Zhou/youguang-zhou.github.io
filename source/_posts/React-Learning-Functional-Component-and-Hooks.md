---
title: 'React Learning: Functional Component and Hooks'
date: 2020-06-08 20:30:07
tags:
- React
- Hooks
categories:
- [React, Notes]
---

*React is a mature web framework developed by facebook.*

----------------------------------------

# Functional Component 
In functional component, `useState()` is used to declare a state, and `return` method is used to render UI:

    const MyComponent = (props) => {
	    const [theState, setState] = useState(0);

        const someFunction = () => {
            // do something
        }

        return (
            <div>{theState}</div>
        );
    };

Initialize the state by passing parameter in the `useState()`. In this case, `theState` is initialized with 0.

Here `useState()` is a kind of **hook**, particularly used to declare the state.

# Hooks
Component is designed to reuse UI, and hook is designed to reuse the logic.
There are many other hooks in React, such as:
- ## useEffect
    The *lifecycle* in Class Component(e.g. `componentDidMount` and `componentDidUpdate` method) is replaced by `useEffect` hook in Functional Component. The function passed in `useEffect` will run after each render if the second parameter in `useEffect` is null. Otherwise, you can pass an array as the second parameter, then `useEffect` will only re-render the component if and only if the variables in array are changed. For example:

        useEffect(() => {
            // get next page content
        }, [pageNo]);

    In this case, `useEffect` will only run when `pageNo` is changed.
    If the second parameter is an empty array, then `useEffect` will only run when the component is mounted and unmounted:

        useEffect(() => {
            // behave like componentDidMount
        }, []);

    Besides, you can return something from `useEffect` to clean up your code in case memory leak:

        useEffect(() => {
            const subscription = props.source.subscribe();
            return () => {
                subscription.unsubscribe();
            };
        }, []);

    However, considering the performance problem, the operations in `useEffect` is asynchronous. Assume the `count` in the following example is 0 initially:

        useEffect(() => {
            console.log(count);  // ---> 0
            setCount(count+1);
            console.log(count);  // ---> still 0
        }, []);

    Finally, `useEffect` is often used to fetch API.
- ## useContext
- ## useReducer
- ## useRef
......

### Check React documentation for more details: https://reactjs.org/docs/hooks-reference.html