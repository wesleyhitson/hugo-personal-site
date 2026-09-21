This blog is made with Hugo, a Go static site generator. The theme is PaperMod.

To run the site locally, run `hugo server`. Site will run on port 1313.

To run the site and see draft posts, run `hugo server -D`.

For some reason, hot reload doesn't seem to work great when adding images, so kill the server and restart if you want to see how your images are looking. I have tried moving this site development to WSL but it ends up causing me more headaches, and VS Code has a markdown previewer anyways: `CTRL + K, V` (and NOT `CTRL + K, CTRL + V `).

In July 2026, added a .nojekyll file to force GitHub to bypass using the Jekyll engine to build the site.

# Setup
Go and Hugo are both required to work on this site. Hugo has good Windows [install instructions](https://gohugo.io/installation/windows/). You can also just go straight to the [releases page](https://github.com/gohugoio/hugo/releases) and download the latest version. 

I created `C:\Hugo\bin` and put the exe there, then added that location to my PATH. 

I also installed [Go](https://go.dev/doc/install).

Once those are both installed, make sure the theme is up to date (I was getting warnings from Hugo about out of date naming conventions). To do that, I had to run:
```bash
git submodule init
git submodule update --remote --merge
```
Then you can run with Hugo.
