This blog is made with Hugo, a Go static site generator. The theme is PaperMod.

To run the site locally, run `hugo server`. Site will run on port 1313.

To run the site and see draft posts, run `hugo server -D`.

For some reason, hot reload doesn't seem to work great when adding images, so kill the server and restart if you want to see how your images are looking. I have tried moving this site development to WSL but it ends up causing me more headaches, and VS Code has a markdown previewer anyways: `CTRL + K, V` (and NOT `CTRL + K, CTRL + V `).

In July 2026, added a .nojekyll file to force GitHub to bypass using the Jekyll engine to build the site.