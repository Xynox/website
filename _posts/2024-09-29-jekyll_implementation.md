---
layout: post
title: "Implementation of Jekyll into my Website"
author: "Joel Wittke"
---

I've faced multiple issues with my implementation of Jekyll into my website.
This blogpost looks into my solutions of those problems, more specifically into relative file paths and hosting onto Github Pages.
<!--preview-->

# Initial problems

Even before starting to implement Jekyll I've hosted (although only for testing purposes) my website on Github Pages (I'm lazy so I'm going to refer to it as GHP) which quite a big setback.
The whole Jekyll documentation including the quick start guide documents the whole process when starting your whole infrastructure on Jekyll. But I wanted to only implement it in certain parts of my website. It also didn't really provide any meaningful documentation when coming to GHP, so I kinda needed to figure it all out by myself.

# First implementation locally

I tried it out first - not with the quick start docs, but the 10 or whatever steps documentation - and it kind of worked out of the box! I quickly tried out the template function and was really impressed: I don't need to implement the whole Navbar etc. onto my pages! But I specifically wanted Jekyll to host my blog sites (like the one you're reading right now!) because GHP doesn't support custom backends (custom CMS) as I would have chosen otherwise. 

# Github Pages did not like me

As I pushed my implementation into my main branch (TIL that you should never delete your branches, there seems to be a stale attribute) and began to configure my deployment, I just winged it at first with the standard implementation from GHP with the following result:

```
Error: The process '/opt/hostedtoolcache/Ruby/3.1.4/x64/bin/bundle' failed with exit code 5
```

Well thanks: I didn't have a clue what the problem was supposed to be, this didn't tell me anything!
Some updates later I didn't even get into the "Setup Ruby" part, I was stuck at "Set up job":

```
Error: An action could not be found at the URI 'https://api.github.com/repos/ruby/setup-ruby/tarball/59444476bbe9e789fc777d8fb4dd456bc057604f'
```

Well, that's a new error I did not have any solution for!

# Deploying to Github Pages (but this time successfully)

Well, turns out, there is a (unbelievably well for my previous experience) [documentation](https://jekyllrb.com/docs/continuous-integration/github-actions/) for the deployment to GHP! My main problem was that I needed to configure the GH Actions jekyll.yml with:

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
#[..]
```

Aaaand... Tada, my website was up and running (again)!

# The problems do not end here

Well, my index.html was showing. But I clicked on any other pages... **404**, Page not found.

Yay, another problem! After abusing the DevTools and figuring out that the filepaths are messed up, I began to poke into different solutions that sounded right to me:
Adding onto all different relative links a dot in front to try and put him into the right folder structure, but no this did not solve anything!

## Github Pages did hate me suddenly

I figured out pretty soon that I needed to configure my _config.yml for the usage of GH Actions/GHP. And some quick additions later (according to my research in the internet) I added besides the url and baseurl parameters the source param. But well...

```
/opt/hostedtoolcache/Ruby/3.3.4/x64/lib/ruby/3.3.0/fileutils.rb:402:in `mkdir': Permission denied @ dir_s_mkdir - /.jekyll-cache (Errno::EACCES)
[...]
Error: Process completed with exit code 1.
```

Great!

## Getting my Website back up

After this devastating error I needed some time to focus on other things and my website was on a old deployment, still with functioning links to the blog side coming from the index.html but not in the other directions and completely missing css on my blog overview. 

But after some time I was motivated again and began working on a fix: Old source param out, this had to be the issue and quite to my advantage I just asked Copilot:
And the AI showed me: I needed to implement into the links a Jekyll-friendly code with the note for Jekyll that it's a relative url!

Quickly pushed this into my main branch, waited it to (hopefully) deploy...

# Finally fixed

... And it deployed... and it was working! I was so relieved, the Navbar was working. Now I just needed to implement some quick fixes to the remaining filepaths and after 10 minutes I finally fixed everything and my locally served Jekyll sites were looking the same on my "real" online presence!



# TLDR

Local filepaths were not working in my Github Deployment, needed to implement parameters into _config.yml and change every line of code into relative file paths with curly braces and comment that it's a relative filepath

