---
title: "Toc and Other"
date: 2023-03-12T14:50:56+01:00
draft: false
tags: ['site']
toc: true
---

I slightly regret wanting to build the site without a nice theme...

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

I also added this in my default architype. 

This gives me a very ugly TOC at the end of the page, but it's something to work with.

![ExampleTOC](/images/tocaos/toc01.jpeg)