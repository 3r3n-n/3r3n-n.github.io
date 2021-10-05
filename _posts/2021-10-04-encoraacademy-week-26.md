---
layout: post
title: "Encora Academy Week #26"
---

This week has been quite fun. 

## Mini-deployment

It began by deploying the flashcards app (that I mentioned in the previous post) to Netlify. Before deploying, you need to build your app, in this case with `npm run build`, which creates a build directory with a production build of your app. When you deploy (through the CLI or the web UI), you need to set the build sub-directory as the path to deploy. I did not need to do much more since the app is quite small and there are no environment variables that need to be configured. So [it is now online](https://serene-brahmagupta-6277ab.netlify.app/), it made me really happy when I was able to access the app with my phone.

I sent the link to a friend and my mentor to get some initial feedback. Based on that, I added the intro text at the beginning to give some context to the users and adjusted the card colors. Then I added some media query breakpoints to make the app responsive and thus, mobile-friendly since my plan is being able to practice on my own when I don't have access to a computer (in public transport, for example).

## More styles tools

I used [css modules](https://create-react-app.dev/docs/adding-a-css-modules-stylesheet/) to style the flashcards app so it was time to try [styled components](https://styled-components.com/docs). I had learned a bit about it during the react school, we were shown a quick example. At that time, I thought that the syntax was quite weird and did not feel compelled to try it. I drastically changed my perspective after reading the quickstart guide from the documentation. Adding dynamic properties to the styles through props is much more straight forward, for example. 

So the conclusion from this week is that I really like styled components. I have also been reviewing some react topics and just decided to migrate the flashcards app to styled components too, since I am planning to add more features so I think that it will be more convenient.
