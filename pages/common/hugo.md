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
## MathJax
- wait for 4 for linebreaks
[Fix MathJax cannot newline in Hugo - KevinZonda's Blog](https://blog.kevinzonda.com/post/fix-mathjax-newline/)
[The line break(\\) is not work · Issue #2312 · mathjax/MathJax · GitHub](https://github.com/mathjax/MathJax/issues/2312)
[mathjax - How to use markdown syntax to write math in Hugo? - Stack Overflow](https://stackoverflow.com/questions/64050359/how-to-use-markdown-syntax-to-write-math-in-hugo)
pandoc/reveal.js: https://discourse.gohugo.io/t/mathjax-newlines-in-hugo-pandoc/28233

[Hugo Bear Blog |Hugo Themes](https://themes.gohugo.io/themes/hugo-bearblog/)

## Images
There are a few ways to link images.

Option 1. Put all of your images in the static/ directory. Then reference the image file with a leading slash, e.g.:

![Scenario 1: Across columns](/across_column.png)

Option 2. Use sub-directories to hold the markdown file and any related resources.

create a directory post/creating-a-new-theme
move your existing markdown file into that directory, and rename it to index.md
create a subdirectory post/creating-a-new-theme/images and move your images in there
reference the image as ![Image alt](images/my-image.jpg)
