---
layout:         post
title:          STACKED BARS
subtitle:       Use dplyr, tidyr, and ggplot2 to quickly make a stacked and facetted bar chart
card-image:     https://context-dependent.github.io/assets/images/stacked_bar.png
date:           2017-09-03 14:14:00
tags:           code R ggplot2 tidyverse
post-card-type: image
---

## Import Packages

Install these packages if you don't have them already

```r
library(tidyverse)
library(ggthemes)
```

## Make fake game data

This is a streamlined version of the structure we talked about.
You won't need to use this chunk for plotting your game data, it's just
for example purposes, though this is a nice way to make small tables within
a script.
```R
stats <- tribble(
    ~player, ~game, ~assists, ~goals,
   "Ondrej",     1,        2,      1,
   "Ondrej",     2,        0,      2,
   "Ondrej",     3,        4,      0,
  "Mateosh",     1,        2,      2,
  "Mateosh",     2,        6,      1,
  "Mateosh",     3,        1,      2,
   "Daniel",     1,        2,      0,
   "Daniel",     2,        0,      1,
   "Daniel",     3,        0,      0
)

stats
```

## Reshape the points and assists columns

the `gather` function takes multiple columns and turns them into a key
column, which uses the input column names as values, an a value
column, which uses the input column values as values. In this case,
I'm naming the key column `point_source`, and the value column `points`.
After reshaping, we're left with a row for each player, game, and point_source.


```r
stats_long <- stats %>%
  gather(point_source, points, assists:goals)

stats_long
```

## Plot results for each player

These columns make sense if you think about the way that ggplot works.
ggplot wants exactly one decision per one column of input data. We want
a bar plot for each player, with a bar that represents the number of points
that they scored in a game, coloured by the point source. n this instance,
we are asking ggplot to make 4 decisions: X-axis values (game), Y-axis values
(points), bar fill colour (point_source), and plot facets (player).

```r
stats_long %>%
  ggplot(aes(game, points, fill = point_source)) +
    geom_bar(stat = "identity") +
    facet_wrap(~player) +
  # The following two lines change the look of the plot
  # there are other scales and themes to choose from, these
  # are just two that I like
    scale_fill_wsj() +
    theme_hc()
```

![points per player per game]({{ site.url }}/assets/images/stacked_bar.png)
