---
title: Adding Local Images to Hexo Posts with Markdown
date: 2021-06-23 01:55:33
keywords: Hexo,Images,Markdown
categories: Hexo
tags:
  - Markdown
---

After testing out a few different static site generators, I settled on Hexo to create this site. Hexo is incredibly simple and flexible and I like the fact that it doesn’t depend on anything other than good old Javascript, HTML and CSS (and of course Node for the build process.)

Content authoring is simple and very much like other static site generators. I just write Markdown posts in my favourite text editor, and let the build process generate static HTML pages.

### The Problem: Limitations of the Hexo asset_img tag
I’m very happy with the Hexo experience overall, but there was one fly in the ointment that made editing and previewing my posts a bit fiddly, and that is that Hexo doesn’t support the Markdown format for adding images to a post, e.g:

```markdown
![Alt Text](folder/image-name.png)
```

Instead, in order to work with the Hexo build process, a [custom tag is required](https://hexo.io/docs/asset-folders.html):

```markdown
{% asset_img image-path.png Alt Text %}
```

While this works fine when deploying to a site or previewing locally with hexo serve, it doesn’t play nicely with Markdown tools like the VS Code markdown preview, and so adds extra steps to the authoring workflow.

### The Solution: Using standard Markdown syntax to add Hexo images
Thankfully, there is an [NPM package](https://www.npmjs.com/package/hexo-asset-link) available that remedies this issue and fits snugly into the Hexo build pipeline, works with Markdown tools and the local Hexo server.

The package is compatible with the Hexo per-post asset folder, so head over to your root _config.yml and ensure the following is set:

```yaml
post_asset_folder: true
```

Then, in your terminal run

```bash
npm i -s hexo-asset-link
```

For example, if you have these files in source/_post/:

> +-- _posts/
> |   +-- 2019-02-14-Test-Post.md
> |   +-- 2019-02-14-Test-Post/
> |     |  +-- Test-Image.png
> |     |  +-- Test-Other-File.pdf

Then in 2019-02-14-Test-Post.md:

**Images**

```markdown
![Alt Text](Test-Image.png "Title Text")
```

**Other Files**

```markdown
[Text](Test-Other-File.pdf)
```

and it will show correctly in both the markdown preview, the Hexo local server, and when built on your site.

### Changing image size in Markdown

Please use HTML tag *img*

```markdown
<img  src="Test-Image.png" width="50%" />
```
