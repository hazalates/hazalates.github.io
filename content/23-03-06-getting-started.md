---
title: "Getting Started"
date: 2023-03-06T12:55:43+01:00
draft: false
tags: ['site']
toc: true
---

## A new site with Hugo

I created a documentation site for documenting my progress on my graduation project. I'm not too fond of ProjectCampus, and I think a static website will fit better into my workflow. Eventually, I'll use this website not only for my graduation project but also as a portfolio site and documentation for other projects. 

I'm already a bit familiar with Jekyll, but I'm not satisfied with the themes and find some options confusing. Hugo is more complicated to set-up but, therefore more customizable. I decided to give it a go.


I installed Hugo with Homebrew, which I already had installed. Then I created a new site in ../hugo/Hazal

```

brew install hugo

mkdir hugo

hugo new site Hazal

```

This created the standard file directory for Hugo. Next, i followed along with [this youtube video](https://www.youtube.com/watch?v=ZFL09qhKi5I&ab_channel=LukeSmith) and used [this repository](https://github.com/LukeSmithxyz/lugo) because it's extremely simple and I'm planning to build the whole site myself, and this gives me something to work with. 

I've changed the config.toml to a yaml file because I've worked with yaml before. I created this 'getting started' page and served the site to take a look. 

![getting-started](/images/23-03-06-getting-started/getting-started.png)

It is looking horrible but that's ok for now.

## Small changes

I added a tag to this page to test if a tag list is created. 

I changed the default archetype (draft: true -> draft: false) to let new pages not be a draft page when created. 

I deleted rss.xml, rss.svg and the following line in the footer in baseof.html to get rid of all the RSS stuff:

```

{{- if .Param "showrss" }}<br><br><a href="/index.xml"><img src="/rss.svg" style="max-height:1.5em" alt="RSS Feed" title="Subscribe via RSS for updates."></a>{{ end }}

```

I changed the colors of my background and text. At first, this didn't work. I had to clear my browser cache to see any changes. To avoid this from happening I can run:

```

hugo server --noHTTPCache

```

## Hosting and deploying my site

I'll use Github pages to host my site. Later I want to take a look at other hosting options such as Netlify, but since I use a static site it doesn't matter much where I host. 

I created a new repository on github named hazalates/documentation.

I initialized the local directory as a Git repository. The initial branch is called main.

``` 
//initialize local directory as Git repo branch main
git init -b main 

//add files to local repo
git add .

//commits changes and prepares for push
git commit -m "First commit"

//set new remote
git remote add origin https://github.com/hazalates/documentation.git

//verify remote
git remote -v

//push changes
git push -u origin main

```

I ran into an error because I added an MIT license to my repo when I created it on Github. To fix this I did the following:

```

//merge repository
git config pull.rebase false

//pull repository with unrelated history
git pull origin main --allow-unrelated-histories

```

Next, I want to automate the process using Github Actions.

In my repository I went to Settings -> Pages

I set the source to GitHub Actions

I created .github/workflows/hugo.yaml and copied [the YAML code](https://gohugo.io/hosting-and-deployment/hosting-on-github/)

``` 

git add .

git commit -m "Add actions"

```

I couldn't push because of the error "refusing to allow a Personal Access Token to create or update workflow"
I fixed this with the following steps:

Go to avatar -> Settings -> Developer settings -> Personal access token -> Generate new token
Select workflow
Copy token key
Go to Keychain Acces 
select Github entry
Change password to token key

``` 

git push -u origin main

```

Now I was able to push. 

The site didn't come out as expected, because there are some specifics in naming a site on github pages. I renamed the repo in github to hazalates.github.io and then edited the origin URL. I also edited the base URL in my config.yaml

```
git remote set-url origin https://github.com/hazalates/hazalates.github.io.git

``` 

## Adding a table of content and other stuff

My site is still very much under construction. I'll add more features soon.

![Under-construction](/images/23-03-06-getting-started/under-construction.jpeg)