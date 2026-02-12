---
layout: post
title: "Github Tutorial"
date:   2022-04-06 10:00:36 +0200
categories: tutorial
---


# Introduction

The aim of this tutorial is to give the reader a quick insight in the meaning of git and how to utilize it.
The guide will reference Github Desktop (program) and I highly recommend installing it prior to this.
To sign in to GitHub Desktop, make sure to generate a personal access token (PAT) as password authentication was depricated last year.

A PAT is generated via Github -> your profile -> settings -> developer settings -> personal access tokens -> generate new token.
Set expiration date to _No Expiration_ and check the little square for _repo_.
That should be enough.


# Github

On Github is where your code lives. 
A repository is either a _public_ or _private_. 
Public means everyone can read, download and request changes to your code. 
Private well, is private.
You can still invite others to participate on private repos however.
Just go into the repository -> settings -> collaborators -> add people.


# Github Desktop

Github Desktop is the official client for git.
There are a ton of others clients like Gitkraken and Fork. 
They are essentially just a GUI for your git program that you can access via terminal, so use what you feel comfortable with.

After installing and signing in to Github Desktop you can start to download or create repos. 
To download a already existing repo that you have, go to File -> Clone repository and paste the repos Github link.
You should see that the current branch is main (sometimes master, but this is depricated in favor for main). When working with several people on a single project, a rule of thumb is to never commit code directly to main as it often causes conflicts. 
When first setting up the repo this is fine.


# Branches

When serveral people are working on a project and code in the same files there will be conflicts. 
To make this easier to deal with you should make a new branch for every feature you want to implement.

For example: I collaborate on a website and shall add the functionality to login. I create a branch (via Github Desktop or Github) and checkout that branch.
Then I start to work on the feature, adding commits when subgoals are met.
A programmer should commit code every hour or so depending on the progress they've made. 
Commits can be super small, like correcting a single line of code, just don't overdo it.

When the feature is done, make sure the branch is up to date with main.
This is done in Github Desktop by clicking the dropdown _Current Branch_ then _Choose a branch to merge into NAME_.
Select main, if it says it is up to date you are good to go.
Else you need to merge main into your branch and deal with the conflicts. 
Conflicts occur when changes where made to the same file in different commits. 
We need to manually select what code we want to keep and what code is old. 
If you are unsure, check with your teammate who wrote the code.

When all is well and done you can create a pull request on Github. 
It is generally a good idea to let others look through the changes and test the code before merging and deleting the branch. 
Main is supposed to be a functional version :)


# TLDR

For every feature, create a new branch. 
Commit often. 
When done, first update your local branch by merging main into it. 
Then create a pull request so others can look at your code. Merge and delete branch when ready.
