---
layout: post
title: Add Commenting System to Blog
date: 2017-06-14
categories: blog maintenance
comments: true
---

##Install Disqus to Jekyll blog

1.Add a variable called comments to the YAML Front Matter and set its value to true

I add comments variable to each post `.md` file so that comments for each post can be controlled separately. 

2.In between a {% raw %} `{% if page.comments %}` {% endraw %}and a {% raw %} `{% endif %}` {% endraw %}tag, copy and paste the Universal Embed Code in the appropriate template where you'd like Disqus to load.

I create a new file called `disqus.html` under `_includes` folder, and put the universal code into it.


    <div id="disqus_thread"></div>
    <script>

        /**
         *  RECOMMENDED CONFIGURATION VARIABLES: EDIT AND UNCOMMENT THE SECTION BELOW TO INSERT DYNAMIC VALUES FROM YOUR PLATFORM OR CMS.
         *  LEARN WHY DEFINING THESE VARIABLES IS IMPORTANT: https://disqus.com/admin/universalcode/#configuration-variables*/
        var disqus_config = function () {
            this.page.url = "http://www.arkshagong.com";  // Replace PAGE_URL with your page's canonical URL variable
            this.page.identifier = PAGE_IDENTIFIER; // Replace PAGE_IDENTIFIER with your page's unique identifier variable
        };
        (function() { // DON'T EDIT BELOW THIS LINE
            var d = document, s = d.createElement('script');
            s.src = 'https://Also change this to site/embed.js';
            s.setAttribute('data-timestamp', +new Date());
            (d.head || d.body).appendChild(s);
        })();
    </script>
    <noscript>Please enable JavaScript to view the <a href="https://disqus.com/?ref_noscript">comments powered by Disqus.</a></noscript>


3.Display comment on post

in post.html template, add {% raw %} `{% include disqus.html %}` {% endraw %} to section under the post content.

4.Display comment count: Place the following code before site's closing </body> tag

```
<script id="dsq-count-scr" src="//arkshagong.disqus.com/count.js" async></script>
```

It is easy to add a third party comment service to jekyll blog, the official site has a complete guidance for each platform like wordpress and other popular blog platforms.

There is a little issue of display comment block. After I add code block of liquid code include disqus.html ,it always recoginze code as an expression not a code block and ''' seems does not work around it.

It is because Jekyll basically use liquid expression as templating language, it can convert curly braces in markdown. like `include` is a tag to include some small page snippets to site, it also has `highlight` tag to render a code block with syntax highlighting. and gist and links and so on and on.

One way to solve this it to add {% raw %} `{% raw %}` {% endraw %} and {% raw %} `{%`  ``` endraw ``` `%}`{% endraw %} block around code block, but it is kind of disappointed if there are some small code blocks scatter along the file.

But when it is time to add raw expression itself? it can be solved by adding a YAML frontmatter variable to represent left curly brace. Then you can output curly brace by using {% raw %} `{{ page.lcb}}` {% endraw %} this form. Urrrrrhhhh, to me it is not that convenient though, after all add another variable to the frontmatter is kind of obey rules for an OCD coder.

__Next issue is to solve how to display comments for most recent post on home page.__

P.S. Every Time I forgot form of Markdown. Need to write more post to remember it.

