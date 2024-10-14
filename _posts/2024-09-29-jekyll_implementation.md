---
layout: post
title: "Implementation of Jekyll into my Website"
author: "Joel Wittke"
---

In this post, I'll share how I solved issues when implementing Jekyll on my website, specifically around handling relative file paths and hosting on Github Pages (GHP).
<!--preview-->

# Initial Problems

Before integrating Jekyll, I had my website hosted on GHP (I'm lazy so I'm going to refer to it as GHP) for testing. Starting with GHP was a bit of a setback since the Jekyll documentation assumes you're starting from scratch with Jekyll and doesn't provide much guidance for GHP. I wanted to integrate Jekyll into specific parts of my site rather than converting everything to Jekyll, so I had to figure out a lot on my own.

# First Local Implementation

I initially skipped the quick start guide and went for the ["10 steps" documentation](https://jekyllrb.com/docs/step-by-step). Things mostly worked out of the box! I was impressed with Jekyll's templating, as it allowed me to manage navigation components like the navbar across pages without duplicating code. My goal was to use Jekyll to host my blog pages (like this one!), as GHP doesn’t support custom backends.

# Github Pages Deployment Challenges

After pushing my changes to the main branch (note to self: don’t delete branches, they are indeed gone forever), I set up the deployment using GHP’s standard process. I immediately ran into this error:

```
Error: The process '/opt/hostedtoolcache/Ruby/3.1.4/x64/bin/bundle' failed with exit code 5
```

That was cryptic. I had no idea what went wrong. After further attempts, I hit another roadblock:

```
Error: An action could not be found at the URI 'https://api.github.com/repos/ruby/setup-ruby/tarball/59444476bbe9e789fc777d8fb4dd456bc057604f'
```

Well, that was a new error I did not have any solution for!

# Deploying Successfully to Github Pages

Eventually, I found a surprisingly detailed [documentation](https://jekyllrb.com/docs/continuous-integration/github-actions/) on deployment to GHP with GH Actions! My main problem was that I needed to configure the GH Actions jekyll.yml with:

```yml
jobs:
    build:
        #[...]
        steps:
            #[...]
            - name: Setup Ruby
              uses: ruby/setup-ruby@v1
              with:
                ruby-version: '3.3.4'
                bundler-cache: true
            #[...]
#[...]
```

Finally, my website was up and running (again)!

# The Filepath Problem

Well, my index.html was showing. But I clicked on any other pages... **404**, Page not found.

Yay, another problem! After debugging with the DevTools and figuring out that the filepaths were messed up, I tried adding dots to relative links to fix the folder structure, but that didn’t help.

## Configuring `_config.yml` for GitHub Pages

I figured out pretty soon that I needed to configure my `_config.yml` for the usage of GH Actions/GHP. I added besides the `url`, `baseurl` and the `source` parameters. But well...

```
/opt/hostedtoolcache/Ruby/3.3.4/x64/lib/ruby/3.3.0/fileutils.rb:402:in `mkdir': Permission denied @ dir_s_mkdir - /.jekyll-cache (Errno::EACCES)
[...]
Error: Process completed with exit code 1.
```

New error!

## Finding a Fix

I took a break and let the website stay on an old deployment.

But after some time I was motivated again and began working on a fix: Old source param out, this had to be the issue and with some help from AI I figured out that I needed to implement a Jekyll-friendly code into the links with the note for Jekyll that it's a relative url!

Quickly pushed this into my main branch, waited for it to (hopefully) deploy...

# Result

And it deployed and it was in a working state! I was so relieved, the Navbar was working. Now I just needed to implement some quick fixes to the remaining filepaths and after 10 minutes I finally fixed everything.



# TL;DR

I had issues with local file paths not working on Github Pages. I resolved this by configuring `_config.yml` and modifying links to use Jekyll’s relative file paths.

