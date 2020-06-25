---
template: post
title: "Introducing atRest: Part 1"
slug: fifty-seventh-post
draft: false
date: 2018-07-29T17:55:00.000Z
description: This is a post about my final project built using Mapbox, React,
  and a Rails 5.2 API.
category: Flatiron School
tags:
  - Flatiron School
  - Immersive
  - Coding School
---

I am just beginning my final project this week. I will be building a donor-powered Airbnb called atRest. I'm really excited to be building this, as I think it will help some friends find places to stay. Throughout the development of the venture, I will be writing about how it is going and various challenges that I approach and how I tackle them. 
# Main Goal
My main goal is to make something barely functional to release August 3rd to be used only in the case of emergencies and with an eye towards future iterations. 
# Backend: Rails 5.2 API 

Right now, I have three models: Users, Locations, and Stays. Users have many locations. Locations belong to Users. Locations have many stays. Stays belong to Locations. This is my basic backend. I built a Rails 5.2 API to deal with the seed data that I'm using, which are the cheapest hostels and motels in DC, so that it can actually be used. 

I built it as a Rails 5.2 JSON API. 
#Frontend: React & Mapbox

The frontend is being built in React Components, which fetch the data from the backend. I'm also using Mapbox-Gl-JS React bindings for the map. 

I have three screens so far. One will be the search page, the second page will include an index page for listings with a corresponding map parallel to each other, and the third page will be a show page for the individual listings. 
# User stories for the skateboard

A user will be able to search for listings by name. 

A user will be able to reserve a stay at a location. 

#Stretch Goals 

User may add filters for price and distance from a location. 

User can login and signup. 

User can donate to support a stay with Stripe. 

Users will be able to review stays. 

Polymorphic associations with hosts, guests, donors. 

User Profiles

2FA through Twilio with User Profile Creation

As a guest, one can CRUD a stay. 

As a host, one can CRUD a listing. 

As a donor, one can CRUD a donation. 

Integration with uPort for identity management for users. 

Text-based booking interface with Twilio for accessibility. 
# Blockers Now

## Map markers
I'm having trouble building the map out with markers and getting the data to load. I'm debugging it with reading the docs and help from a fellow student. Mapbox does not recommend using the React components, but it is a crucial part of the project, so I will be doing it in React components. I'm still searching for the right solution. 

## Existential 
I've gotten a bit overwhelmed with the scope of the project because of the impact it could have and am a bit scared that it will have a negative impact on society in unforeseen ways. To approach this blocker, I'm biting off small chunks of the project and moving forward as conscientiously as possible. 

# In closing

I'm looking forward to revealing the final product, which will be a living product, August 3rd and updating you on the progress that I've made since the start of this week onward. 

I couldn't be more excited about the future and what it holds. Thank you for reading this post and thank you for being supportive of my coding journey. It means the world to me! 







