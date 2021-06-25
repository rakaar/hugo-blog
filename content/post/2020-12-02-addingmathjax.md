---
date: "2020-12-02T00:00:00Z"
mathjax: true
subtitle: Including mathjax in your blog
tags: random
title: Adding Latex Support for math equations in your blog on Github Pages
---

If you have a blog, hosted on github pages where you adding your posts in markdown(like most of the blogs built using Jekyll have), here is how you can write math using latex in your blogs. 

### Step 1
In your `_includes` folder, create a file by name - `mathjax.html` and paste the content present [here](https://github.com/rakaar/rakaar.github.io/blob/master/_includes/mathjax.html)

The value of `inlineMath` key in the above code, means you must write your latex code between two `$` symbols for example to write the famous Einstien's enerngy mass equialent relation, you need to write something like this `$E=mc^2$`


### Step 2
In `_layouts/base.html`, you need to include `mathjax.html` in the way shown in below image.(You can refer [this](https://github.com/rakaar/rakaar.github.io/blob/master/_layouts/base.html))
![Including mathjax.html](/img/include.png)


### Step 3
Finally in the blog post, where you want mathjax to do its job. Include `mathjax: true` in the top of every post alongside the title, subtitle and tags, for example for this blog post, it would be something like this
![Include something like this at the top of the your post file - XYZ.md](/img/top-mathjax.png)

## Testing
Pythogorean theorem $ x^2 + y^2 = z^2 $


## Credits
To [this](https://github.com/rava-dosa/rava-dosa.github.io) repo, that belongs to Apoorva Kumar. Infact the Github repo for this blog, is a result of a blatant clone of his repo. 

**P.S** I could not include the important text directly as it was messing up the blog. By that I mean, it was rendering content and code when I included the needed text as it is in the blog. Hence I tried my best by including images and links.

