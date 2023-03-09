---
title: "Getting Started"
date: 2023-03-06T12:55:43+01:00
draft: true
tags: ['site']
---

## A new site with Hugo

I decided to create a documentation site for documenting my progress on my graduation project. I don't like ProjectCampus, and i think a static website will fit better into my workflow. Eventually i'll use this website not only for my graduation project, but also as a portfolio site and documentation for other project. 

I'm already a bit familiar with Jekyll, but i'm not satisfied with the themes and find some options confusing. Hugo is more complicated to set-up, but therefore more customizable. I decided to give it a go.


I installed Hugo with Homebrew, which i already had installed. Then i created a new site in ../hugo/Hazal

```

brew install hugo

mkdir hugo

hugo new site Hazal

```

This created the standard file directory for Hugo. Next i followed along with [this youtube video](https://www.youtube.com/watch?v=ZFL09qhKi5I&ab_channel=LukeSmith) and used [this repository](https://github.com/LukeSmithxyz/lugo) because it's extremely simple and i'm planning in building the whole site myself, and this gives me something to work with. 

i changed the config.toml to a yaml file because i've worked with yaml before. I created this getting started page and served the site to take a look. 

![getting-started](/getting-started.png)

It looks horrible but thats fine for now.

## Small changes

I added a tag to this page to test if a tag-list is created. 

I changed the default architype (draft: true -> draft: false) to let new pages not be a draft page when created. 

I deleted rss.xml, rss.svg and the following line in the footer in baseof.html to get rid of all the RSS stuff:

```

{{- if .Param "showrss" }}<br><br><a href="/index.xml"><img src="/rss.svg" style="max-height:1.5em" alt="RSS Feed" title="Subscribe via RSS for updates."></a>{{ end }}

```

I changed the colors of my background and text. At first this did not work. I needed to clear my browser cache to see the changes. To avoid this from happening i can run 

```

hugo server --noHTTPCache

```

## Hosting and deploying my site

I'll use Github pages to host my site. Later i want to take a look in other hosting option such as Netlify, but since i use a static site it doesn't matter much where i host. 

I created a new repository on github named hazalates/documentation

I initialized the local directory as a Git repository. The initial branch us called main

``` 
//initialize local directory as Git repo branch main
git init -b main 

//add files to local repo
git add .

//commits changes and prepare for push
git commit -m "First commit"

//set new remote
git remote add origin https://github.com/hazalates/documentation.git

//verify remote
git remote -v

//push changes
git push -u origin main

```

I ran into an error, because i added a MIT license to my repo when i created it on Github. To fix this i did the following:

```

//merge repository
git config pull.rebase false

//pull repository with unrelated history
git pull origin main --allow-unrelated-histories

```

Next i want to automate the process using Github Actions.

In my repository i went to Settings -> Pages

I set source to GitHub Actions

I created .github/workflows/hugo.yaml and copied [the YAML code](https://gohugo.io/hosting-and-deployment/hosting-on-github/)