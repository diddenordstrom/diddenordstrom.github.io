---
layout: post
title:  "Git Bare repository for Unix dotfiles"
date:   2022-05-27 10:00:36 +0200
categories: tutorial
---

# introduction

Need a place to store your config files aka dotfiles (or commonly spelled .dotfiles)?
A Git Bare Repo is great for this, you can initialize it in a folder and only add the files systemwide you actually want to track.
While you create a whole git repo at your home directory, things could get messy and ending up in you pushing sensitive information.


# setup

More detailed guide [here](https://martijnvos.dev/using-a-bare-git-repository-to-store-linux-dotfiles/)

First, create a git repo at GitHub named .dotfiles
Then, just paste the commands in your terminal.

```bash
# Create the directory for your bare repository
mkdir ~/.dotfiles

# Initialize a bare repository in the directory you just created
git init --bare ~/.dotfiles

# Create a Git alias that references the Git dotfiles (or config, which I use) repository and the local root directory from which Git adds files by default
alias dotfiles='/usr/bin/git --git-dir=$HOME/.dotfiles --work-tree=$HOME'

# Configure the dotfiles bare repository to hide untracked files
dotfiles config status.showUntrackedFiles no

# Add the remote location to the repository (in this case GitHub) 
dotfiles remote add origin https://github.com/$USERNAME/$REPOSITORY.git
```


# what have we done?

1. Created a folder to host our git bare repo files, and initialized the repo

2. Added an alias for our Git Bare repo, so we add files, check status etc by replacing `git` with `dotfiles` or wathever we set the alias to.
Example: `dotfiles add .bashrc`

3. Removed the feature to show untracked files while running `dotfiles status`, as it would print out the whole home directory. 
