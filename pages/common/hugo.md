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

## Ops
- remove/empty baseURL from config for relative URLs (`absURL` not working then)
```bash
hugo server (-D)  # with drafts

# You have a theme you added as a git submodule and you recently re-cloned your project. Guess what? Your submodule needs to be re-downloaded as well.
git submodule init
git submodule update
```

## Concepts
- [page bundles](https://gohugo.io/content-management/page-bundles/), those directories with index.md or `_index.md` files at their root.
- Page resources are only available to the page with which they are bundled.

### Themes and Layouts
- your layout files will override theme layouts with the same name and relative location.
- [website/layouts at main · letsencrypt/website · GitHub](https://github.com/letsencrypt/website/tree/main/layouts): no thema

## MathJax
- wait for 4 for linebreaks
[Fix MathJax cannot newline in Hugo - KevinZonda's Blog](https://blog.kevinzonda.com/post/fix-mathjax-newline/)
[The line break(\\) is not work · Issue #2312 · mathjax/MathJax · GitHub](https://github.com/mathjax/MathJax/issues/2312)

[mathjax - How to use markdown syntax to write math in Hugo? - Stack Overflow](https://stackoverflow.com/questions/64050359/how-to-use-markdown-syntax-to-write-math-in-hugo)
pandoc/reveal.js: https://discourse.gohugo.io/t/mathjax-newlines-in-hugo-pandoc/28233

[How to Add Math Expressions to Hugo Blog - the Shortest Guide Possible](https://www.mrnice.dev/posts/how-to-add-math-expressions-to-hugo-blog-the-shortest-guide-possible/#inline-math-expressions)
[Hugo Bear Blog |Hugo Themes](https://themes.gohugo.io/themes/hugo-bearblog/)

### Gotcha
```latex
- boldsymbol instead of bold

- Use \textbf instead of \text:
  \boldsymbol{A_{\textbf{Circle}}}

\\() instead of $..$
line-break: \\\\ instead of \\
```


## Images
Option 1.
- Put all of your images in the static/ directory. Then reference the image file with a leading slash:
- ![Scenario 1: Across columns](/across_column.png)

Option 2.
- Use sub-directories to hold the markdown file and any related resources.
- create a directory `post/creating-a-new-theme`
- move your existing markdown file into that directory, and rename it to `index.md`
- create subdirectory `./images` and move your images in there
- reference the image as ![Image alt](images/my-image.jpg)

### Center and size
{{< figure src="/path/to/image.png" width="100%" class="center" >}}


## Video
[Hugo : create a shortcode for local videos | Roneo.org](https://roneo.org/en/hugo-create-a-shortcode-for-local-videos/)
If your video link is:

https://www.youtube.com/watch?v=w7Ft2ymGmfc
{{< youtube w7Ft2ymGmfc >}}


## Code Blocks
- configuration of theme: `config.toml`
- https://gohugo.io/content-management/syntax-highlighting/#highlight-shortcode
- themes: https://help.farbox.com/pygments.html
- you can specify the path to a custom CSS file if you want to use a different theme.
```toml
# code black themes: https://stackoverflow.com/a/38861541/8212129
PygmentsCodeFences = true
PygmentsStyle = "pastie"  #"friendly" # "native"  #"emacs"  #"vim"  #!"autumn"  #!"tango"  #!"trac" #!"vs"   #! "murphy"   #"default" # "borland" # "manni" # "darcula" "bw"
```

## Custom Partials
you can create custom partials and save them in the layouts/partials directory of your theme.
Partials are reusable pieces of content that you can include in your templates or pages using the partial template function.

For example, suppose you have a custom partial called `header.html` that you want to use in your theme.
You can save this file in `layouts/partials` of your theme, and then include it in your templates or pages using the following syntax:

  {{ partial "header.html" . }}

This will include the contents of the `header.html` partial at the location where the partial function is used.

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


## Google Analytics
[Integrate Google Analytics with Hugo Theme](https://www.kiroule.com/article/integrate-google-analytics-with-hugo-theme/)

# Gotcha ...........................................................................................
- baseURL must not have trailing slash

## Config not working
/Users/Q187392/dev/s/private/sysid-blog/layouts/partials/footer.html:
inline equation marker not working
[javascript - MathJax config is not taking ## as inline Math delimiter - Stack Overflow](https://stackoverflow.com/questions/72124916/mathjax-config-is-not-taking-as-inline-math-delimiter)
