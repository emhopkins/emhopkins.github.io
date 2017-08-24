---
layout: post
title:  "Sinatra Portfolio Project - CRUD Application "
date:   2017-08-24 04:33:15 +0000
---


For my Sinatra portfolio project, I created a travel site.  A user can sign up for my site and then create new travel destinations as well as view destinations created by other users.  They can also edit their own destinations.  Every travel destination also has many activities.  For my project structure, I ended up with this:

![](http://imgur.com/a/cgyOj)

I created three models for my application: users, destinations and activities.  The relationships between the models were:
* A user has many destinations and many activities. 
* A destination has many activities and belongs to a user. 
* An activity belongs to a destination and belongs to a user. 

In practice, I did slightly change the relationship between activities and destinations.  Technically, an activity belongs to the first destination that creates it.  However, in order to be able to chose from an unduplicated list of all activities, I decided to associate single activities with multiple destinations.  In my implementation, an activity can belong to many destinations.  I also decided to not allow the user to edit any activities after they have been created.  Rather, they can just edit the activities associated with a destination.

My application begins at http://localhost:9393/.  Here you are welcomed to my site and prompted to log in or sign up.  If the user is already logged in, they are redirected to the home page.  Once a user has successfully logged in or signed up, they are brought to the homepage.  From the home page, they can view destinations that were created by other users as well as create their own.  Users can only edit their own destinations.  

I also added some input validations for signup and creating/editing destinations.  For signup, the user cannot enter a blank username, email or password.  And when creating or editing a destination, the name cannot be blank.  

I also added additional security to editing destinations by redirecting any gets to my edit view if the destination user is not the logged in user.  

My project source code is located [here](https://github.com/emhopkins/sinatra-portfolio-project-travel-site).



