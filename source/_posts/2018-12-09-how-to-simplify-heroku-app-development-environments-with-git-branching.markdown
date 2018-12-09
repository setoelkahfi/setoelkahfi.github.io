---
layout: post
title: "How To Simplify Heroku App Development Environments With Git Branching"
date: 2018-12-09 19:07:03 +0100
comments: true
categories: 
---
The Heroku system for managing different app's environments is difficult to understand for me at first. The concept of having different branches tracking different remote repository is new. But, overtime, as I use the service, I'm kind of familiar with it. 

<!-- more -->

The confusion for me is because an asymmetry behaviour of Git branching system itself. As written in [this post](https://longair.net/blog/2011/02/27/an-asymmetry-between-git-pull-and-git-push/), a simple `git pull` and `git push` will behave differently regarding remote tracking branch. You can just read it in the blog post.


My point is, dear future `self`, it will helpful for you if you set the `push.default` config to `upstream` (previously `tracking`). With this command, everytime you do `git push`, it will automatically push the `upstream` branch. For example, if you're on the `staging` branch, `git push` will update repository in `staging` remote `master` branch. Simple, right?

``` bash
$ git config push.default tracking
$ git push 
```
You should always follow [the official documentation](https://devcenter.heroku.com/articles/multiple-environments) for this specific Heroku setup. Hej då!


Reference:

https://devcenter.heroku.com/articles/multiple-environments
https://longair.net/blog/2011/02/27/an-asymmetry-between-git-pull-and-git-push/