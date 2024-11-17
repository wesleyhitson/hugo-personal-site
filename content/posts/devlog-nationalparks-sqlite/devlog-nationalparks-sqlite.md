---
title: "DevLog: National Parks SQLite"
date: 2024-11-16T12:49:30-04:00
draft: true
tags: ["api", "database", "sqlite", "csv", "data", "devlog"]
---
One thing that I love to do is visit National Parks. Visiting them all is on my bucketlist. Some people really like to collect the cancellation stamps; I am more concerned with witnessing and experienceing all these amazing natural areas. At the time of writing this, there are 63 parks, and I have been to about 18 of them. I say "about" because I am not 100% sure that I went to certain ones since I went when I was a kid (Grand Teton, Haleakala). So I will be going back to make sure that I hit those. But I digress. There are a couple different ways I could track which parks I have gone to. But, perhaps inevitably due to my occupation as a software developer, I have considered storing that information in a database. I thought about putting that info in a SQLite database so I could easily drop it in to various projects if I'm trying a new technology and need some quick data. Sure, I could use a mock data generator like [Mockaroo](https://www.mockaroo.com/), but it's fun to have real data sometimes, and also to be reminded of my goal.

I first started looking online for a CSV of the data. I found a few Github repositories, but they didn't quite have all the data that I was looking for most of the time. This repository from eric-yates was the closest I found, but it's also 6 years old and probably not 100% up to date. New River Gorge was added as a National Park in 2020, so I wanted to find up to date data if I could. It took a little bit of digging, but the National Park Service actually provides an API to get this data if you know where to look. Start [here](https://www.nps.gov/subjects/science/science-data.htm), and click the link for "NPS APIs", which takes you [here](https://www.nps.gov/subjects/developer/api-documentation.htm) (holy crap a Swagger page?? Let's go America!).

If you try to make a request using the Swagger page, it asks you to register, and it's entirely free. But it makes sense that they'd want to issue API keys. I put in my email and shortly received my key in my inbox. I went back to the Swagger page and put my key in the green "Authorize" box near the top.

![NPS API Authorize](/posts/devlog-nationalparks-sqlite/nps-api-auth.png "NPS API Authorize")

Once we're authorized, we can start getting data.

Python comes with SQLite as part of the standard library. Just `import sqlite3`. I am using [these](https://docs.python.org/3/library/sqlite3.html) docs to guide me through this. 