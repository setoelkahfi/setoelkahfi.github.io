---
layout: post
title: "Seto, This Is How You Setup This Blog In Another Machine"
date: 2018-11-23 12:14:36 +0100
comments: true
categories: [Technical]
---
This is new chapter in my life. I've been working in Sweden since 19 November. Moving to Sweden itself is a new experience for me. So, everything surround that are also new, except some general things, such as work, Indonesian friends, iOS development, and MacBook.

I've been working since I graduated from technical high school when I was 19. There are some Indonesian friends here in Sweden, mostly students. I've been working as iOS developer since 2016, so it's not really new experience either. And since that time, I've been using Macbook for sure.

But, I always face difficulties when I setup this blog in new machine. So, dear future self, this is how you do it.

<!-- more -->

<h2>Prerequisites</h2>
You need to have Ruby installed in you new shiny machine, Seto. You always use the latest stable branch. So, go ahead. 
``` bash
$ ruby --version
```
{% pullquote %}
And for a matter of fact, you always use rbenv when your new machine is MacBook. You never use built in Ruby on Mac. To install it, you use Homebrew. So make sure you install it before you install rbenv. You always joke that you need to install version manager with package manager to eventually install dependency manager. {"The good thing about meeting new people is you can recycle your jokes."}
{% endpullquote %}


``` bash
$ brew install rbenv
$ rbenv init
# Load rbenv automatically by appending
# the following to ~/.bash_profile:

eval "$(rbenv init -)"
```
In this step, you usually face difficulty. Basically you just need to put that line in your .bash_profile. Or just execute this line:
``` bash
echo 'eval "$(rbenv init -)"' >> ~/.bash_profile
```
If you open the file, it should looks like this:
``` bash
$ vim ~/.bash_profile

export LC_ALL=en_US.UTF-8
export LANG=en_US.UTF-8

export NVM_DIR="$HOME/.nvm"
[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh"  # This loads nvm
[ -s "$NVM_DIR/bash_completion" ] && \. "$NVM_DIR/bash_completion"  # This loads nvm bash_completion

eval "$(rbenv init -)"
```
There's a time when you accidentally add the result of that command instead:
``` bash
export RBENV_SHELL=bash
source '/usr/local/Cellar/rbenv/1.1.1/libexec/../completions/rbenv.bash'
command rbenv rehash 2>/dev/null
rbenv() {
  local command
  command="$1"
  if [ "$#" -gt 0 ]; then
    shift
  fi

  case "$command" in
  rehash|shell)
    eval "$(rbenv "sh-$command" "$@")";;
  *)
    command rbenv "$command" "$@";;
  esac
}
```
If you find that code in your .bash_profile, remove. 

Now you need to open new terminal, or you can just open new tab. Command + T is the shortcut. If you don't, your previous setup will not loaded. OK, now install latest Ruby version. Check the available ruby versions, then pick the latest stable release.
``` bash
$ rbenv install --list
$ rbenv install 2.5.3
```
Set the latest Ruby version that you've just download as global version. Or local. But always set it as global.
``` bash
$ rbenv global 2.5.3 # or
$ rbenv local 2.5.3
```
Done with the Ruby thing.

<h2>Clone</h2>
Make sure you only clone the source branch of the repository. 
``` bash
$ git clone -b source https://github.com/setoelkahfi/setoelkahfi.github.io
$ cd setoelkahfi.github.io
```
The mistake you always do is to clone the default branch from the repository, which is master. Don't do that. After you move to the cloned directory, then clone the master branch to _deploy folder.
```
$ git clone https://github.com/setoelkahfi/setoelkahfi.github.io _deploy
```
After that, you do setup for the project itself. Which include installing bundler and setup github pages. 

```
$ gem install bundler
$ rbenv rehash
$ bundle install
$ rake setup_github_pages
```
If you fail with the rake things, you usually prepend the command with bundle exec.
```
$ bundle exec rake setup_github_pages
```
Then you need to enter the name of the repository.
```
Enter the read/write url for your repository
(For example, 'git@github.com:your_username/your_username.github.com)
```
It should done by mow.

<h2>Start Blogging</h2>

Then you need to test the actual blogging command. Generate post, preview, deploy. You can always refer to the official documentation of how to blog with Octopress. But here's some commands to get started. 

```
$ bundle exec rake preview # to preview the blog in development server
$ bundle exec rake new_post["title"] # to make new post
```
If you want to workaround with this, you should uninstall rake that doesn't match with the rake in the Gemfile.

