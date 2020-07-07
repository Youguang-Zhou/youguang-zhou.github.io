---
title: "React Learning: Build My First Single Page Application"
date: 2020-06-08 16:12:07
tags:
- React
categories:
- [React, Notes]
---

*React is a mature web framework developed by facebook.*

----------------------------------------

# Why Use React
Because the great reusable feature, everything in React is a component.

# What is Component
Component looks like the object in object oriented programming.

As we all know, an object has an constructor, some attributes and methods, component is almost the same.

Each component has some properties(called props), its own attributes(called states), and also some methods can be written in the component.

Besides, each component needs to render its UI by using JSX or a HTML snippet.

# Props and States
**Props** are something on the tag:

    <User name="myName" age="myAge" />

here `name` and `age` are the props of User component.

**States** are something inside the component:

    const User = (props) => {
	    const [nameState, setName] = useState(props.name); // <--- state
        const [ageState, setAge] = useState(props.age);    // <--- state

        return (
            <div>name: {nameState}, age: {ageState}</div>  // <--- name: myName, age: myAge
        );
    };

the props on tag(e.g. name, age) can be used to initialize the state(like constructor in object), and it is also a good way to pass variables between each component. `nameState` and `ageState` are the name of those states, `setName` and `setAge` are the setter method of those states.

# Class Component and Functional Component
They are two different ways to write components in the code.

However, because of the new features, *hook*, in the functional components, it is recommended to write functional components because hook makes the code more reusable and flexible.

# SPA --- Single Page Application
SPA is the application that there is only one entry(index.html). Router is often used to switch different pages. Using create-react-app to create a new single page application:

    npx create-react-app my-app
    cd my-app
    npm start

# Virtual DOM in React
The core mechanism in React engine is render. During each render, React will compare the real DOM and its own DOM(or called Virtual DOM) and check if there is any update. If it is, then React will rerender this particular updated part.

Take create-react-app as example, in `public/index.html`, there is a div tag which declares the root of virtual DOM:

    <div id="root"></div>

and in `src/index.js`, there is a render method of ReactDOM:

    ReactDOM.render(
        <App />,
        document.getElementById("root")
    );

This render method means the App component will be rendered in the element whose id is root.

### Check React documentation for more details: https://reactjs.org/docs/hello-world.html