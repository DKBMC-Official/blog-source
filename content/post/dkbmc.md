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
image = "/img/about-bg.jpg" #optional image - "/img/about-bg.jpg" is the default
title = "Default .md 테스트"
description = "test test"
draft = false
comments = true
+++
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
        <script>
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