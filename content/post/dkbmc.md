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
]
authors = [
    "BongHoon Ko"
]
image = "/img/about-bg.jpg" #optional image - "/img/about-bg.jpg" is the default
title = "Default .md 테스트"
description = "test test"
draft = false
comments = true
+++
# THIS IS A H1
## THIS IS A H2

> BlockQuote

<!-- >> BlockQuote2

>>> BlockQuote3

>>> BlockQuote3 -->

paragraph!<code>paragraph!</code>paragraph!`paragraph`!paragraph!paragraph!paragraph!paragraph!
paragraph!<kbd>paragraph!</kbd>paragraph!paragraph!paragraph!paragraph!
paragraph!<mark>paragraph!</mark>paragraph!

TEST PPP

1. One
2. Two
3. Three

* TEST1
* THIS2

- - -
syntax: [title](http://www.naver.com)

*this*
_ddd_
**Double**
__Double__
~~Cancel~~
![이미지](https://camo.githubusercontent.com/202c9ae1d457d6109be6c4cf13db9cac5fd708a6/687474703a2f2f6366696c65362e75662e746973746f72792e636f6d2f696d6167652f32343236453634363534334339423435333243374230)

<pre>
    &lt;html&gt;
        &lt;head&gt;&lt;/head&gt;
        &lt;body&gt;
            &lt;p&gt;TEST&lt;/p&gt;
        &lt;/body&gt;
    &lt;/html&gt;
</pre>

{{< highlight go "hl_lines=8 15-17,linenostart=199" >}}
    <html>
        <head></head>
        <body></body>
        <script type="text/javascript">
            var test = document.getElementById("thisId");
            test.style.display = "none";
            $(".test").css({"width":"100%","opacity":"0.5"});
        </script>
    </html>
{{< / highlight >}}

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