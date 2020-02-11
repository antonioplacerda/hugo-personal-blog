---
title: "Full stack react redis app"
date: 2020-02-11T06:45:53+01:00
draft: false
css: []
tags: ["react", "redis", "heroku", ]
---

I wanted to get an app live and [here](https://salty-badlands-66972.herokuapp.com/) it is!
I followed a tutorial from [code drip](https://www.youtube.com/channel/UCRLEADhMcb8WUdnQ5_Alk7g) to help me start this.
Since this was supposed to be a simple test of my skills, I didn't want to pay for a hosting solution. I checked digital ocean, amazon web services and google cloud platform. I also tried firebase, but it also seemed an overkill.
So, I went with heroku!
Initially I had an express app running for the api where it would only fetch the data from the redis database, a worker with a scheduled cron job and the react server for the frontend.
In heroku I couldn't find a way to expose both ports, so I went with a single express app for the frontend and for the api.
I had to tweak the package.json files a lot to get to where the app is right now, but I like the final result!
The source code can be found [here](https://github.com/antonioplacerda/full_stack_node_react_redis)
