---
layout: post
title: "Setup an existing Octopress repository after git clone"
date: 2013-07-24 07:25
comments: true
categories: Octopress, github
---

## The Problem

It seems all the tutorial on Octopress deals with setting up a new repository.
Recently, I needed to clone the my Octopress repo on a new machine and
resume writing there. I thought it would be as simple as
`git clone`, `rake setup_getihub_pages`, `rake new_post`, and `rake gen_deploy`.
However I encourtered the following during deploy

```
To https://github.com/username/username.github.io
 ! [rejected]        master -> master (non-fast-forward)
 error: failed to push some refs to 'https://github.com/username/username.github.io'
 hint: Updates were rejected because the tip of your current branch is behind
 hint: its remote counterpart. Merge the remote changes (e.g. 'git pull')
 hint: before pushing again.
 hint: See the 'Note about fast-forwards' in 'git push --help' for details.

## Github Pages deploy complete
cd -
```

## The Fix

I googled around and found no solution. 
After digging into the Rakefile, I came up with a manual fix.
Basically, you need to create and link the `deploy` directory to `master` branch.

First, we clone the repo and switch to the correct branch:

```
git clone https://github.com/username/username.github.io.git
git checkout source
```

Then we need to setup the `deploy` directory.

```
mkdir _deploy
cd _deploy
git init
git remote add -t master -f origin https://github.com/username/username.github.io.git
```

Done! Now we can make changes in `source` branch and use `rake gen_deploy` as usual.
