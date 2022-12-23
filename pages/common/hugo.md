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
