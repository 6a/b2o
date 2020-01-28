---
layout: post
title: "Launcher Lobby Animations"
date: 2020-01-15 21:25:23 +0000
categories: update
tags: update info
---

For the lobby of th Blade II Online launcher, I decided to implement a selector component based on the one found in various Legend of Heroes games - A rotating circle with options along the circumference.

In the Legend of Heroes games, this selector component is present during battles and is used to select an action for the current character. The player uses the up and down buttons on the gamepad to choose from one of the options, and then presses the confirm key(X, A, Circle etc) as can be seen in the video below.

<video width="800" height="auto" controls="controls" autoplay loop>
  <source src="{{ site.url }}/media/vid/tits-fc-battle.mp4" type="video/mp4">
</video>
*The Legend of Heroes - Trails in the Sky FC*

For the Blade II Online Launcher, once the user has logged in they are taken to the lobby. From here, by clicking the up or down buttons a user can switch between multiple options, changing the functionality of the button on the right hand side and eventually clicking it to trigger an event - as seen below.

<video width="800" height="auto" controls="controls" autoplay loop>
  <source src="{{ site.url }}/media/vid/blade2-launcher-lobby.mp4" type="video/mp4">
</video>
*Blade II Online Launcher - Lobby Animations*