---
layout: post
title: "Encora Academy Week #2"
---

This week we learned a lot of things with regards to software development tools. I have this mental tendency to divide this topic in two main sub groups: the 'technical' and the 'psychological'. However, I don't think this mental shortcut is good because it makes me see them as two independent components. 

Thus, this time I want to try to address them in a more integral way, I think that it would be helpful for my understanding.

One of the toughest topics that we saw this week was [compression](https://www.youtube.com/watch?v=6rnF2Mo80x0) (and related algorithms). The goal of data compression is the removal of redundant data. This reduces the number of bits that are required to represent the information contained within that data. 

Compression is achieved if short codes are assigned to commonly-occurring (high probability) symbols and long codes are assigned to rare symbols. It is easy to see that symbol-specific frequency will be different from one language to another. I was curious about this so I searched a little bit and found some interesting info. For example, [the Huffman compression technique ( remember Huffman's binary tree! ) can be more adequate to compress Arabic texts, whereas the LZW algorithm is better suited for English texts](http://www.m-hikari.com/ces/ces2013/ces1-4-2013/zahranCES1-4-2013.pdf). The LZW algorithm is a variation of LZ78, one of the two lossless data compression algorithms together with LZ77 ([this one was discussed in this week's videos](https://www.youtube.com/watch?v=Jqc418tQDkg)). LZ77 works by looking at substrings and their ocurrence from a starting point; compression is achieved by replacing the repeated ocurrences with references to a single copy of that data existing earlier. 

Markov chains are a way to model the patterns that can be extracted from a file. To me, they look like flow diagrams but their key feature is the probability of taking an specific path. For example: if we were to make a Markov chain with some text, we would look at the frequency of words that appear after others, something like this:

![https://www.soliantconsulting.com/wp-content/uploads/2013/02/Using-JavaScript-generate-text.png](https://www.soliantconsulting.com/wp-content/uploads/2013/02/Using-JavaScript-generate-text.png)

Note that here the probabilities focus on the transitions between elements, not so much on the elements by themselves as independent units. I feel like I still need to grasp more about their compression mechanism. I found this really [cool resource on compression](https://go-compression.github.io/), so I want to keep reading it to solidify my understanding.

Compression is an important element when it comes to performance, as we saw in one of the [videos](https://www.youtube.com/watch?v=8MMmg3bDOjc). Of course, this is not the only factor that we should take into account. For example, we must be really savvy with paint events, sometimes they are just too much. Measuring performance is key, this is concrete way to know what is going on and to evaluate a solution's effectivity. 

Besides that, another key points that came out frequently was team collaboration as a software development tool. This is in opposition to some popular myths about devs, where they seem to be lone geniuses, as Fitzpatrick and Collins-Sussman say in their 2009 [talk](https://www.youtube.com/watch?v=0SARbwvhupQ). On the contrary, the best thing one can do for their career is to collaborate early and often. This has multiple and inmense benefits: it is easier for us to know if our ideas are good or viable; we learn to give and receive feedback and this allows us to improve in more than one aspect of our craft.  

Collaboration is a multi-dimensional tool. Fitzpatrick and Collins-Sussman dive deeper into this in their 2011 [talk](https://www.youtube.com/watch?v=q-7l8cnpI4k) : we need particular considerations for dealing with coworkers in contrast with users, for example; it is important to practice patience with users, as the background is different (and this is key for building a trustworthy relationship). I find their leadership definition to be quite helpful: it is more about making life easier for your team and remove obstacles than just giving orders.

Dave Thomas gives a lot of good advice that relates to this. He also states particular approaches for different people, based on their skill stages. His talk is really good to challenge assumptions that we have about novices and experts, for example. Something that I found specially interesting was the key difference between advanced beginners and competents: independence. This is something that I want to keep at the center of my journey as a priority to work on. 

In contrast, in their 2012 [talk](https://www.youtube.com/watch?v=OTCuYzAw31Y), Fitzpatrick and Collins-Sussman talk not only about what is desirable, but also about what is unfortunately common, real and problematic (at the end of the day, we need to acknowledge where we are to be able to move forward). This talk has a more pragmatic approach that can be summarized in 'choose your battles'. I think this is one of those clichés that are actually useful and it has a lot to do with what we actively choose to pay attention to and why autopilot is so problematic. This is something that I personally want to work in, I feel like I need to develop more my intuition to make certain decisions and be okay with the opportunity cost that comes with some of them. 

Linda Rising has a more academic [approach](https://www.infoq.com/interviews/linda-rising-agile-bonobos/) to team collaboration, she brings some etiological statements to make a case for it. For example, our species seems to be more comfortable working in small teams; this would be a maximum of ten people. I think this makes a lot of sense. Most people have probably been in a situation where organizing something with more than ten people is just chaotic, communication gets difficult and it is not easy to keep track of who is doing what, it is also easier to be less committed. She also explains the conceptual basis for the [agile](https://www.infoq.com/presentations/power-agile-mindset/) mindset in another talk: the key concepts are the fixed and growth mindset. This also resonates a lot with me, since I struggle a lot with letting my shortcomings define me. Acquiring a growth mindset is more about praxis than about just grasping the concept, it is active work and I still have a lot to do.

In addition, Linda Rising focuses on some other concrete techniques in another [talk](https://www.youtube.com/watch?v=IGHhCmdIvuI). She states the value of pair programming, continuous iteration and retrospective, these are key for improvement. These last few weeks I have noticed how new challenges bring back certain things like a reflection of something you did some time ago that did not seem relevant at the time but now it makes way more sense, for example. This retrospective is not passive, it is immersed in practicing and realizing things as you go, which has a lot to do with everything being a [remix](https://www.everythingisaremix.info/watch-the-series/) of something else. And this takes us back to Fitzpatrick and Collins-Sussman initial point: there are not lone geniuses, ideas don't happen in a vaccuum and they are developed within a context that influences the people who execute them. And again, this is why connecting with other people is so important. We won't come up with ideas unless we have a lot of context, feedback and experiences. As she says it in another [talk](https://www.infoq.com/presentations/Perfection-Is-Unrealistic-Linda-Rising/), "software development is a series of experiments / learning adventures". I think it is a very useful and beautiful approach. She talks about the unrealistic pursue of perfection. I think that this is related to fixed mindsets because they (we? hehe) are afraid of accepting failures because they see them as permanent characteristics that cannot be changed. 

Multitasking is another myth that Linda Rising challenges. This constant switch between tasks, even if they are small, is what makes us take way longer to finish something. At the end, it has more to do with us not being in control of what we pay attention to. 

All of the above come handy for example, when we need to choose a programming language for an specific project. [Damien Katz](https://www.youtube.com/watch?v=g9lNzg27P9M) talks about the community factor as something important to keep in mind for this. The community around a certain language can make your life so much easier by generating libraries for specific tasks and solving questions. I think that this element shows how tools for software development can't simply be divided into soft skills and technical skills that go into two separate independent boxes. 

In addition, I am still thrilled with how many things can be done in bash besides just installing programs. I struggled a little bit to understand that short scripting operators and comparison operators are not the same thing. I think there are some non-verbal concepts that we start assuming when we get comfortable with a certain programming language and when we learn a different one, these concepts get challenged but we don't always have the words to articulate how. At least this is that I feel that is happening to me and I think that I need to keep reading about CS concepts because that's probably the root of this problem.