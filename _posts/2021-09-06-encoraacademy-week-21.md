---
layout: post
title: "Encora Academy Week #22"
--- 

This week in Encora Academy/Apprenticeship I kept learning about React (styling, routing and state management) in the course that we are taking.

CSS is the most obvious tool when it comes to styling frontend elements regardless of the JS framework that we might be using. However, there are tools that involve CSS but are more specific and, depending on the case, might be more useful. For example, there are CSS modules and styled components. In vanilla CSS, we might think about writing an stylesheet for the whole project but it can get messy quite quickly, it is not so easy to scope our classes. With CSS Modules, all class names and animation names are scoped locally by default. When using CSS Modules, each React component is provided with its own CSS file, that is scoped to that file and component alone. styled-components is a library for React that allows you to use component-level styles in your application that are written with a mixture of JavaScript and CSS using a technique called CSS-in-JS.

With regards to routing, we learned about react-router-dom. It is a library that allows you to implement your routes through components. It has a structure that is similar to switch-case. 

During most of the week I learned about redux in React. Redux is a library that helps to manage the state inside an application. This is helpful when your app is complex enough that you have data that needs to be shared among a significant number of components so it would be quite impractical to lift state because it would cause a lot of prop drilling so redux is a good choice in cases like this. Methods in redux must be pure functions so they cannot have any side effects. But then, what do we do with, for example, API requests or other side effects that involve communicating with elements outside the app ? This is when the redux-sagas library comes handy. I have been struggling a lot to understand the sagas concepts but the project that we are making has been helpful as an exercise so now I feel that I can grasp at least the very basics of it to handle some simple middlewares.

All these concepts have helped me to strengthen my React skills, I feel like now I can go to past projects, read the codebase and understand something that I didn't before, which feels quite good and motivating.
