+++
tags = [
    "go",
    "golang",
    "templates",
    "themes",
    "development",
]
categories = [
    "Development",
    "golang",
    "Hugo"
]
authors = [
    "고봉훈",
    "이지은"
]
image = "/img/about-bg.jpg" #optional image - "/img/about-bg.jpg" is the default
title ="test"
description = "test"
draft = false
comments = true
+++
<h2>1. Title</h2>
{{< highlight html >}}
<section id="main">
  <div>
    <h1 id="title">{{ .Title }}</h1>
    {{ range .Pages }}
      {{ .Render "summary"}}
    {{ end }}
  </div>
</section>
{{< /highlight >}}
<blockquote>dddddd</blockquote>