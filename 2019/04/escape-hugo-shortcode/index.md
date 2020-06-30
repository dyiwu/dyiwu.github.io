# Escaping Hugo Shortcode

{{% admonition note %}}
This article originally comes from
[Chris Liatas's web site](https://liatas.com/posts/escaping-hugo-shortcodes/) ,
just make a copy here for quick reference.
{{% /admonition %}}

In the rare case you might need to escape (i.e., prevent from executing) 
a shortcode in a Hugo markdown (.md) file, and display the shortcode as 
`{{</* myshortcode */>}}` you will probably find that escaping the double curly
braces with `\` is not working for this, producing a result similar to this
\{{&lt; myshortcode &gt;\}} or this 
\{\{&lt; myshortcode &gt;\}\}
or that the shortcode is executed nevertheless.


A simple method that works at the time of writing, is adding `/*` after the
opening double curly braces and the angle bracket or percent sign 
(i.e., `{{>/*` or `{{%/*` and adding `*/` after the closing angle bracket
or percent sign and double curly braces (i.e., `*/>``}}` or `*/%}}`).
So, if for example, you have the shortcode myshortcode and you want to
use it to a code highlight without being executed during page building
from the Hugo engine, you should try including it in your markdown file as:


<pre><code>```md
{{&lt;/* myshortcode */&gt;}}
```
</code></pre>

The above will produce an output like the following:

<pre><code>
{{</* myshortcode */>}}
</code></pre>

References for further reading:

1. [Escaping Hugo shortcodes](https://liatas.com/posts/escaping-hugo-shortcodes/)
2. [How is the Hugo Doc site showing shortcodes in code blocks?](https://discourse.gohugo.io/t/how-is-the-hugo-doc-site-showing-shortcodes-in-code-blocks/9074)
3. [How To Escape Shortcode In Hugo Template](https://code.luasoftware.com/tutorials/hugo/how-to-escape-shortcode-in-hugo/)

