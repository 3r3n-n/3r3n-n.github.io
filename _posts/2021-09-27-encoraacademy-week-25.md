---
layout: post
title: "Encora Academy Week #25"
---

This week has been kind of a transition week. I began by following up on the small flashcards app that I started building the previous week (in ReactJS). It worked fine with a constant countdown start time that I set inside the respective component and the next step was setting a dynamic start time depending on the questions, because, as I was reading, [some interview questions require more time than others](https://interviewgenie.com/blog-1/2017/6/20/how-long-should-your-interview-answers-be#:~:text=What%20length%20is%20the%20right,questions%20should%20be%20the%20shortest.) so it is better if you practice with that into account. 

As the start time would be passed as props to the component, I started running into trouble with the state, it was not updating properly. I struggled with this, modifying my code multiple times during a whole afternoon going in circles and then I realized that I was not using the useEffect hook properly, or at least not really taking advantage of it.

I solved my problem but I still did not fully understand why my first approach did not work whereas the second one did. Thus I switched to read more about react hooks, particularly useEffect.

I spent the rest of the week reviewing some JS topics from my notes and going to pick up the new computer, I have been setting up my environment.
