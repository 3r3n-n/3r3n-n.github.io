---
layout: post
title: "Encora Academy Week #14"
--- 
This was the fifth week of the third phase of the Academy/Apprenticeship program at Encora: contributing to open source.

I spent most of this week working on writing tests for my fourth contribution, addressing [this issue](https://github.com/nukeop/nuclear/issues/859). 

# React context and tests

In order to test the components that I modified for my contribution, I needed to update the snapshots that already existed. 

Snapshot tests are focused on the UI. According to the react-testing-library documentation:

> A typical snapshot test case renders a UI component, takes a snapshot, then compares it to a reference snapshot file stored alongside the test. The test will fail if the two snapshots do not match: either the change is unexpected, or the reference snapshot needs to be updated to the new version of the UI component.

When a snapshot test fails, part of the information that you get in the terminal is a diff of the reference snapshot and the actual snapshot that was generated from the files that you just run. One of my mistakes was thinking that I could update the snapshots by manually solving these diffs... but even after that, the error was still happening even though it was showing zero diffs. I had seen how to update the snapshots in the terminal according to the documentation but the `jest` command was not installed in this project (even though it is used for the tests). One of my mentors helped me to understand basic jest configuration: you can modify it in the package.json file or in a jest.config file. A quick workaround for succesfully updating the snapshot was temporarily adding the `filepath pdateSnapshot` snippet to the test command in the `package.json` file and running `npm test`. After this, we would just un-do that change in `package.json`. 

I also struggled with some mock variables that I needed for the functionality test. I was confused as to where they were stored and how to call them. One of my mentors helped me to understand that they were in the context.

The context in react is like a mega-state. In this way you don't need to be sending things through props, which can get quite messy. The disadvantage (and it would be the props advantage) is that it is less straight-forward to track what is in the context. The redux library can help to contain and manage the mega-state, which is the case for this project, so it is an alternative to using the react context.

The variable that I needed to mock was `isMiniPlayerEnabled`, it is a boolean that says whether or not the mini-player is enabled (and its value changes when clicking on the respective button), so it is a conditional render and without this variable being properly called, the `miniPlayer` component would not render in the test and thus no single test would pass.

 This variable is defined in the `useMiniPlayerSettings` function (in the `hooks` file in the `MiniPlayerContainer` folder):

```jsx
const settings = useSelector(settingsSelector);

const isMiniPlayerEnabled = _.get(settings, 'miniPlayer');
```

So it is stored inside the `settings` object, and the way to retrieve this object from the state is with the redux `useSelector` function.

Thus, the way to mock this in the test was inserting the following in the component props: 

```jsx
settings: {miniPlayer: true}
```

Before understanting this, I was previously trying with just `{miniPlayer: true}` (without the settings key and, obviously, without success).

So now it was all good. Functionality worked and all tests were passing (already-existing ones and newly-written ones).

# some GIT drama

There is a script in the project that automatically updates some translation files in this project. This contribution would not modify anything related to the locale (in contrast with the previous contribution to this same project) so I found it weird when they were automatically updated. I included these automatic updates in [my PR](https://github.com/nukeop/nuclear/pull/1006) without thinking much about it (I should have been more mindful about this). 

When submitting my PR, I noticed that there were conflicts since I had not updated my fork so I did that... however the conflicts were so many (in files that I had no idea that they did) that I decided to look for a git command to just override my branch with master latest commit and then add my contribution changes on top of it. However, this was a bad idea from my part and the maintainers asked me to undo this, so I did a `git reset`.

After this, the maintainers requested that I removed the changes in the locale files (from the automatic updates). At first, I understood that they were referring to a couple of conflicted files so I addressed these conflicts but a bit later I understood that they did not want any change to the language files and they were referring to the *files changed* tab in the Github web app. Unfortunately, these changes were almost at my very first commit in my branch so I did a lot of reverts to get there (my plan was re-adding my intended changes after having fixed this). I pushed this previous commit to see if the *files changed* number would decrease but it didn't. I was looking for some info in the github documentation to see what was the reference commit for the diffs in that tab (was it the last master commit before the branch was created or was it the most recent master commit?) but I did not succeed. Then I thought that I could reduce the *files changed* number if I pulled the most recent changes from master but that just increased the number. I was so confused and it was already late at night, I felt quite frustrated that something that apparently would be so simple was taking so much time and I still could not figure out how to fix it. So at that point I decided that it was better to explain my confusion to the maintainers and ask them for more granular advice on how to do it. One of them told me that they would be looking into my branch in the following days. 

So that is what has been going on. I am still thinking what would have been the best approach from my part to have avoided this problem.

Something that I hace certainly learned were more git commands, it was my first time doing `git revert`. I also learned about different ways to use `git diff`.
