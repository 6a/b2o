---
layout: post
title: "Switching to Electron"
date: 2019-12-31 04:11:22 +0000
categories: update
tags: update technology electron ue4
---

Earlier this month I decided to split the game into two main bundles - the launcher and the game executable. I decided to use [Electron][1] as I have used it in the past.

I decided to make the change as developing UI with Unreal Engine was very slow. It's hard to find decent documentation, especially that which is not out of date. I wasted many hours wading through source-code and old blog/forum posts trying to implement very basic features such as adding hyperlinks to in-game UI text, and was having strange issues with language switching using Unreal Engines built-in localization tools.

I especially like how Electron is very quick to develop, test and prototype, with no compile times and immediate updates while debugging. Also, using Visual Studio Code it's possible to launch the app along with a chromium developer console, which makes troubleshooting and tweaking styles very quick and relatively painless.

Being able to harness CSS animations is also a boon, as it allowed me to effortlessly make use of transitions & animations to animate loading spinners, move UI components around, and transition between layouts.

Switching environments also allowed me to redesign the UI in a more flat design style. I took a lot of inspiration from the current (at the time of writing) League of Legends launcher, which may be clear to see for those who are familiar with the game.

![Unreal Engine 4 Login Page][2]
*Unreal Engine 4 Login Page*

![Electron Login Page][3]
*Electron Login Page*

But the biggest boon for me since switching to Electron is the ease of finding solutions for problems that arise during development. JavaScript and HTML hold the first and second spot at the top of the most popular technologies list in the most [recent stackoverflow survey (2019)][4], and as a result almost any answer can be found, if you can think of the right search terms to get there.

[1]:https://electronjs.org/
[2]:{{ site.url }}/media/img/login-page-ue4.jpg
[3]:{{ site.url }}/media/img/login-page-electron.jpg
[4]:https://insights.stackoverflow.com/survey/2019#technology
