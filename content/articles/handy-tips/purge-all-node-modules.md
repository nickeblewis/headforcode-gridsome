---
title: Purge all node_modules folders
tags: nodejs,bash,scripts,handy,tips,node_modules,npm,javascript,command-line
excerpt: A great trick I learnt yesterday was how to free up 16GB on my hard drive with just one command line input!
createdAt: 2020-03-31 09:12:00
---

I found this tip the other day via Flavio Copes website and it came just at the right time, since my poor old MacBook (2014 edition) is struggling for space on it's well used SSD drive!

So here is how you can drop all of you old node_modules folders under a given project directory to free up a whole load of space! You don't need to keep all of them 100% of the time, why not clear them down once in awhile!

```bash
find . -name "node_modules" -type d -prune -exec rm -rf '{}’ +
```

Ok it later caught me out, as I tried to run a project and then realised that I had purged its NPM modules!! Simply just re-install them and it does no harm even to update them if your project has gone stale. Though that can also present it's own problems when you hit NPM dependencies that just don't match up anymore.