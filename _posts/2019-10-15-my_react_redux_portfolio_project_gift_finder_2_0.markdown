---
layout: post
title:      "My React Redux Portfolio Project: Gift Finder 2.0"
date:       2019-10-15 15:01:28 +0000
permalink:  my_react_redux_portfolio_project_gift_finder_2_0
---


In wrapping up my final project with the Flatiron School, I did some self-reflection on the concepts that did and did not click immediately. The result of that can be quite different depending on who you talk to. One of the consistent themes of confusion for me was the concept of when, how, and where data gets updated. More specifically, when am I modifying my actual database tables versus a more temporary state (in React, this is called the store)? What information does my database need to create an object versus what does the DOM need to display it? The importance of understanding this became vital in my final project - a React Redux frontend with a Rails API backend. I would even say that the concept did not fully click until I was about half-way through the project after hours of debugging, refactoring, and googling whenever I was stuck. I am going to walk you through how I may explain data management in Redux React to myself a month ago when I first began to learn React. 

1. Fetch requests will be made in your action functions. These functions will be called in your components (e.g. the login submit button will call the “addUser” function which will send a POST fetch request to /users path). Everything you learned about Rails APIs still apply - don’t let a new framework throw you off course. Fetch requests methods will still be GET, POST, DELETE, etc. You still need headers and a body that your API can understand. 

2. You will maintain the current state in a store. Your current state does not need to always mirror your database tables. Instead if should reflect exactly the state your frontend needs to display the data you want to display in that moment (e.g. you don’t need to store all users all the time, you may just want your current state to store the current user). The creation, update, and deletion of your database objects will reflect in your store as necessary using your reducers. 

3. Your action functions (#1) and your current state or store (#2) can be assessed by any component using mapDispatchToProps (#1) or mapStateToProps (#2). (e.g. when I create a review on a blog post, the ReviewInput component needs access to the addReview function and the store to see the current user which is me). 

4. Props, which can include elements in your store or action functions, can be passed down from a parent component to a child component. (e.g. I grab the current blog post’s id in my ReviewsContainer component and pass it down to the ReviewsInput component & the Reviews component, which are 2 child components of the ReviewsContainer. Now when I create a new review or just read the existing reviews, both components know the blog post that we are on.) Often when props are passed down from the parent, the parent initially gained these props using the mapDispatchToProps or mapStateToProps functions (#3). 

