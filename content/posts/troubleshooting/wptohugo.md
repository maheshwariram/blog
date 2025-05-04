---
date: '2025-05-04T04:28:10+02:00'
title: 'issues faced while migrating from WordPress to Hugo with PaperMod'
tags: [migrating, Wordpress, Hugo, PaperMod, front matter, regex]
---

### issue 1: no navigation links at the bottom of the post

### issue 2: no posts displayed on the homepage

TL;DR: remove `type: post` (if present) from the front matter.\
this should solve both the issues, as Hugo looks for `type: posts`, and you are explicitly stating that your content is of the "section" `post`, and not `posts`.

**why?**

Hugo has a clever system for organizing content and choosing layouts.

* **content sections & types:** by default, Hugo determines a content file's "section" based on its directory (e.g., files in `content/posts/` belong to the `posts` section). it also often infers the content "type" from the section name. your `hugo.yaml` correctly identified the main section with `mainsections: posts`.
* **the conflict:** by explicitly setting `type: post` (singular) in the front matter, i might have been forcing Hugo to look for a layout specifically named for the singular `post` type (like `layouts/post/list.html` or `themes/PaperMod/layouts/post/list.html`).\
it seems either such a layout didn't exist, was flawed, or the PaperMod theme expected list pages to be associated with the *plural* section name (`posts`) derived from the directory or `mainsections` setting.\
removing `type: post` allowed Hugo to correctly use the type inferred from the directory (`posts`) and find the working default list layout (`themes/PaperMod/layouts/_default/list.html`).

### the solution

if your posts are in `content/posts/` and your `hugo.yaml` has `mainsections: posts`, you generally don't need to specify `type: post` in the front matter of individual posts. Hugo is smart enough to figure it out from the directory structure.

**simply remove the `type: post` line from your post front matter.**

```yaml
---
title: your awesome post title
authors: ["your name"]
draft: false
# type: post  <-- remove this line
date: 2025-05-04
categories: ["some category"]
tags: ["some-tag"]
---

your post content starts here...
```

#### finding & replacing this using regex

> in all of my files, the next line started with `date: <...>`, and i used this regex to remove the `type: post` line *and its line break*.

```regex
(^\s*type:\s*post\s*\r?\n)(^\s*date:)
```
