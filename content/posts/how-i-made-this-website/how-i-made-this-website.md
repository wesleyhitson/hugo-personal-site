---
title: "How I Made This Website"
date: 2024-08-04T19:28:53-04:00
draft: true
tags: ["hugo", "jamstack", "how-to"]
---
If you're reading this, that means I was able to get this website up and running. This site is running on the Hugo framework, which is a static site generator written in Go with the PaperMod theme. I have written a static site before with Pelican, which is based on Python. There are static site generators written in all sorts of languages -- I tend to gravitate towards the ones that I like for programming, but in reality, the language doesn't matter as much because most of the writing that I do is some form of markdown. If I wanted to start modifying the workings of the site, or modifying the template itself, then maybe I would dig into the code, but I digress. Here's a general overview of what I did to get this site off the ground, and how I overcame some snags along the way.

The first step is to pick a framework based from some options on the [Jamstack site](https://jamstack.org/generators/). Some of the ones that interest me are Next.js and Hugo, but you'll also find Pelican if you scroll a bit. I chose Hugo because I didn't really want to deal with React for a simple blog, and I am interested in the Go ecosystem at the moment. I chose one from the top of the list because I am assuming that the more popular ones will have better documentation and more themes available.

I went to the Hugo [quickstart page](https://gohugo.io/getting-started/quick-start/) and followed the instructions. After running `hugo server`, I was looking at my new website.

I followed the rest of the tutorial, then started looking for a theme that I liked from the [Hugo themes page](https://themes.gohugo.io/). This is where I hit my first small snag. I found a theme that I liked the look of, but the docs were pretty lacking. I tried getting it set up but I got frustrated and gave up. I don't want to name names, that's not the point of this, but what I'm getting at is that there isn't an obvious way to sort the themes by popularity (GitHub stars, in this case). I find that the more popular ones tend to have better docs and are easier to find information about, just like any other web technology. The themes are sorted by popularity on the theme "home" page, but as soon as you start filtering by the categories on the right, the filtering goes away. I knew I wanted a blog theme, so I immediately filtered down to that and lost my ordering. 

![Hugo theme blog tag](/posts/how-i-made-this-website/hugo-theme-blog-tag.png "Blog Tags")

What I discovered though, is that once you've picked a theme, you can use the left and right arrows on the upper right of the theme page to switch between them. That let me more easily see which ones were blog themes that I might like. 

![Theme sorting](/posts/how-i-made-this-website/hugo-theme-sort.png "Sorting")

In the end, I ended up selecting [PaperMod](https://github.com/adityatelange/hugo-PaperMod), which is the most popular one on the site. Go figure.

This is where I started following the [docs](https://github.com/adityatelange/hugo-PaperMod) specifically for the theme. The installation section specifies that I should use YAML format for the config, so I actually need to create a new Hugo site because as far as I'm aware, there isn't a way to change an existing site from TOML to YAML. Then I used the Git Submodule theme install method (method #2). 

The rest of this article is going to go over how I overcame any obstacles I encountered that weren't in the docs. 

The docs show a sample "config.yaml", but the file is actually called "hugo.yaml" in the project. Maybe that's super obvious to someone who has worked with Hugo a lot, but that could be more clear.

In a similar vein, when I was generating a new post with `hugo new content`, the page was still getting creating with the TOML front matter. In order to change that, I had to modify the [archetype](https://gohugo.io/content-management/archetypes/). This is something I had to quite a bit of Googling to even figure out what to search for/call it. But once I knew what it was called, it was straightforward to change by creating/modifying archetypes/default.md:
```
---
title: "{{ replace .File.ContentBaseName '-' ' ' | title }}"
date: {{ .Date }}
draft: true
---

```

So now I've got pretty much everything setup and working the way I want. The last thing I wanted to get working before I'd consider publishing this first article is images. I'm not going to sugarcoat it - this was *extremely* difficult to figure out for something that should, in my opinion, be one of the core features that they explain. If you're branding your theme as a blogging theme, I expect to be able to add images without too much trouble. Now, with my rant out of the way, I will explain what finally worked.

The ["Features" docs](https://github.com/adityatelange/hugo-PaperMod/wiki/Features) explain how to get a cover image for a post, but that's not what I want. I want an image in the middle of my article. There doesn't seem to be any docs about how to do that anywhere. I tried putting images in the static/ folder and referencing them, but that didn't seem to work. I started Googling and found a Stack Overflow [post](https://stackoverflow.com/questions/48023746/hugo-server-not-rendering-images-from-static-folder/57129742#57129742) from someone who is having a similar issue, and the answer from Anton is enlightening. I will let you read it for yourself if you so choose, but in short, I'm not supposed to use the static/ directory for images in posts, I'm supposed to use something called [page bundles](https://gohugo.io/content-management/page-bundles/).

Page bundles are essentially subdirectories that group content together. So the article and included images are all under the same subdirectory under the `content/posts/` directory. So my directory structure looks like this:

```
| - content/posts
|   | - how-i-made-this-website
|   |   | - how-i-made-this-website.md
|   |   | - hugo-theme-blog-tag.png
```

That lets me reference an image with `![Hugo theme blog tag](/posts/how-i-made-this-website/hugo-theme-blog-tag.png "Blog Tags")` in the markdown file.

Finally! Images, the last piece of the puzzle.

For a few more bells and whistles, I added some custom CSS to justify the text. Those instructions are found [here](https://adityatelange.github.io/hugo-PaperMod/posts/papermod/papermod-faq/). In short, I had to create `assets/css/extended/custom.css` and add my rule (taken from [this](https://github.com/rhazdon/hugo-theme-hello-friend-ng/issues/20) GitHub issue): 

```
.post-content p {
    text-align: justify;
}
```

The same page also has favicon instructions. I found that I have to hard refresh (Ctrl+Shift+R) for changes with images to take effect, including with the favicon. I ended up using [RedKetchup](https://redketchup.io/favicon-generator) to generate my favicon because they let me generate a round one with a clear background. Thanks to the creators of this great tool!

All in all, this was quite a bit more involved than I expected it to be. I would have liked to see some better theme recommendations, and of course, I would love if the theme docs were actually correct and explained how to do everything instead of just saying "look at the example site", because the example site doesn't have everything that I want. But I am optimistic about the speed and flexibility that Hugo gives, and I'm sure I will learn its quirks. For what it's worth, the gohugo.io docs seem to be very thorough (as that's where I ultimately learned about both archetypes and page bundles), so maybe I need to give that a more in-depth read. 

Thanks for reading, and I hope that this is the first of many articles on this site.