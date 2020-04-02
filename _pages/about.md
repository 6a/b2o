---
layout: page
title: About
fulltitle: About Blade II Online
permalink: /about/
---

Blade II Online is a game that I developed as my "Individual Project" (roughly equivalent to a disseration) during my final year at [Kingston University London](https://www.kingston.ac.uk/).

Blade (also known as Blade II or Gate of Avalon, depending on the game) is a one versus one card game that appears as a minigame in various [Nihon Falcom](https://en.wikipedia.org/wiki/Nihon_Falcom) titles such as [The Legend of Heroes - Trails of Cold Steel II](http://www.trailsofcoldsteel.com/cs2/) and [Tokyo Xanadu](https://en.wikipedia.org/wiki/Tokyo_Xanadu).

I really enjoyed playing Blade, in its various forms, during my playthroughs of these games. So much so that I decided to recreate it for my final project.

In order to broaden my horizons, and in search of a challenge, I set out to expand upon the original concept and produce a version of the game that can be played online, versus opponents from anywhere in the world. Using my existing knowledge, as well as the valuable experience gained during my internship as a foundation, I designed and developed both the entire back-end system that supports online gameplay, user accounts, authentication, profiles, hiscores, and logins, as well as the game launcher and the game itself.

I developed the launcher for this game using [Electron](https://electronjs.org/), writing code in Javascript and creating layouts and interfaces with HTML and CSS. I wanted to aim for a relatively fully featured launcher experience, found in other online games such as [League of Legends](https://leagueoflegends.com) and [Dota2](dota2.com), and after reading a great article describing [the architecture of the new League of Legends client](https://technology.riotgames.com/news/architecture-league-client-update), I decided that Electron would be the perfect tool given the time and resources I had avaiable. Rapid prototyping, no compile times, and a huge range of powerful open-source libraries made development quick and relatively painless.

I developed the actual game itself using [Unreal Engine 4](https://www.unrealengine.com/en-US/what-is-unreal-engine-4). Where possible, I made use of C++ to implement both engine and gameplay logic. While I personally find it easier to develop with [Unity](https://unity.com/), I believed (and still believe) that I need more experience developing games using C++. 

All the graphical assets that I made myself were made using [Maya](https://www.autodesk.com/products/maya/overview) or [GIMP](https://www.gimp.org/), while the rest were taken from various free or open source offerings (see Licenses tab for details)

The back-end infrastructure is supported by various technologies, using various [AWS](https://aws.amazon.com/) services:
- Gameserver written in Go, reverse proxied using [NGINX](https://www.nginx.com/) and hosted on an [EC2](https://aws.amazon.com/ec2/?nc2=h_ql_prod_fs_ec2) virtual machine.
- Persistent data storage etc using [RDS for MySQL](https://aws.amazon.com/rds/mysql/).
- REST API using [Lambda](https://aws.amazon.com/lambda/) (with logic written in Go) and [API Gateway](https://aws.amazon.com/api-gateway/) that offers auth, leaderboards, account manipulation/lookup etc.

---

### Thanks for checking out my game!

- James Einosuke Stanton