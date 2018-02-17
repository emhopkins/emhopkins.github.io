---
layout: post
title:      "React Project - Displaying Quotes from "The Office""
date:       2018-02-17 23:51:07 +0000
permalink:  react_project_-_displaying_quotes_from_the_office
---

I am so excited to have made it to my final project!  I was actually pretty excited to start this app and get to go through the process of building a Rails API backend with a React front end.  I started by working through [this](https://www.fullstackreact.com/articles/how-to-get-create-react-app-to-work-with-your-rails-api/) tutorial.  This author went through the process of creating your Rails API and then the React app all in one project repository.  I initially thought I would need two repos for this project (one for the API and one for the front end), so walking through this tutorial was really helpful.  The author also recommended using the [foreman](https://github.com/ddollar/foreman) gem in order to start both the Rails server and the React app with one command.  This was a huge help as I was getting started.  

Before creating my react app, I built my Rails API.  I created two models inspired by my first portfolio project.  For my Ruby cli project, I created an app that displayed quotes from "The Office" after scraping them from a website.  I used that data to create the seeds for my Rails API.  I first created a few Character objects (characters have a name attribute and have_many quotes).  Then I created lots of Quote objects (quotes have a text attribute and belong_to a character).  Once my Rails models were good to go, I started by creating a get route to my /api/quotes path and created a Quotes Controller.  I started my controller with just an index action.  

Next, I went back to the tutorial and began wiring up my react app using create-react-app.  I created a React app named client in the root of my Rails API.  Then I continued to set up my react app until I was ready to test my first get request to my API.  After a few console.log 's and some tweaking I was able to retrieve the data from my API and display it in my react app!  From this point, I added active model serializers to my Rails API and then worked on displaying the specific data exactly how I wanted in my React app.  

The next challenge was to implement the different react-redux elements and to post data to my API.  I went back through our movies labs to refresh on react-router and then implemented my routes in a similar way to those labs.  I also set up Redux middleware to respond to and modify state changes.  

Next up, I worked on adding a form in which the user can add a new character and create that character in my Rails API database.  I read up on how to do a javascript post using fetch [here](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API/Using_Fetch) and began by modeling the post request off of my original get request.  My first "success" was when I got a route not found error from my API.  I intentionally left our the post route for characters, because I wanted to make sure my post was working as expected before I wrote that code.  Next I continued one step at a time until I was successfully creating my new object, returning it with a JSON response and updating the state in my React app.  


