---
layout: post
title: "Encora Academy Week #27"
---

This week began quite similar to the end of the previous one: migrating styles to styled components. At this point, I think that it is quite evident that I really like this library.

MEME

The first version of the flashcards app for practicing answering interview questions only included behavioral topics. Since there is not really a right or wrong answer to these type of questions, I just cared about showing a timer so users can be aware of it and adjust their answers length if necessary.

However, for technical questions, there is some sort of right and wrong answers so I wanted to implement a UI feature in the cards that allows to show an example of a correct answer so users can check it if they did not know it or if they just want to check after rehearsing their answer.  

In the beginning I thought about flipping the cards. However, I did not succeed in two of my attempts to implement this effect. I decided that the priority was to have something useful soon so it was better to try something simpler so I tried implementing a switch button for showing the answer.

I found that a common implementation uses the `checkbox` type with the `input` HTML tag to take advantage of the checked CSS pseudo-class of these elements and apply the changing properties of the switch based on that checked/unchecked status. As the `onClick` action would also trigger a change in the state of the component, I found that the CSS `checked` pseudo-class was not getting updated when props in the component changed, they would update only after a click. I found a workaround by changing the properties dynamically and in this way I did not need to use the `checked` property so it was not necessary to use an `input checkbox` element (a disadvantage of this is that the checkbox actually shows so you have to hide it behind the switch) so I decided that a `button` would be more appropiate. I found the appropiate way to make this accesible in the aria specification.

I also got a challenge from a client so that was the core activity at the end of my week. I am not sure if I can talk about it with so much detail here but I think it is safe to say that it was my first approximation to GraphQL since it involved consuming data from an API that uses this technology so I learned about Apollo that is a popular client for GraphQL. It was also my first time writing unit tests for API requests so I learned that it is necessary to mock the API in order to test it and there are [libraries](https://mswjs.io/docs/getting-started/mocks/graphql-api) like `msw` that help you to do that.