```
$ gem uninstall rake

Select gem to uninstall:
 1. rake-10.4.2
 2. rake-10.5.0
 3. rake-12.3.0
 4. All versions
> 3
Successfully uninstalled rake-12.3.0
```
After that, you can update the rake with this command:
```
$ bundle update rake
```
though you unsuccessfully update the rake in Gemfile. But now, you should be able to use rake command without prepend it with `bundle exec`.

Always preview the blog before publishing it. Use the preview command and check in your browser on port 4000.
```
$ rake preview
```
This command will watch for changes in your blog, and regenerate it. You just need to refresh your browser.

<h2>Deploy The Blog, Seto!</h2>
You will likely face difficulty when trying to deploy the blog. So follow this guide below.

This happened when you run `rake setup_github_pages`: your deploy will be rejected because the tip of your current branch is behind. 

```
## Pushing generated _deploy website
To https://github.com/setoelkahfi/setoelkahfi.github.io
 ! [rejected]        master -> master (non-fast-forward)
error: failed to push some refs to 'https://github.com/setoelkahfi/setoelkahfi.github.io'
hint: Updates were rejected because the tip of your current branch is behind
hint: its remote counterpart. Integrate the remote changes (e.g.
hint: 'git pull ...') before pushing again.
hint: See the 'Note about fast-forwards' in 'git push --help' for details.

## Github Pages deploy complete
```
The explanation for this problem is that, `rake setup_github_pages` will remove `_deploy` folder and initiate new repository inside of it. So, it it happened, just remove _deploy folder, and do clone the master branch in it, again. Like so:
```
$ rm -rf _deploy/
$ git clone https://github.com/setoelkahfi/setoelkahfi.github.io _deploy

# You might want to make sure that your local master branch is align with remote master branch by doing this
$ cd _deploy/
$ git log
commit 9663633e98e3b460191150a4cc656e1400acf974 (HEAD -> master, origin/master, origin/HEAD)
Author: Seto Elkahfi <seto@isolve.se>
Date:   Fri Nov 23 19:35:52 2018 +0100

    Site updated at 2018-11-23 18:35:52 UTC

commit f1959c62b14065998a4ea7d438576ca57d4e5405
Author: Seto Elkahfi <seto@isolve.se>
Date:   Fri Nov 23 19:31:38 2018 +0100

    Site updated at 2018-11-23 18:31:38 UTC

# more commit logs
```
After that, `rake deploy` should be executed successfully.
```
$ rake deploy
## Deploying branch to Github Pages 
## Pulling any updates from Github Pages 
cd _deploy
Already up to date.
cd -
rm -rf _deploy/index.html
rm -rf _deploy/images
rm -rf _deploy/about
rm -rf _deploy/blog
rm -rf _deploy/favicon.png
rm -rf _deploy/atom.xml
rm -rf _deploy/javascripts
rm -rf _deploy/sitemap.xml
rm -rf _deploy/robots.txt
rm -rf _deploy/assets
rm -rf _deploy/stylesheets

## Copying public to _deploy
cp -r public/. _deploy
cd _deploy

## Committing: Site updated at 2018-11-27 21:01:09 UTC
[master ec32357] Site updated at 2018-11-27 21:01:09 UTC
 6 files changed, 121 insertions(+), 5 deletions(-)

## Pushing generated _deploy website
Counting objects: 18, done.
Delta compression using up to 4 threads.
Compressing objects: 100% (14/14), done.
Writing objects: 100% (18/18), 2.93 KiB | 1.46 MiB/s, done.
Total 18 (delta 8), reused 0 (delta 0)
remote: Resolving deltas: 100% (8/8), completed with 8 local objects.
To https://github.com/setoelkahfi/setoelkahfi.github.io
   9663633..ec32357  master -> master

## Github Pages deploy complete
cd -
```

```
rake generate
rake deploy
```
This will generate your blog, copy the generated files into `_deploy/`, add them to git, commit and push them up to the master branch. In a few seconds you should get an email from Github telling you that your commit has been received and will be published on your site.

Don't forget to commit the source for your blog.
```
git add .
git commit -m 'your message'
git push origin source
```

<h2>Summary</h2>
<li>You're not working with `master` branch. It is managed by `rake` command.</li>
<li>If you plan to blog in two machines, always make sure you push all the changes to your `source` and `master` branch.</li>

That's it for today, Seto. Just remember, everytime you move to a new place, be a nice person as you want your new friends would be. Good luck for the new adventure. Hejdå!