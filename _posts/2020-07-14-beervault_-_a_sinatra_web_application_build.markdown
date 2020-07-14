---
layout: post
title:      "BeerVault - A Sinatra Web Application Build"
date:       2020-07-14 12:48:14 +0000
permalink:  beervault_-_a_sinatra_web_application_build
---


The goal for this project was to build a MVC Sinatra application utilizing ActiveRecord. Using the Sinatra framework, I'm able to separate my application code by function, providing separation of concerns; making writing, reading, and debugging a more straightforward process. A little more on MVC - 


## MVC

* The **M** of MVC, or Models, is the part of my application that handles my logic. Data is manipulated and/or saved here.

* **V**, or Views, is the part of the application seen and used by the user. My HTML and CSS code isi stored here and is responsible for the overall look and design of BeerVault.

* The **C**, or Controllers, is the code that makes my models and views play nicely together. The controller relays data from the browser to the application and from the application to the browser.

## The App

My last CLI build was beer-themed, so I wanted the opportunity to go a little more in-depth on that project. I use an app called UnTappd, which allows me to keep track of different beers I've had, rating each one and being able to reference what I've tried. Similarly, with BeerVault users can sign-up, sign-in/out, add new beers (with name, style, and rating), edit previously entered beers, or delete entries. All entries are stored in each users BeerVault.

## RESTful Routes
The functionality of my app comes courteousy of the RESTful route mapping between my HTTP verbs (get, post, put, delete, patch) to controller CRUD (create, read, update, delete) actions. Each time the application receives an HTTP request (when the user visits a diifferent part of the app), the corresponding controller action executes the code in that action and then sends back the appropriate response to the user.

Building this project was a great conclusion to mod 2 and was such a thorough recap of everything we've learned along the way. Thanks for reading!


