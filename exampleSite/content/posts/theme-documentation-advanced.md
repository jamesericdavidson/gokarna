---
weight: 15
title: "Theme Documentation - Advanced"
date: 2020-05-06T21:29:01+08:00
description: "Discover how to maximise Gokarna's potential"
tags: ["installation", "configuration", "markdown"]
type: post
showTableOfContents: true
---

Gokarna is an opinionated theme with a focus on minimalism and simplicity.

## Content types

Gokarna supports two [content types](https://gohugo.io/content-management/types/): `post` and `page` - they are specified in [front matter](https://gohugo.io/content-management/front-matter/).

### Post

A blog post - listed in the `/posts/` section, with indexed [`/tags/`](https://gohugo.io/content-management/taxonomies/#assign-terms-to-content).

```md
---
title: "Hello, world!"
date: 2021-01-01
description: "A blog post"
image: "/path/to/image.png"
type: "post"
tags: ["blog"]
---

## Hello World!

This is my blog.
```

### Page

A typical Markdown page - perfect for detailing your portfolio, projects, or anything else you can imagine.

```md
---
title: "Hello, world!"
image: "/path/to/image.png"
type: "page"
---

# Projects

Keep an eye on this space for my upcoming projects!
```

### Table of Contents

To enable an optional [Table of Contents](https://gohugo.io/configuration/markup/#table-of-contents) for posts and pages, add the following to `config.toml`:

```toml
[markup]
  [markup.tableOfContents]
    startLevel = 2
    endLevel = 4
    ordered = false
```

Note that `.Title` is also used as `<h1>`. You should ['avoid using multiple `<h1>` elements on one page'](https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/Heading_Elements#avoid_using_multiple_h1_elements_on_one_page), therefore a `startLevel` of `2` (or more) is recommended.

Set `showTableOfContents` to `true` in front matter where you want the Table of Contents to appear:

```yaml
---
title: "Hello, world!"
image: "/path/to/image.png"
type: "page"
showTableOfContents: true
---
```

## Copyright notice

Define the [copyright notice for your site](https://gohugo.io/methods/site/copyright/). The notice will only be displayed on [page Kinds](#content-types).

For example, the following configuration in `config.toml` and front matter...

```toml
copyright = "Verbatim copying and distribution of this entire article are
permitted worldwide, without royalty, in any medium, provided this notice is
preserved."

[params]
  footer = "The Marauders"
```

```yaml
date: 2020-06-17
lastmod: 2024-02-05
```

Will produce this footer:

> © 2020-2024 The Marauders Verbatim copying and distribution of this entire article are permitted worldwide, without royalty, in any medium, provided this notice is preserved.

Note that the years of `.Date` and `.Lastmod` are used to create a date range for your copyrighted material.

`copyright` can include [Markdown syntax](https://www.markdownguide.org/tools/hugo/). This is best used for including hyperlinks, emoji, or text formatting.

## Custom HTML

Inject arbitrary HTML into `<head>` and `<footer>`, using `customHeadHTML` and `customFooterHTML` respectively.

### Analytics

Integration with any analytics tool: This was a personal pet peeve. User privacy is our primary concern and this meant not using Google Analytics or any of the popular tools.

We preferred privacy friendly tools like [Umami](https://umami.is/) & [Fathom Analytics](https://usefathom.com/), but the downside was that no theme supported them out of the box which led to either changing the theme source code or contributing supporting code to the original theme (both of which are good ways to extend the theme, but not our ideal choice)

Giving users the freedom to add anything in the HTML via config.toml seemed like an elegant way to solve the problem.

```toml
[params]
  customHeadHTML = '''
    <script async defer data-website-id="website-id" src="https://analytics.example.com/script.js"></script>
  '''
```

### Bring your own scripts

Add your own JavaScript or CSS by putting them in the `static/` folder and importing them into your HTML.

```toml
[params]
  customHeadHTML = '''
    <script>console.log("Custom script or import");</script>
    <script src="/js/custom.js"></script>
  '''
  customFooterHTML = '''
    <div>Comment SDK Integration</div>
    <script>console.log("Custom script or import");</script>
    <script src="/js/custom.js"></script>
  '''
```

### Comments section

Add a comments section to the end of posts by providing a `customCommentHTML` script from your platform of choice.

For example:

<!-- toml syntax highlighting gives forward slashes an ugly red foreground and
    black background -->

```toml
customCommentHTML = """
    <script src="https://utteranc.es/client.js"
        repo="526avijitgupta/gokarna"
        issue-term="title"
        theme="github-dark"
        crossorigin="anonymous"
        async>
    </script>
    """
```

You can style the resulting `<div id="comments">` with CSS.

### Katex

Katex is a math typesetting library for the web which lets you write beautiful equations. To use it, add the javascript as mentioned in [their documentation](https://katex.org/docs/browser.html) in our `params.customHeadHTML`.

```toml
[params]
  customHeadHTML = '''
    <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/katex@0.16.0/dist/katex.min.css" integrity="sha384-Xi8rHCmBmhbuyyhbI88391ZKP2dmfnOl4rT9ZfRI7mLTdk1wblIUnrIq35nqwEvC" crossorigin="anonymous">
    <script defer src="https://cdn.jsdelivr.net/npm/katex@0.16.0/dist/katex.min.js" integrity="sha384-X/XCfMm41VSsqRNQgDerQczD69XqmjOOOwYQvr/uuC+j4OPoNhVgjdGFwhvN02Ja" crossorigin="anonymous"></script>
    <script defer src="https://cdn.jsdelivr.net/npm/katex@0.16.0/dist/contrib/auto-render.min.js" integrity="sha384-+XBljXPPiv+OzfbB3cVmLHf4hdUFHlWNZN5spNQ7rmHTXpd7WvJum6fIACpNNfIR" crossorigin="anonymous"></script>
    <script>
      document.addEventListener("DOMContentLoaded", function() {
        renderMathInElement(document.body, {
          // customised options
          // • auto-render specific keys, e.g.:
          delimiters: [
            {left: '$$', right: '$$', display: true},
            {left: '$', right: '$', display: false},
          ],
          // • rendering keys, e.g.:
          throwOnError : false
        });
      });
    </script>
  '''
```

> Note: Make sure you use the latest version of katex and use the correct script tags as described in [their documentation](https://katex.org/docs/browser.html)

Then the equation `$$y_t = \beta_0 + \beta_1 x_t + \epsilon_t$$` wrapped by double `$$` would be displayed as:

   $$y_t = \beta_0 + \beta_1 x_t + \epsilon_t$$

The equation `$y_t = \beta_0 + \beta_1 x_t + \epsilon_t$` wrapped by single `$` would be displayed inline as $y_t = \beta_0 + \beta_1 x_t + \epsilon_t$.

## Weights

The `weight` attribute can be added in the markdown metadata for `post` types. We have an option in our config.toml: `params.showPostsOnHomePage` which allows you to:

1. Show popular posts on home page if the value is set to `popular`. It sorts the all the posts by it's weight attribute in ascending order.
2. Show recent posts on home page if the value is set to `recent`
3. Do not show anything if the variable is unset or an empty string.

## Homepage

### About description text

In extension to the basic configuration with the `description` field, it's also possible to write the about section using markdown.

Create a file called `_index.md` in the `content` directory and write your content there.

> **Attention**: Don't use frontmatter in this file. It would also render it.

```md
# Gokarna
Gokarna is a small temple town located in the Uttara Kannada district of Karnataka state in southern India.

## Beaches
Something about beaches, **which is *very* important**.

- every
- beach
- is beautiful
```

Having the above about section in place, results in the following homepage:

![Markdown about description](/images/theme-documentation-advanced/homepage-markdown-about-description.png "Markdown about description")


## Icons

Gokarna supports popular social media icons (Github, Linkedin, Twitter, StackOverflow, Dribbble, etc.) out of the box. See full list of supported icons [on GitHub](https://github.com/gokarna-theme/gokarna-hugo/tree/main/static/icons).

### Icons on homepage

To display icons on the homepage, simply update the `socialIcons` config param with a list of name and url of each icon. The specified `name` should exactly match one of the names from [the `icons` directory](https://github.com/gokarna-theme/gokarna-hugo/tree/main/static/svg/icons).
If you want to add more icons, you can download the svg directly from [Simple Icons' website](https://simpleicons.org/)  and place them in your local icons directory (`/static/svg/icons/`)

```toml
  [params]
    socialIcons = [
      {name = "twitter", url = "https://example.com"},
      {name = "linkedin", url = "https://example.com"},
      {name = "stackoverflow", url = "https://example.com"},
    ]
```

Preview:

![Icons on homepage Preview](/images/theme-documentation-advanced/icons-homepage-preview.png "Icons on homepage Preview")


### Icons in header

[Feather](https://feathericons.com) icons has a comprehensive list of icons which are more general purpose and not limited to social media.
Therefore, we use feather as an additional source of icons. Here is an example of how to add custom icons in the header using feather:

```toml
  [[menu.main]]
    identifier = "github"
    url = "https://github.com"
    weight = 3
    # Using feather-icons
    pre = "<span data-feather='github'></span>"
```

The same icon in this case could also be added without feather:

```toml
  [[menu.main]]
    identifier = "github"
    url = "https://www.buymeacoffee.com/"
    weight = 3
    # Without using feather-icons
    pre = "<img class='svg-inject' src='/icons/github.svg' />"
```

You can add `params` allowing menu link to open in a new tab, for example: 
```toml
[[menu.main]]
  identifier = "github"
  url = "https://github.com/zerodahero"
  weight = 4
  # We use feather-icons: https://feathericons.com/
  pre = "<span data-feather='github'></span>"
  [menu.main.params]
    newPage = true
```
## Syntax Highlighting

Hugo lets you choose the color scheme for the codeblocks. You can choose from the options here: https://xyproto.github.io/splash/docs/all.html

After choosing your theme, just update the `pygmentsStyle`  attribute in config.toml.

```toml
pygmentsStyle = "monokai"
```

You can read more about syntax highlighting on the [official hugo docs](https://gohugo.io/content-management/syntax-highlighting/).

## Site Metadata

Gokarna enables you to improve the SEO performance of your website with minimal effort.

### Image preview

We make sure your pages are social media ready.

![Social Media Preview](/images/theme-documentation-advanced/preview.png "Social Media Preview")

```md
---
title: "Hello, world!"
image: "/path/to/image.png"
---
```

> Note: If no image is specified in the markdown metadata, the site avatar is automatically used instead.

### SEO keywords

The keywords relevant for SEO are composed of the page `tags` as defined below:

```md
---
title: "Hello, world!"
tags: ["hello", "world"]
---
```

and the `metaKeywords` specified in the config.toml:

```md
[params]
  metaKeywords = ["blog", "gokarna", "hugo"]
```

## Hide tags, date or description of posts

Tags can be used to categorize posts (e.g.: Project or Blog), and be hidden on
the posts. Use the `params.hiddenTags` field in `hugo.toml`.  
A post's date and description can be hidden if it has at least one tag listed in
`params.tagsHidePostDate` or `params.tagsHidePostDescription`, respectively.


```toml
[params]
  [params.hidden]
  tags = ["project", "blog"]
  tagsPostDate = ["project"]
  tagsPostDescription = ["project"]
  [menu]
    [[menu.main]]
      identifier = "projects"
      url = "/tags/project/"
      name = "My Projects"
      weight = 1
    [[menu.main]]
      identifier = "blog"
      url = "/tags/blog/"
      name = "Blog"
      weight = 2
```

## Minify HTML

`hugo`'s HTML output can be [minified](https://gohugo.io/hugo-pipes/minification/#usage), resulting in smaller files. This makes your site more performant (especially when paired with compression), and may confer a better [Google Lighthouse](https://pagespeed.web.dev/) score.

```toml
[minify]
    minifyOutput = true
```
