# hugo

> Template-based static site generator. Uses modules, components, and themes.
> More information: <https://gohugo.io>.

- Create a new Hugo site:

`hugo new site {{path/to/site}}`

- Create a new Hugo theme (themes may also be downloaded from https://themes.gohugo.io/):

`hugo new theme {{theme_name}}`

- Create a new page:

`hugo new {{section_name}}/{{path/to/file}}`

- Build a site to the `./public/` directory:

`hugo`

- Build a site including pages that are marked as a "draft":

`hugo --buildDrafts`

- Build a site to a given directory:

`hugo --destination {{path/to/destination}}`

- Build a site, start up a webserver to serve it, and automatically reload when pages are edited:

`hugo server`


# Custom ...........................................................................................
You have a theme you added as a git submodule and you recently re-cloned your project. Guess what? Your submodule needs to be re-downloaded as well.

You can do this with:

git submodule init
git submodule update
Then your project will load without errors.

## MathJax
- wait for 4 for linebreaks
[Fix MathJax cannot newline in Hugo - KevinZonda's Blog](https://blog.kevinzonda.com/post/fix-mathjax-newline/)
[The line break(\\) is not work · Issue #2312 · mathjax/MathJax · GitHub](https://github.com/mathjax/MathJax/issues/2312)
[mathjax - How to use markdown syntax to write math in Hugo? - Stack Overflow](https://stackoverflow.com/questions/64050359/how-to-use-markdown-syntax-to-write-math-in-hugo)

pandoc/reveal.js: https://discourse.gohugo.io/t/mathjax-newlines-in-hugo-pandoc/28233

## Images
{{< figure src="/path/to/image.png" width="100%" class="center" >}}

# My Page

This is a page with an image.

{{< figure src="/path/to/image.png" width="100%" class="center" >}}

This is some more text below the image.


```md
![image alt text](/path/to/image.png)
<div style="text-align:center; width:100%;">
  ![image alt text](/path/to/image.png)
</div>

```

## Code Blocks
https://gohugo.io/content-management/syntax-highlighting/#highlight-shortcode

themes: https://help.farbox.com/pygments.html
Hugo comes with several built-in themes that you can use, or you can specify the path to a custom CSS file if you want to use a different theme.

{{< highlight python theme="your-theme-name" >}}
# Your Python code goes here
{{< /highlight >}}

Keep in mind that this method will only work if you have the Pygments syntax highlighter installed and configured in your Hugo site. If you are using a different syntax highlighter, you will need to use a different method to change the theme of your code blocks.


## Custom Partials
you can create custom partials and save them in the layouts/partials directory of your theme. Partials are reusable pieces of content that you can include in your templates or pages using the partial template function.

For example, suppose you have a custom partial called header.html that you want to use in your theme.
You can save this file in the layouts/partials directory of your theme, and then include it in your templates or pages using the following syntax:

  {{ partial "header.html" . }}

This will include the contents of the header.html partial at the location where the partial function is used.

Keep in mind that the layouts/partials directory is just one of the places where you can save custom partials in a Hugo theme.
You can also save partials in the static/partials directory or in the archetypes directory if you want to use them in your content files.


## Custom CSS
1. Add the CSS file to the static directory:
You can add your custom CSS file to the static directory of your Hugo site.
Then, you can include the CSS file in your templates or pages using the static template function. For example:
```html
  <link rel="stylesheet" href="{{ "css/custom.css" | static }}">
```

This will include the custom.css file from the static/css directory in your page.

2. Add the CSS file to the static/css directory:
You can also add your custom CSS file directly to the static/css directory of your Hugo site.
Then, you can include the CSS file in your templates or pages using the static template function or by linking to the file directly.
For example:
```html
  <link rel="stylesheet" href="/css/custom.css">
```

3. Use a custom CSS stylesheet in your theme:
If you are using a custom theme, you can add your custom CSS to the theme's CSS stylesheet.
This can be a good option if you want to make global style changes to your site.
To do this, you can add your CSS rules to the css/main.css file in your theme's static directory.
