---
layout: post
title: "Encora Academy Week #24"
---

At the beginning of this week at the Academy/Apprenticeship we finished the last details of a ReactJS project. We migrated it to TypeScript and tried to tidy up our codebase as much as possible.

During this process I learned some more about styling Material UI components. I was trying to refactor a component where I had implemented the style in a kind of awkward way so the code did not look clean. In Material UI, usually you define the styles outside the component and then, inside the component you call the styles function. Since I wanted to make a reusable component, some of its styling depended on the props that were passed to it. In the beginning I could not figure out how to pull these prop values as arguments for the styles functions so I defined them inside the component, hence why it looked weird. 

After searching for a while and trying different approaches from the internet, I found that you don't have to pass the props as arguments in the styles function. You just need to use props as an arrow function wherever you need to use its values.

I have also been preparing for an upcoming interview. I went back to my interview preparation notes and did a sketch of my recent technical experiences, the technologies that I used and the main insights that I gained. This helped me to synthetize a pitch that I can use for example when asked 'tell me about yourself'. I always struggle with this apparently broad questions and hadn't had much luck drafting an adequate response. Having an schematic note of my experience helped me to structure it.

I know that I need to keep practicing so I started a mini-project in React to display flashcards with random interview questions. It is still in progress but I think that it is a good way to keep improving in my front end skills while also getting familiar with common interview questions.
