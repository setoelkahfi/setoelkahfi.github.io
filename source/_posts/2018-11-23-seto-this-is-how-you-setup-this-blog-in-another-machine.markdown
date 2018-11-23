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

You will likely face difficulty when trying to deploy the blog. So follow this guide below.
```
rake generate
rake deploy
```
This will generate your blog, copy the generated files into _deploy/, add them to git, commit and push them up to the master branch. In a few seconds you should get an email from Github telling you that your commit has been received and will be published on your site.

Don't forget to commit the source for your blog.
```
git add .
git commit -m 'your message'
git push origin source
```

That's it for today, Seto. Just remember, everytime you move to a new place, be a nice person as you want your new friends would be. Good luck for the new adventure. Hejdå!