---
layout: post
title: "How To Setup Heroku App With Multiple Environtment For Each Branch On New Machine"
date: 2018-12-02 02:57:34 +0100
comments: true
categories: 
---
Recently, I want to migrate my side project into another domain. This project is currently hosted on Heroku platform. I really like the concept of push-to-deploy that Heroku good at. It frees me from server-management thingy. And, I feel so geeky whenever I use git from command line, hahaha. Ok, let me explain the state of the app.

<!-- more -->

<h2>Application Environment in Heroku Platform</h2>
The app has two different environment: `staging` and `production`. I think the names are self explanatory, otherwise you do not have enough experience in developing software. Each environment will point to different branch in developer local machine. The staging environment itself is pointing to development branch. At least in my local machine. My first local machine that I use originally for this project. The production environment is pointing to master branch in local repo. I know it sounds confusing. Because it really is. And that confusion slapped my face hardly when I try to rename my app in order to migrate the domain.

{% pullquote %}
So, each application in Heroku will has its own repository. In my case, staging app will have repo name with -staging in it. The production app simply the name itself, without prefix or suffix. At first, I naievely clone each app in different local repo, {" which make me even confuse and question my existence "}. So if I have two repos, I need to work in two different place? Hell no. That's the wrong way, Seto.
{% endpullquote %}

<h2>The Right Way (Really?)</h2>
The right way, it turns out, is to only clone the staging app. Why? Because it contains all the commit before merging them to the production branch: `master` local branch. I mean remote production branch. I mean, what??? Anyway, just continue the reading.

After you clone the `staging` repo, automatically you'll get heroku remote. Oh no, that's when you use heroku command `heroku git:clone -a [app name]`. Otherwise, I think you'll get `origin` as remote name. Check it.

``` bash
$ git remote -v
heroku	https://git.heroku.com/mycoolapp-staging.git (fetch)
heroku	https://git.heroku.com/mycoolapp-staging.git (push)
```

You want to make naming remotes and branches reflect to the environment and how you'll work. So, change the remote name into staging, the same name as the application environment it listed as the git repository.
``` bash
$ git remote rename heroku staging
```

And the next step is to add `production` app as `production` remote with the magic of git. Now I'm arguing my opinion to name the remote the sama with the app's environment. So `production` remote is `production` app and `staging` remote is `staging` app? *mikir*

``` bash
$ git remote add production https://git.heroku.com/mycoolapp.git
$ git remote -v
production	https://git.heroku.com/mycoolapp.git (fetch)
production	https://git.heroku.com/mycoolapp.git (push)
staging	https://git.heroku.com/mycoolapp-staging.git (fetch)
staging	https://git.heroku.com/mycoolapp-staging.git (push)
```

And then, you  need to fetch each remote like so:
``` bash
$ git fetch production
remote: Counting objects: 4, done.
remote: Compressing objects: 100% (4/4), done.
remote: Total 4 (delta 0), reused 3 (delta 0)
Unpacking objects: 100% (4/4), done.
From https://git.heroku.com/mycoolapp
 * [new branch]      master     -> production/master
```

Now is a bit tricky but should be understandable. Make sure local `master` branch track `production/master` and local `staging` branch tracks `staging/master`.

``` bash
$ git checkout -b staging staging/master
Branch 'staging' set up to track remote branch 'master' from 'staging'.
Switched to a new branch 'staging'
$ git checkout -b master production/master
Branch 'master' set up to track remote branch 'master' from 'production'.
Switched to a new branch 'master'
```

<h2>`bundle install` And The PostgreSQL Thingy</h2>
After you finish with the git stuff, you need to install the dependency. Use bundle.
``` bash
$ bundle install
```

And will most likely face this issue after running that command.
``` bash
Fetching pg 0.21.0
Installing pg 0.21.0 with native extensions
Gem::Ext::BuildError: ERROR: Failed to build gem native extension.

current directory:
/Users/setoelkahfi/.rbenv/versions/2.5.3/lib/ruby/gems/2.5.0/gems/pg-0.21.0/ext
/Users/setoelkahfi/.rbenv/versions/2.5.3/bin/ruby -r ./siteconf20181203-18631-gh6gvb.rb
extconf.rb
checking for pg_config... no
No pg_config... trying anyway. If building fails, please try again with
 --with-pg-config=/path/to/pg_config
checking for libpq-fe.h... no
Can't find the 'libpq-fe.h header
*** extconf.rb failed ***
Could not create Makefile due to some reason, probably lack of necessary
libraries and/or headers.  Check the mkmf.log file for more details.  You may
need configuration options.

Provided configuration options:
	--with-opt-dir
	--without-opt-dir
	--with-opt-include
	--without-opt-include=${opt-dir}/include
	--with-opt-lib
	--without-opt-lib=${opt-dir}/lib
	--with-make-prog
	--without-make-prog
	--srcdir=.
	--curdir
	--ruby=/Users/setoelkahfi/.rbenv/versions/2.5.3/bin/$(RUBY_BASE_NAME)
	--with-pg
	--without-pg
	--enable-windows-cross
	--disable-windows-cross
	--with-pg-config
	--without-pg-config
	--with-pg_config
	--without-pg_config
	--with-pg-dir
	--without-pg-dir
	--with-pg-include
	--without-pg-include=${pg-dir}/include
	--with-pg-lib
	--without-pg-lib=${pg-dir}/lib

To see why this extension failed to compile, please check the mkmf.log which can be found
here:

/Users/setoelkahfi/.rbenv/versions/2.5.3/lib/ruby/gems/2.5.0/extensions/x86_64-darwin-17/2.5.0-static/pg-0.21.0/mkmf.log

extconf failed, exit code 1
```
This is because the app is using PostgreSQL, and you don't have the database yet. You usually use Postgress.app for the installation. Or you can use other PostgreSQL distribution (is this the term?). Frankly speaking, it because it's the default database for Ruby stack on Heroku that we use PostgreSQL. But I like it. So doesn't really matter to me.

You need to install `postgresql` via Homebrew.

``` bash
$ brew install postgresql
```
You might need to change `password` and `username` for your local database, since you committed them on your other machine.

<h2>Run The App Locally</h2>
If you're lucky (and you're always), you will be able to run the app locally with `heroku`command:
``` bash
$ heroku local
```


Easy breezy lemon squeezy, eller hur? Hejdå!