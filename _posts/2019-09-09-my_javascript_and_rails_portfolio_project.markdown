---
layout: post
title:      "My JavaScript & Rails Portfolio Project: Gift Finder"
date:       2019-09-09 17:12:42 -0400
permalink:  my_javascript_and_rails_portfolio_project
---


The final JavaScript portfolio project challenged us to build a Single Page Application (SPA) with an HTML, CSS & JS frontend that communicated with a backend API built with Rails. While object oriented programming in Ruby was quite familiar at this point, object orientation in JavaScript was a new and challenging concept. I am going to take you through early iterations of my Javascript without object orientation and the shift in later iterations towards full object orientation. 

Below is an early version of part of the index.js file, specifically the functions related to displaying a list of gifts onto the initial landing page. Upon **clicking submit** (the event listener tied to the signupForm), the **user is created**, which **calls the loggedInUser** function, which **calls the fetchGifts** function, which finally **calls the putGiftsOnDom** function.

```
signupForm.addEventListener('submit', function(e){
    e.preventDefault()
    fetch(USERS_URL, {
        method: "POST",
        headers: {
            "Content-Type": "application/json",
            Accept: "application/json"
        },
        body: JSON.stringify({
            user: {
                email: signupInputs[0].value,
                password: signupInputs[1].value
            }
        })
    })
    .then(res => res.json())
    .then(function(object){
        if (object.message) {
            alert(object.message)
        }
        else {
        loggedInUser(object)
        }
    }
    )
})

function loggedInUser(object){
    currentUser = object
    signupForm.style.display = 'none'
    welcome.innerHTML = `<h3>Hello, <i>${currentUser.email}</i> !</h3>`
    logout.innerText = "Logout"
    fetchGifts()
}

function fetchGifts(){
    fetch(GIFTS_URL)
    .then(res => res.json())
    .then(gifts => putGiftsOnDom(gifts))
}


function putGiftsOnDom(giftArray){
    giftCollection.innerHTML = `<h2 class="subheader">All Gift Ideas</h2>
                                <h4 class="favorites-link">View My Favorites ♡</h4>`
    giftArray.forEach(gift => {
        giftCollection.innerHTML += `<div class="card">
          <h2>${gift.title} ($${gift.price})</h2>
          <h4 class="gift-cat">${gift.category}</h4>
          <a href=${gift.link} target="_blank"><img src=${gift.image} class="gift-image" /></a>
          <p>${gift.description}<p>
          <button data-gift-id=${gift.id} class="like-btn">♡</button>
        </div>`
    })
}
```

With the help of Flatiron's technical coaches, we identified the opportunity to refactor the putGiftsOnDom function and move towards more object oriented JavaScript. You'll see below that I took the following steps to move logic out of the putGiftsOnDom function and into a Gift class: create a Gift class, create a constructor for the Gift class assigning each attribute, render a Gift "card" for each Gift instance to display in the frontend. From there, I updated the putGiftsOnDom function to iterate through the giftArray and create a new instance of each Gift, and thus a new Gift "card"/div, to the giftCollection element.

```
class Gift {
    constructor(giftAttributes) {
        this.title = giftAttributes.title;
        this.price = giftAttributes.price;
        this.category = giftAttributes.category;
        this.description = giftAttributes.description;
        this.link = giftAttributes.link;
        this.image = giftAttributes.image;
        this.id = giftAttributes.id;
    }

    render() {
        return `<div class="card">
                  <h2>${this.title} ($${this.price})</h2>
                  <h4 class="gift-cat">${this.category}</h4>
                  <a href=${this.link} target="_blank"><img src=${this.image} class="gift-image" /></a>
                  <p>${this.description}<p>
                  <button data-gift-id=${this.id} class="like-btn">♡</button>
                </div>`
    }
}

function putGiftsOnDom(giftArray){
    giftCollection.innerHTML = `<h2 class="subheader">All Gift Ideas</h2>
                                <h4 class="favorites-link">View My Favorites ♡</h4>`
    giftArray.forEach(gift => {
        giftCollection.innerHTML += new Gift(gift).render()
    })
}



```
