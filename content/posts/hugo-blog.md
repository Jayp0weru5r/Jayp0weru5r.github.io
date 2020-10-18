---
title: "Hugo Blog with Github Actions"
subtitle: "How I create this Blog using Hugo with Github Actions"
date: 2020-09-23T10:09:35-04:00
lastmod: 2020-09-23T10:09:35-04:00
draft: false
author: "LeftSecOps"
authorLink: ""
description: ""
tags: ['Hugo','Blog']
categories: ['Blog']

hiddenFromHomePage: false
hiddenFromSearch: false

featuredImage: ""
featuredImagePreview: ""

toc:
  enable: true
math:
  enable: false
lightgallery: false
license: ""
---

<!--more-->

# Current Setup & Flow of Blog

## Hosting on Github Pages

This blog is hosted on Github.com using [Github Pages](https://pages.github.com/) and uses [Hugo](https://gohugo.io/) static site generator to build the content. If you would like to understand how to create a blog and host with Github pages, please see the documentation [here](https://gohugo.io/hosting-and-deployment/hosting-on-github/#types-of-github-pages).

## Creating Content

The current process & flow that I follow for building the content is creating a feature branch locally ```git checkout -b blog-article-title``` after the branch is checked out I generate a new page with ```hugo new posts/my-post-title.md```. This generates the scaffolding and base blog post. After content is created using markdown language its time to review and edit locally before publishing to Github this can be accomplished by executing ```hugo server -D``` which also shows any draft posts that you may have created. 

##  Publishing Website

Once I am happy with the content that I created I push my code to [github.com](https://github.com) and use github actions to build the site. With Github actions I am able to automatically build & publish my site once changes are merged to master. The process & current github actions include on push to master it will trigger the CI & build hugo site & push its contents to gh-pages to the /docs directory. For reference I have attached my current deploy pipeline below:  

```yaml
name: deploy

on:
  push:
    branches:
      - master  # Set a branch to deploy

jobs:
  deploy:
    runs-on: ubuntu-18.04
    steps:
      - uses: actions/checkout@v2
        with:
          submodules: false  # Fetch Hugo themes (true OR recursive)
          fetch-depth: 0    # Fetch all history for .GitInfo and .Lastmod

      - name: Setup Hugo
        uses: peaceiris/actions-hugo@v2
        with:
          hugo-version: '0.75.1'
          extended: true

      - name: Build
        run: hugo --minify

      - name: Deploy
        uses: peaceiris/actions-gh-pages@v3
        with:
          github_token: ${{ secrets.GITHUB_TOKEN }}
          publish_dir: ./docs
          publish_branch: gh-pages
```

# Final Notes

I just want to thank you for taking the time to read my first blog post. Be looking out for new content that relates to security, DevOps, & DevSecOps practices and operations. 


