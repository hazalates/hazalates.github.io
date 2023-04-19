---
title: "Site improvement"
date: 2023-03-12T14:50:56+01:00
draft: false
tags: ['site']
toc: true
---

I slightly regret wanting to build the site without a nice theme... so here i'll document all the changes and improvements I make.

## TOC

One of the fist features i want to add is a table of contents. For this i did the following:

In the config.yaml: 

```

markup:
  tableOfContents:
    endLevel: 3
    ordered: false
    startLevel: 2

```

In layouts/_default/single.html:

```

{{ define "main" }}
<main>
    <article>
    <header>
        <h1>{{ .Title }}</h1>
    </header>
        {{ .Content }}
    </article>
    <aside>
        {{ .TableOfContents }}
    </aside>
</main>
{{ end }}

```

In the front matter of the posts:

```

toc: true

```

I also added this in my default archetype. 

This gives me a very ugly TOC at the end of the page but it's something to work with.

![ExampleTOC](/images/23-03-12-site-improvement/toc01.jpeg)

## Allowing HTML in markdown files

I tried to add images in a row, with some CSS and HTML but wasn't able to make it work. Than i tried to add a video and stumbled upon the following: <q cite="https://discourse.gohugo.io/t/how-to-use-video/22332/6"> If using Hugo 0.60.+ then the unsafe flag needs to be set in the config for Goldmark (the new Markdown parser) to accept the HTML." </q>

So I went to my config.yaml and added

``` 

markup:
  goldmark:
    renderer:
      unsafe: true

```

### Images in a row

Now I should be able to add raw HTML in my markdown files so I'll try to make 3 images show next to each other instead of underneath.

I add this code to my CSS file:

```
* {
  box-sizing: border-box;
}
/* Three image containers (use 25% for four, and 50% for two, etc) */
.column {
  float: left;
  width: 33.33%;
  padding: 5px;
}

/* Clear floats after image containers */
.row::after {
  content: "";
  clear: both;
  display: table;
}

```

And this code to my page:

```

<div class="row">
  <div class="column">
    <img src="/images/23-03-12-site-improvement/site01.png" alt="pepper" style="width:100%">
  </div>
  <div class="column">
    <img src="/images/23-03-12-site-improvement/site01.png" alt="pepper2" style="width:100%">
  </div>
  <div class="column">
    <img src="/images/23-03-12-site-improvement/site01.png" alt="pepper2" style="width:100%">
  </div>
</div>

```

But nothing shows up... This might have something to do with CSS float, or maybe I didn't address the images correctly. I'll look into it later.

BUT THEN

I found [this site](https://anaulin.org/blog/hugo-raw-html-shortcode/) explaining how to create a shortcode to allow raw html in Hugo and I was able to add a video to [this page](/23-04-12-interface/)

And I tried again... And it worked!

{{< rawhtml >}}
<div class="row">
  <div class="column-3">
    <img src="/images/23-03-12-site-improvement/site01.png" alt="pepper" style="width:100%">
  </div>
  <div class="column-2">
    <img src="/images/23-03-12-site-improvement/site01.png" alt="pepper2" style="width:100%">
  </div>
  <div class="column-2">
    <img src="/images/23-03-12-site-improvement/site01.png" alt="pepper2" style="width:100%">
  </div>
</div>
{{< /rawhtml >}}