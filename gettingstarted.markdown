---
layout: page
title: Getting Started
permalink: /gettingstarted/
nav_order: 2
---

# Getting Started with a GitHub Documentation Page!

## Installing the necessary software on Mac OS

* Install Homebrew
```
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```
* Install Git:
```
brew install git
```
*  Install [rbenv](https://github.com/rbenv/rbenv#installation)
```
brew install rbenv
```
* Set up rbenv for your shell:
```
rbenv init
```

This might put out a message like this:

```
# Load rbenv automatically by appending
# the following to ~/.zshrc:

eval "$(rbenv init - zsh)"

```

Make sure to so what it says. In this case, you should run the command it says: `eval "$(rbenv init - zsh)"`

* Setup your terminal to start rbenv on open:
```
echo 'eval "$(rbenv init -)"' >> ~/.bash_profile
source ~/.bash_profile
```
* Check your installation (you should see a bunch of "OK"s)
```
curl -fsSL https://github.com/rbenv/rbenv-installer/raw/main/bin/rbenv-doctor | bash
```
* Install Ruby 2.7.2 using rbenv, which is not the latest version, but is the version required for GitHub Pages.
```
rbenv install 2.7.2
```

* Set Ruby 2.7.2 as your global version
Note: This will set version 2.7.2 as your default Ruby version. If you're working on other Ruby projects with the computer you're using for this workshop, feel free to contact us for help maintaining multiple versions.
```
rbenv global 2.7.2
```
* Restart your terminal

* Download / update Ruby gems:
```
gem update --system
```

* Install the jekyll and bundler gems.
```
gem install jekyll bundler
```

* Navigate your terminal window to the Repository Directory and run the following command

```
bundle install --user-install
```

* Now you can start the local server on your machine with the following command

```
bundle exec jekyll serve
```

You'll probably see something like this:

```
Configuration file: /Users/preservation/Documents/Github/bavc-resources/_config.yml
To use retry middleware with Faraday v2.0+, install `faraday-retry` gem
            Source: /Users/preservation/Documents/Github/bavc-resources
       Destination: /Users/preservation/Documents/Github/bavc-resources/_site
 Incremental build: disabled. Enable with --incremental
      Generating...
      Remote Theme: Using theme pmarsceill/just-the-docs
       Jekyll Feed: Generating feed for posts
                    done in 4.72 seconds.
 Auto-regeneration: enabled for '/Users/preservation/Documents/Github/bavc-resources'
    Server address: http://127.0.0.1:4000
  Server running... press ctrl-c to stop.
```

This means that you have a local instance of the page running at http://127.0.0.1:4000

The purpose of running the local server on your machine is so that you can see the changes you're making to site. These changes will be made to the live site at http://bavc.github.io/bavc-resources when you push them to the repo, but the local instance is as great way to test them beforehand.
