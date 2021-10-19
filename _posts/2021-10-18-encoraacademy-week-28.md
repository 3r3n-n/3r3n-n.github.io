---
layout: post
title: "Encora Academy Week #28"
---

Last week I noticed that I had a lot to catch up with asynchrony in JS, as well as consuming data from APIs. Thus, this week I started a project to focus on this.

I spent some time looking at public APIs to see which ideas I could come with for a new personal project. Last week I got my first experience with GraphQL but before diving deeper into it I wanted to practice with REST APIs a bit more, so in the end I chose the Spotify API because its documentation is quite friendly. 

This API requires you to authenticate first and it has different authentication types depending on the type of data that you want to consume. As I did not need to access users-related data, just content data, I just needed a token that would allow me to make successful requests. One thing that I realized is that, in order to get this token, I need to make the requests from a server, not a client so my project would need to have a small backend to fetch these tokens. So I needed to refresh my very basic knowledge of node.JS to implement this part.

After getting the token, the other requests can be made from the client so I wrote some axios-based functions to do this. Then I incorporated them into a promise chain. 

After a couple of days of implementing this, now I have the data that I need and it is time to display it inside the frontend using a library that I have been wanting to learn for a while: D3.
