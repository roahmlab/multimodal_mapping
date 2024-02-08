---
# Front matter. This is where you specify a lot of page variables.
layout: default
title:  "You've Got to Feel It To Believe It: Multi-Modal Bayesian Inference for Semantic and Property Prediction"
date:   2024-02-08 13:24:00 -0400
description: >- # Supports markdown
  This is the project page for the submitted paper
show-description: true

# Add page-specifi mathjax functionality. Manage global setting in _config.yml
mathjax: true
# Automatically add permalinks to all headings
# https://github.com/allejo/jekyll-anchor-headings
autoanchor: true

# Preview image for social media cards
image:
  path: https://cdn.pixabay.com/photo/2019/09/05/01/11/mountainous-landscape-4452844_1280.jpg
  height: 100
  width: 256
  alt: Random Landscape

# Only the first author is supported by twitter metadata
authors:
  - name: Parker Ewen
    footnotes: 1
    url: https://parkerewen.com
    email: pewen@umich.edu
  - name: Hao Chen
    footnotes: 1
    email: haochern@umich.edu
  - name: Yuzhen Chen
    footnotes: 1
    email: yuzhench@umich.edu
  - name: Anran Li
    footnotes: 1
    email: anranli@umich.edu
  - name: Anup Bagali
    footnotes: 1
    email: abagali@umich.edu
  - name: Gitesh Gunjal
    footnotes: 1
    email: gitesh@umich.edu
  - name: Ram Vasudevan
    footnotes: 1
    email: ramv@umich.edu

# If you just want a general footnote, you can do that too.
# See the sel_map and armour-dev examples.
author-footnotes:
  1: All authors affiliated with the Robotics Institute and department of Mechanical Engineering of the University of Michigan, Ann Arbor.

links:
  - icon: arxiv
    icon-library: simpleicons
    text: Arxiv HP
    url: https://arxiv.org/
  - icon: github
    icon-library: simpleicons
    text: Code
    url: https://github.com/roahmlab

# End Front Matter
---

{% include sections/authors %}
{% include sections/links %}


# [Content](#content)
<div markdown="1" class="content-block grey justify no-pre">
some text

Try clicking this heading, this shows the manually defined header anchor, but if you do this, you should do it for all headings.
</div>

I made this look right by adding the `no-pre` class.
If you don't include `markdown="1"` it will fail to render any markdown inside.

You can also make fullwidth embeds (this doesn't actually link to any video)
<div class="fullwidth">
<video controls="" style="background-color:black;width:100%;height:auto;aspect-ratio:16/9;"></video>
</div>

<div markdown="1" class="content-block grey justify">
# Topic inside of the content block

Lorem ipsum dolor sit amet Consectetur adipiscing elit Integer molestie lorem at massa.

![Alt Text](https://cdn.pixabay.com/photo/2019/09/05/01/11/mountainous-landscape-4452844_1280.jpg "Random Image")
</div>

# Topic outside of content block

![Alt Text](https://cdn.pixabay.com/photo/2019/09/05/01/11/mountainous-landscape-4452844_1280.jpg "Random Image")

Lorem ipsum dolor sit amet Consectetur adipiscing elit Integer molestie lorem at massa.

## This is how we can get the image at 100%

<div markdown="1" class="fullwidth">
![Alt Text](https://cdn.pixabay.com/photo/2019/09/05/01/11/mountainous-landscape-4452844_1280.jpg "Random Image")
</div>

## And this is how we can get the image closer

<div markdown="1" class="no-pre">
![Alt Text](https://cdn.pixabay.com/photo/2019/09/05/01/11/mountainous-landscape-4452844_1280.jpg "Random Image")
</div>

Lorem ipsum dolor sit amet Consectetur adipiscing elit Integer molestie lorem at massa.

<div markdown="1" class="cabin">
It's also possible to specify a new font for a specific section
</div>

<div markdown="1" class="jp">
## See? 1
</div>

And you can also <span class="cabin">change it in the middle</span>, though that's a bit more problematic for other reasons.

To specify fonts, just use Google Fonts and update `_data/fonts.yml`.
Any fonts you add as extra fonts at the bottom become usable fonts in the body of the post.

There are also tools to grab icons from other repos.
Just use the following:
{% include util/icons icon='github' icon-library='simpleicons' -%}
, and you'll be able to add icons from any library you have enabled that is supported.

This uses the liquid template engine for importing.
If you include the - at the start of end of such a line, it say to discard all whitespace before or after.
In order to keep the comma there, we added the -.
This is what happens:
{% include util/icons icon='github' icon-library='simpleicons' %}
, when you don't have it (notice the space).

And if you have mathjax enabled in `_config.yml` or in the Front Matter as it is here, you can even add latex:

$$
\begin{align*}
  & \phi(x,y) = \phi \left(\sum_{i=1}^n x_ie_i, \sum_{j=1}^n y_je_j \right)
  = \sum_{i=1}^n \sum_{j=1}^n x_i y_j \phi(e_i, e_j) = \\
  & (x_1, \ldots, x_n) \left( \begin{array}{ccc}
      \phi(e_1, e_1) & \cdots & \phi(e_1, e_n) \\
      \vdots & \ddots & \vdots \\
      \phi(e_n, e_1) & \cdots & \phi(e_n, e_n)
    \end{array} \right)
  \left( \begin{array}{c}
      y_1 \\
      \vdots \\
      y_n
    \end{array} \right)
\end{align*}
$$

You can also treat a section of text as a block, and use kramdown's block attribution methods to change fonts.
You can see at the end of this section in the markdown that I do just that
{: class="cabin"}

<div markdown="1" class="content-block grey justify">
# This is a really long heading block so I can see if justify breaks the heading, and make sure that headings don't get justify unless they are explicitly classed with justify like the following heading

# This is the following really long heading block so I can see if justify breaks the heading, and make sure that only this heading is justified because it has the explicit tag
{: class="justify"}
</div>

<div markdown="1" class="content-block grey justify">
# Citation

*Insert whatever message*

```bibtex
@article{nash51,
  author  = "Nash, John",
  title   = "Non-cooperative Games",
  journal = "Annals of Mathematics",
  year    = 1951,
  volume  = "54",
  number  = "2",
  pages   = "286--295"
}
```
</div>
