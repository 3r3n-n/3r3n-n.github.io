---
layout: post
title: "Encora Academy Week #8"
---

As usual, this week has been hard but quite rewarding.

I learned a couple of significant lessons so I will list them here.

At the beginning of the week, we felt that we had finally reached a good progress rate... however, it was still not enough to catch up with our backlog from the previous week.

 In my case, I was still trying to get some functionalities working in my React component. 

And this brings me to my biggest mistake of this week: when we started working on frontend form validation, I was trying to implement our validations with vanilla JavaScript... however, I was having trouble implementing a minimum date limit. That same morning, some of our mates told us that they were using formik and yup libraries in their project, so I decided to have a look at them and checked the documentation and a couple of examples to see if that could help me with the date validation that I was struggling with. At first glance, it seemed to me like including these libraries would make it so much easier and quicker so I proposed this to my teammates and we decided to give it a try. 

I started running into trouble with some nested data in formik, one of my mentors helped me to figure it out. The validation was done. However, when I tried to implement another functionality (for editing some data) into my component, I started getting stuck with some errors and I did not exactly know what was happening, since the lines where the problem lied had to do with the formik library methods. At this point, by looking at other examples, I had figured the date validation out with vanilla JavaScript so my initial motivation for using formik and yup was no more... and it was not providing any other advantage... I was not 'saving' lines of code and it was not significantly easier to understand. This made me realize that incorporating these libraries had been a mistake... at least from me, since I am not fluent in basic JavaScript at least. I talked to my teammates about this and no one else felt like these libraries were making our lives easier so we decided to remove these libraries from our code. And, the consequence of this was that it took me longer to fix the error that  had (I still got a ton of help from one of my mentors)... at the end I completely re-wrote my code in vanilla JavaScript. It was a good exercise and a significant learning experience but it consumed valuable time since we are on a tight schedule.

I have also been learning how to make better pull requests. I got feedback from my teammates and my mentors. I learned that a good pull request starts by making adequate commits: as often as I complete an step or an 'snapshot' that would be useful in the future, so that is why commit descriptions are so important. I understood this when I had to go back to a previous commit and fortunately, I was able to find it quickly thanks to the comments.  

This has required a mindset switch from my part, by remembering as often as possible that I am not writing code for a personal project, I am writing code as part of a collective project so I have to make everything as clear as possible for others. So it is another habit that I must keep developing, I feel that I have improved a lot this week but I must keep working on it. 

By the end of the week, we still had not caught up with the work that we had planned to finish at that point. We were anxious about what our client would say but we showed our progress anyway and were open about what we still needed to do and the adjustments that we would need to make in order to deliver the most valuable system that was achievable in the time that we had left. 

We talked about this before presenting the demo to our client, I found it a bit hard to reformulate what we had planned: how would we simplify this plan to make it achievable in the following week and at the same time keep providing value to our client ? In order to answer this question, it is necessary to be aware of the technical implications of any modification that we would make to our plans so my more experienced teammates were more aware of this and they helped to come out with ideas for adjustments. Again, I feel really fortunate to learn from them and to be supported by my mentors. At the end, we are seeing that the first iteration of something is never the final one and we need to keep trying and getting feedback so the solution keeps getting better.
