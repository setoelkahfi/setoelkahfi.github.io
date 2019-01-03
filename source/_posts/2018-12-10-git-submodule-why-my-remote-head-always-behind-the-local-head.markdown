---
layout: post
title: "Git Submodule Why My Remote HEAD Always Behind The Local HEAD"
date: 2018-12-10 18:37:14 +0100
comments: true
categories: 
---

The git submodule is something I've never use it extensively during my career as software developer. But fortunately, now in my current work in Sweden we use it. I'm lucky enough to have the opportunity to actually learn to use it in real world. And this is the first simple problem that I encountered during my work with git submodule. First, let me tell you the background of the project.

<h2>iOS Project With Git Submodule</h2>
This is an iOS project that share resources (icon, string translations, configurations, etc.) with Android project. I will not talk about the Android project. So, basically we have translation file in .xlsx file. It's stored in Google docs, so everyone in the team can access, add, and modifies it. Everytime we add new new translations, we need to run a 'python' script against it, and it will generate the resource file for both iOS and Android project. It works smoothly, thanks to our previous
developer :beer.

<h2>The Missing Part</h2>
As usual, after someone added new translations and generate the resource strings, she should commit it in the resource repository, which is inside the iOS project. What I missed about how git submodule works is you should also commit the change (which is the new commit inside the submodule) in the parent repository. Off course, after the other developer pushed the changes to the remote repository.

<h2>The Circle CI Pull The Wrong Commit</h2>
Previously I asked my colleague why my submodule HEAD is always behind the remote in parent repository. That's actually the effect of not committing the change in submodule repository in the parent repository. This problem will affect how our CI build pull the string translations generated in the submodule: it always pull whatever it is in the HEAD.

Glad I understand it. I know it might a simple concept to understand. But I hope someone will benefit from my post here. Once, everybody is a beginner.

https://stackoverflow.com/a/36375256/1137814
https://ttboj.wordpress.com/2014/05/06/keeping-git-submodules-in-sync-with-your-branches/
