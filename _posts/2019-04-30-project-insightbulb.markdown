---
title: ":low_brightness: Insightbulb"
layout: post
date: 2019-04-15 22:10
tag: 
- Flask
- IoT
- Communication
- Team development
- javascript
- python
image: /assets/images/insightbulb.png
headerImage: true
projects: true
hidden: true # don't count this post in blog pagination
description: "An IoT web application to control ambient light notifications!"
category: project
author: whoodes
externalLink: false
---
A second semester software engineering course project.  Our team built a Flask application to scrape tidal
and wave data to be used for ambient notifications via Yeelight IoT light bulbs.

The user can select a station based off a desired region.  When this occurs, the application spins up two 
threads.  One incrementally increases or decreases the brightness based off the percentage to the next
high or low tide.  The other thread is delayed based off of the delta from the current time to the next
tide extrema. 

The app also displays the data visually using [Chart.js](https://www.chartjs.org/).  A very cool, and customizable
graphing framework.  Currently, tide and lunar-phase data can be toggled between.  A Hawaiian Island page is on the road
map, adding ambient alerts for people on the islands.

Check us out on [GitHub](https://github.com/insightbulb/insightbulb)!


