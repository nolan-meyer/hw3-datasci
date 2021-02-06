---
title: 'Weekly Exercises #3'
author: "Nolan Meyer"
output: 
  html_document:
    keep_md: TRUE
    toc: TRUE
    toc_float: TRUE
    df_print: paged
    code_download: true
---





```r
library(tidyverse)     # for graphing and data cleaning
```

```
## ── Attaching packages ─────────────────────────────────────── tidyverse 1.3.0 ──
```

```
## ✓ ggplot2 3.3.2     ✓ purrr   0.3.4
## ✓ tibble  3.0.5     ✓ dplyr   1.0.2
## ✓ tidyr   1.1.2     ✓ stringr 1.4.0
## ✓ readr   1.4.0     ✓ forcats 0.5.0
```

```
## ── Conflicts ────────────────────────────────────────── tidyverse_conflicts() ──
## x dplyr::filter() masks stats::filter()
## x dplyr::lag()    masks stats::lag()
```

```r
library(gardenR)       # for Lisa's garden data
library(lubridate)     # for date manipulation
```

```
## 
## Attaching package: 'lubridate'
```

```
## The following objects are masked from 'package:base':
## 
##     date, intersect, setdiff, union
```

```r
library(ggthemes)      # for even more plotting themes
library(geofacet)      # for special faceting with US map layout
theme_set(theme_minimal())       # My favorite ggplot() theme :)
```


```r
# Lisa's garden data
data("garden_harvest")

# Seeds/plants (and other garden supply) costs
data("garden_spending")

# Planting dates and locations
data("garden_planting")

# Tidy Tuesday data
kids <- readr::read_csv('https://raw.githubusercontent.com/rfordatascience/tidytuesday/master/data/2020/2020-09-15/kids.csv')
```

```
## 
## ── Column specification ────────────────────────────────────────────────────────
## cols(
##   state = col_character(),
##   variable = col_character(),
##   year = col_double(),
##   raw = col_double(),
##   inf_adj = col_double(),
##   inf_adj_perchild = col_double()
## )
```

## Setting up on GitHub!

Before starting your assignment, you need to get yourself set up on GitHub and make sure GitHub is connected to R Studio. To do that, you should read the instruction (through the "Cloning a repo" section) and watch the video [here](https://github.com/llendway/github_for_collaboration/blob/master/github_for_collaboration.md). Then, do the following (if you get stuck on a step, don't worry, I will help! You can always get started on the homework and we can figure out the GitHub piece later):

* Create a repository on GitHub, giving it a nice name so you know it is for the 3rd weekly exercise assignment (follow the instructions in the document/video).  
* Copy the repo name so you can clone it to your computer. In R Studio, go to file --> New project --> Version control --> Git and follow the instructions from the document/video.  
* Download the code from this document and save it in the repository folder/project on your computer.  
* In R Studio, you should then see the .Rmd file in the upper right corner in the Git tab (along with the .Rproj file and probably .gitignore).  
* Check all the boxes of the files in the Git tab and choose commit.  
* In the commit window, write a commit message, something like "Initial upload" would be appropriate, and commit the files.  
* Either click the green up arrow in the commit window or close the commit window and click the green up arrow in the Git tab to push your changes to GitHub.  
* Refresh your GitHub page (online) and make sure the new documents have been pushed out.  
* Back in R Studio, knit the .Rmd file. When you do that, you should have two (as long as you didn't make any changes to the .Rmd file, in which case you might have three) files show up in the Git tab - an .html file and an .md file. The .md file is something we haven't seen before and is here because I included `keep_md: TRUE` in the YAML heading. The .md file is a markdown (NOT R Markdown) file that is an interim step to creating the html file. They are displayed fairly nicely in GitHub, so we want to keep it and look at it there. Click the boxes next to these two files, commit changes (remember to include a commit message), and push them (green up arrow).  
* As you work through your homework, save and commit often, push changes occasionally (maybe after you feel finished with an exercise?), and go check to see what the .md file looks like on GitHub.  
* If you have issues, let me know! This is new to many of you and may not be intuitive at first. But, I promise, you'll get the hang of it! 



## Instructions

* Put your name at the top of the document. 

* **For ALL graphs, you should include appropriate labels.** 

* Feel free to change the default theme, which I currently have set to `theme_minimal()`. 

* Use good coding practice. Read the short sections on good code with [pipes](https://style.tidyverse.org/pipes.html) and [ggplot2](https://style.tidyverse.org/ggplot2.html). **This is part of your grade!**

* When you are finished with ALL the exercises, uncomment the options at the top so your document looks nicer. Don't do it before then, or else you might miss some important warnings and messages.


## Warm-up exercises with garden data

These exercises will reiterate what you learned in the "Expanding the data wrangling toolkit" tutorial. If you haven't gone through the tutorial yet, you should do that first.

  1. Summarize the `garden_harvest` data to find the total harvest weight in pounds for each vegetable and day of week (HINT: use the `wday()` function from `lubridate`). Display the results so that the vegetables are rows but the days of the week are columns.


```r
garden_harvest %>% 
  mutate(day = wday(date, label = T)) %>% 
  group_by(vegetable, day) %>% 
  summarize(total_wt_lbs = round((sum(weight) * 0.00220462), 2)) %>% 
  arrange(day) %>% 
  pivot_wider(id_cols = vegetable,
              names_from = day,
              values_from = total_wt_lbs)
```

```
## `summarise()` regrouping output by 'vegetable' (override with `.groups` argument)
```

<div data-pagedtable="false">
  <script data-pagedtable-source type="application/json">
{"columns":[{"label":["vegetable"],"name":[1],"type":["chr"],"align":["left"]},{"label":["Sun"],"name":[2],"type":["dbl"],"align":["right"]},{"label":["Mon"],"name":[3],"type":["dbl"],"align":["right"]},{"label":["Tue"],"name":[4],"type":["dbl"],"align":["right"]},{"label":["Wed"],"name":[5],"type":["dbl"],"align":["right"]},{"label":["Thu"],"name":[6],"type":["dbl"],"align":["right"]},{"label":["Fri"],"name":[7],"type":["dbl"],"align":["right"]},{"label":["Sat"],"name":[8],"type":["dbl"],"align":["right"]}],"data":[{"1":"beans","2":"1.91","3":"6.51","4":"4.39","5":"4.08","6":"3.39","7":"1.53","8":"4.71"},{"1":"beets","2":"0.32","3":"0.67","4":"0.16","5":"0.18","6":"11.89","7":"0.02","8":"0.38"},{"1":"broccoli","2":"1.26","3":"0.82","4":"NA","5":"0.71","6":"NA","7":"0.17","8":"NA"},{"1":"carrots","2":"2.94","3":"0.87","4":"0.35","5":"5.56","6":"2.67","7":"2.14","8":"2.33"},{"1":"corn","2":"1.46","3":"0.76","4":"0.73","5":"5.30","6":"NA","7":"3.45","8":"1.32"},{"1":"cucumbers","2":"3.10","3":"4.78","4":"10.05","5":"5.31","6":"3.31","7":"7.43","8":"9.64"},{"1":"jalapeño","2":"0.26","3":"5.55","4":"0.55","5":"0.48","6":"0.22","7":"1.29","8":"1.51"},{"1":"kale","2":"0.83","3":"2.07","4":"0.28","5":"0.62","6":"0.28","7":"0.38","8":"1.49"},{"1":"lettuce","2":"1.47","3":"2.46","4":"0.92","5":"1.19","6":"2.45","7":"1.80","8":"1.32"},{"1":"onions","2":"0.26","3":"0.51","4":"0.71","5":"NA","6":"0.60","7":"0.07","8":"1.91"},{"1":"peas","2":"2.06","3":"4.63","4":"2.07","5":"1.08","6":"3.40","7":"0.94","8":"2.85"},{"1":"peppers","2":"0.50","3":"2.53","4":"1.44","5":"2.44","6":"0.71","7":"0.34","8":"1.38"},{"1":"radish","2":"0.08","3":"0.20","4":"0.09","5":"NA","6":"0.15","7":"0.19","8":"0.23"},{"1":"rutabaga","2":"19.26","3":"NA","4":"NA","5":"NA","6":"NA","7":"3.58","8":"6.90"},{"1":"spinach","2":"0.49","3":"0.15","4":"0.50","5":"0.21","6":"0.23","7":"0.20","8":"0.26"},{"1":"strawberries","2":"0.08","3":"0.48","4":"NA","5":"NA","6":"0.09","7":"0.49","8":"0.17"},{"1":"Swiss chard","2":"1.25","3":"1.07","4":"0.07","5":"0.91","6":"2.23","7":"0.62","8":"0.73"},{"1":"tomatoes","2":"75.61","3":"11.49","4":"48.75","5":"58.27","6":"34.52","7":"85.08","8":"35.13"},{"1":"zucchini","2":"12.24","3":"12.20","4":"16.47","5":"2.04","6":"34.63","7":"18.72","8":"3.41"},{"1":"basil","2":"NA","3":"0.07","4":"0.11","5":"NA","6":"0.03","7":"0.47","8":"0.41"},{"1":"hot peppers","2":"NA","3":"1.26","4":"0.14","5":"0.07","6":"NA","7":"NA","8":"NA"},{"1":"potatoes","2":"NA","3":"0.97","4":"NA","5":"4.57","6":"11.85","7":"3.74","8":"2.80"},{"1":"pumpkins","2":"NA","3":"30.12","4":"31.86","5":"NA","6":"NA","7":"NA","8":"92.69"},{"1":"raspberries","2":"NA","3":"0.13","4":"0.34","5":"NA","6":"0.29","7":"0.57","8":"0.53"},{"1":"squash","2":"NA","3":"24.33","4":"18.47","5":"NA","6":"NA","7":"NA","8":"56.22"},{"1":"cilantro","2":"NA","3":"NA","4":"0.00","5":"NA","6":"NA","7":"0.07","8":"0.04"},{"1":"edamame","2":"NA","3":"NA","4":"1.40","5":"NA","6":"NA","7":"NA","8":"4.69"},{"1":"chives","2":"NA","3":"NA","4":"NA","5":"0.02","6":"NA","7":"NA","8":"NA"},{"1":"kohlrabi","2":"NA","3":"NA","4":"NA","5":"NA","6":"0.42","7":"NA","8":"NA"},{"1":"apple","2":"NA","3":"NA","4":"NA","5":"NA","6":"NA","7":"NA","8":"0.34"},{"1":"asparagus","2":"NA","3":"NA","4":"NA","5":"NA","6":"NA","7":"NA","8":"0.04"}],"options":{"columns":{"min":{},"max":[10]},"rows":{"min":[10],"max":[10]},"pages":{}}}
  </script>
</div>

  2. Summarize the `garden_harvest` data to find the total harvest in pound for each vegetable variety and then try adding the plot from the `garden_planting` table. This will not turn out perfectly. What is the problem? How might you fix it?


```r
garden_harvest %>% 
  group_by(vegetable, variety) %>% 
  summarize(total_wt_lbs = round((sum(weight) * 0.00220462), 2)) %>% 
  left_join(garden_planting,
            by = c("vegetable", "variety")) %>% 
  select(vegetable:plot)
```

```
## `summarise()` regrouping output by 'vegetable' (override with `.groups` argument)
```

<div data-pagedtable="false">
  <script data-pagedtable-source type="application/json">
{"columns":[{"label":["vegetable"],"name":[1],"type":["chr"],"align":["left"]},{"label":["variety"],"name":[2],"type":["chr"],"align":["left"]},{"label":["total_wt_lbs"],"name":[3],"type":["dbl"],"align":["right"]},{"label":["plot"],"name":[4],"type":["chr"],"align":["left"]}],"data":[{"1":"apple","2":"unknown","3":"0.34","4":"NA"},{"1":"asparagus","2":"asparagus","3":"0.04","4":"NA"},{"1":"basil","2":"Isle of Naxos","3":"1.08","4":"potB"},{"1":"beans","2":"Bush Bush Slender","3":"22.13","4":"M"},{"1":"beans","2":"Bush Bush Slender","3":"22.13","4":"D"},{"1":"beans","2":"Chinese Red Noodle","3":"0.78","4":"K"},{"1":"beans","2":"Chinese Red Noodle","3":"0.78","4":"L"},{"1":"beans","2":"Classic Slenderette","3":"3.60","4":"E"},{"1":"beets","2":"Gourmet Golden","3":"7.02","4":"H"},{"1":"beets","2":"leaves","3":"0.22","4":"NA"},{"1":"beets","2":"Sweet Merlin","3":"6.39","4":"H"},{"1":"broccoli","2":"Main Crop Bravado","3":"2.13","4":"D"},{"1":"broccoli","2":"Main Crop Bravado","3":"2.13","4":"I"},{"1":"broccoli","2":"Yod Fah","3":"0.82","4":"P"},{"1":"carrots","2":"Bolero","3":"8.29","4":"H"},{"1":"carrots","2":"Bolero","3":"8.29","4":"L"},{"1":"carrots","2":"Dragon","3":"4.11","4":"H"},{"1":"carrots","2":"Dragon","3":"4.11","4":"L"},{"1":"carrots","2":"greens","3":"0.37","4":"NA"},{"1":"carrots","2":"King Midas","3":"4.10","4":"H"},{"1":"carrots","2":"King Midas","3":"4.10","4":"L"},{"1":"chives","2":"perrenial","3":"0.02","4":"NA"},{"1":"cilantro","2":"cilantro","3":"0.11","4":"potD"},{"1":"cilantro","2":"cilantro","3":"0.11","4":"E"},{"1":"corn","2":"Dorinny Sweet","3":"11.41","4":"A"},{"1":"corn","2":"Golden Bantam","3":"1.60","4":"B"},{"1":"cucumbers","2":"pickling","3":"43.61","4":"L"},{"1":"edamame","2":"edamame","3":"6.09","4":"O"},{"1":"hot peppers","2":"thai","3":"0.15","4":"potB"},{"1":"hot peppers","2":"variety","3":"1.32","4":"potC"},{"1":"jalapeño","2":"giant","3":"9.87","4":"L"},{"1":"kale","2":"Heirloom Lacinto","3":"5.95","4":"P"},{"1":"kale","2":"Heirloom Lacinto","3":"5.95","4":"front"},{"1":"kohlrabi","2":"Crispy Colors Duo","3":"0.42","4":"front"},{"1":"lettuce","2":"Farmer's Market Blend","3":"3.80","4":"C"},{"1":"lettuce","2":"Farmer's Market Blend","3":"3.80","4":"L"},{"1":"lettuce","2":"Lettuce Mixture","3":"4.75","4":"G"},{"1":"lettuce","2":"mustard greens","3":"0.05","4":"NA"},{"1":"lettuce","2":"reseed","3":"0.10","4":"NA"},{"1":"lettuce","2":"Tatsoi","3":"2.89","4":"P"},{"1":"onions","2":"Delicious Duo","3":"0.75","4":"P"},{"1":"onions","2":"Long Keeping Rainbow","3":"3.31","4":"H"},{"1":"peas","2":"Magnolia Blossom","3":"7.46","4":"B"},{"1":"peas","2":"Super Sugar Snap","3":"9.57","4":"A"},{"1":"peppers","2":"green","3":"5.69","4":"K"},{"1":"peppers","2":"green","3":"5.69","4":"O"},{"1":"peppers","2":"variety","3":"3.65","4":"potA"},{"1":"peppers","2":"variety","3":"3.65","4":"potA"},{"1":"peppers","2":"variety","3":"3.65","4":"potD"},{"1":"potatoes","2":"purple","3":"3.01","4":"D"},{"1":"potatoes","2":"red","3":"4.43","4":"I"},{"1":"potatoes","2":"Russet","3":"9.09","4":"D"},{"1":"potatoes","2":"yellow","3":"7.40","4":"I"},{"1":"potatoes","2":"yellow","3":"7.40","4":"I"},{"1":"pumpkins","2":"Cinderella's Carraige","3":"32.87","4":"B"},{"1":"pumpkins","2":"New England Sugar","3":"44.86","4":"K"},{"1":"pumpkins","2":"saved","3":"76.93","4":"B"},{"1":"radish","2":"Garden Party Mix","3":"0.95","4":"C"},{"1":"radish","2":"Garden Party Mix","3":"0.95","4":"G"},{"1":"radish","2":"Garden Party Mix","3":"0.95","4":"H"},{"1":"raspberries","2":"perrenial","3":"1.86","4":"NA"},{"1":"rutabaga","2":"Improved Helenor","3":"29.74","4":"NA"},{"1":"spinach","2":"Catalina","3":"2.03","4":"H"},{"1":"spinach","2":"Catalina","3":"2.03","4":"E"},{"1":"squash","2":"Blue (saved)","3":"41.52","4":"A"},{"1":"squash","2":"Blue (saved)","3":"41.52","4":"B"},{"1":"squash","2":"delicata","3":"10.50","4":"K"},{"1":"squash","2":"Red Kuri","3":"22.73","4":"A"},{"1":"squash","2":"Red Kuri","3":"22.73","4":"B"},{"1":"squash","2":"Red Kuri","3":"22.73","4":"side"},{"1":"squash","2":"Waltham Butternut","3":"24.27","4":"A"},{"1":"squash","2":"Waltham Butternut","3":"24.27","4":"K"},{"1":"strawberries","2":"perrenial","3":"1.31","4":"NA"},{"1":"Swiss chard","2":"Neon Glow","3":"6.88","4":"M"},{"1":"tomatoes","2":"Amish Paste","3":"65.67","4":"J"},{"1":"tomatoes","2":"Amish Paste","3":"65.67","4":"N"},{"1":"tomatoes","2":"Better Boy","3":"34.01","4":"J"},{"1":"tomatoes","2":"Better Boy","3":"34.01","4":"N"},{"1":"tomatoes","2":"Big Beef","3":"24.99","4":"N"},{"1":"tomatoes","2":"Black Krim","3":"15.81","4":"N"},{"1":"tomatoes","2":"Bonny Best","3":"24.92","4":"J"},{"1":"tomatoes","2":"Brandywine","3":"15.65","4":"J"},{"1":"tomatoes","2":"Cherokee Purple","3":"15.71","4":"J"},{"1":"tomatoes","2":"grape","3":"32.39","4":"O"},{"1":"tomatoes","2":"Jet Star","3":"15.02","4":"N"},{"1":"tomatoes","2":"Mortgage Lifter","3":"26.33","4":"J"},{"1":"tomatoes","2":"Mortgage Lifter","3":"26.33","4":"N"},{"1":"tomatoes","2":"Old German","3":"26.72","4":"J"},{"1":"tomatoes","2":"volunteers","3":"51.61","4":"N"},{"1":"tomatoes","2":"volunteers","3":"51.61","4":"J"},{"1":"tomatoes","2":"volunteers","3":"51.61","4":"front"},{"1":"tomatoes","2":"volunteers","3":"51.61","4":"O"},{"1":"zucchini","2":"Romanesco","3":"99.71","4":"D"}],"options":{"columns":{"min":{},"max":[10]},"rows":{"min":[10],"max":[10]},"pages":{}}}
  </script>
</div>

There are a few problems. Some of the vegetable varieties have been plotted in multiple plots, creating duplicate rows of information to include both plot locations. Also, there are some varieties with missing plots which is something we must be considerate of moving forward (these are most likely perennials or reseed). You could try only selecting the first plot location which would reduce some of the unwanted duplicate entries.


  3. I would like to understand how much money I "saved" by gardening, for each vegetable type. Describe how I could use the `garden_harvest` and `garden_spending` datasets, along with data from somewhere like [this](https://products.wholefoodsmarket.com/search?sort=relevance&store=10542) to answer this question. You can answer this in words, referencing various join functions. You don't need R code but could provide some if it's helpful.
  
  You could group by vegetable and find the total amount you spent on seeds in the garden_spending table, and also group by vegetable in the garden_harvest table and find the total amount you harvested for each by weight. Then you could join these tables by vegetable so you have the total weight and total amount spent. Using data from a store, you could also join the prices to the table by vegetable, multiply the store price by your weight, and then find the difference between how much you paid for the seeds and how much it would have cost. 
  

  4. Subset the data to tomatoes. Reorder the tomato varieties from smallest to largest first harvest date. Create a barplot of total harvest in pounds for each variety, in the new order.


```r
garden_harvest %>% 
  filter(vegetable == "tomatoes") %>% 
  group_by(variety) %>% 
  mutate(first_harvest = min(date)) %>% 
  mutate(tot_wt = sum(weight) * 0.00220462) %>% 
  ggplot() +
  geom_col(aes(x = tot_wt, y = fct_reorder(variety, first_harvest))) +
  labs(x = "", y = "", title = "Total harvest weight in lbs by tomato variety")
```

![](03_exercises--1-_files/figure-html/unnamed-chunk-3-1.png)<!-- -->

  5. In the `garden_harvest` data, create two new variables: one that makes the varieties lowercase and another that finds the length of the variety name. Arrange the data by vegetable and length of variety name (smallest to largest), with one row for each vegetable variety. HINT: use `str_to_lower()`, `str_length()`, and `distinct()`.
  

```r
garden_harvest %>% 
  mutate(lwr_case = str_to_lower(variety),
         len_of_var = str_length(variety)) %>% 
  distinct(variety, .keep_all = T) %>% 
  arrange(vegetable, len_of_var) %>% 
  select(vegetable, variety, lwr_case, len_of_var)
```

<div data-pagedtable="false">
  <script data-pagedtable-source type="application/json">
{"columns":[{"label":["vegetable"],"name":[1],"type":["chr"],"align":["left"]},{"label":["variety"],"name":[2],"type":["chr"],"align":["left"]},{"label":["lwr_case"],"name":[3],"type":["chr"],"align":["left"]},{"label":["len_of_var"],"name":[4],"type":["int"],"align":["right"]}],"data":[{"1":"apple","2":"unknown","3":"unknown","4":"7"},{"1":"asparagus","2":"asparagus","3":"asparagus","4":"9"},{"1":"basil","2":"Isle of Naxos","3":"isle of naxos","4":"13"},{"1":"beans","2":"Bush Bush Slender","3":"bush bush slender","4":"17"},{"1":"beans","2":"Chinese Red Noodle","3":"chinese red noodle","4":"18"},{"1":"beans","2":"Classic Slenderette","3":"classic slenderette","4":"19"},{"1":"beets","2":"leaves","3":"leaves","4":"6"},{"1":"beets","2":"Sweet Merlin","3":"sweet merlin","4":"12"},{"1":"beets","2":"Gourmet Golden","3":"gourmet golden","4":"14"},{"1":"broccoli","2":"Yod Fah","3":"yod fah","4":"7"},{"1":"broccoli","2":"Main Crop Bravado","3":"main crop bravado","4":"17"},{"1":"carrots","2":"Dragon","3":"dragon","4":"6"},{"1":"carrots","2":"Bolero","3":"bolero","4":"6"},{"1":"carrots","2":"greens","3":"greens","4":"6"},{"1":"carrots","2":"King Midas","3":"king midas","4":"10"},{"1":"chives","2":"perrenial","3":"perrenial","4":"9"},{"1":"cilantro","2":"cilantro","3":"cilantro","4":"8"},{"1":"corn","2":"Dorinny Sweet","3":"dorinny sweet","4":"13"},{"1":"corn","2":"Golden Bantam","3":"golden bantam","4":"13"},{"1":"cucumbers","2":"pickling","3":"pickling","4":"8"},{"1":"edamame","2":"edamame","3":"edamame","4":"7"},{"1":"hot peppers","2":"thai","3":"thai","4":"4"},{"1":"hot peppers","2":"variety","3":"variety","4":"7"},{"1":"jalapeño","2":"giant","3":"giant","4":"5"},{"1":"kale","2":"Heirloom Lacinto","3":"heirloom lacinto","4":"16"},{"1":"kohlrabi","2":"Crispy Colors Duo","3":"crispy colors duo","4":"17"},{"1":"lettuce","2":"reseed","3":"reseed","4":"6"},{"1":"lettuce","2":"Tatsoi","3":"tatsoi","4":"6"},{"1":"lettuce","2":"mustard greens","3":"mustard greens","4":"14"},{"1":"lettuce","2":"Lettuce Mixture","3":"lettuce mixture","4":"15"},{"1":"lettuce","2":"Farmer's Market Blend","3":"farmer's market blend","4":"21"},{"1":"onions","2":"Delicious Duo","3":"delicious duo","4":"13"},{"1":"onions","2":"Long Keeping Rainbow","3":"long keeping rainbow","4":"20"},{"1":"peas","2":"Magnolia Blossom","3":"magnolia blossom","4":"16"},{"1":"peas","2":"Super Sugar Snap","3":"super sugar snap","4":"16"},{"1":"peppers","2":"green","3":"green","4":"5"},{"1":"potatoes","2":"red","3":"red","4":"3"},{"1":"potatoes","2":"purple","3":"purple","4":"6"},{"1":"potatoes","2":"yellow","3":"yellow","4":"6"},{"1":"potatoes","2":"Russet","3":"russet","4":"6"},{"1":"pumpkins","2":"saved","3":"saved","4":"5"},{"1":"pumpkins","2":"New England Sugar","3":"new england sugar","4":"17"},{"1":"pumpkins","2":"Cinderella's Carraige","3":"cinderella's carraige","4":"21"},{"1":"radish","2":"Garden Party Mix","3":"garden party mix","4":"16"},{"1":"rutabaga","2":"Improved Helenor","3":"improved helenor","4":"16"},{"1":"spinach","2":"Catalina","3":"catalina","4":"8"},{"1":"squash","2":"delicata","3":"delicata","4":"8"},{"1":"squash","2":"Red Kuri","3":"red kuri","4":"8"},{"1":"squash","2":"Blue (saved)","3":"blue (saved)","4":"12"},{"1":"squash","2":"Waltham Butternut","3":"waltham butternut","4":"17"},{"1":"Swiss chard","2":"Neon Glow","3":"neon glow","4":"9"},{"1":"tomatoes","2":"grape","3":"grape","4":"5"},{"1":"tomatoes","2":"Big Beef","3":"big beef","4":"8"},{"1":"tomatoes","2":"Jet Star","3":"jet star","4":"8"},{"1":"tomatoes","2":"Bonny Best","3":"bonny best","4":"10"},{"1":"tomatoes","2":"Better Boy","3":"better boy","4":"10"},{"1":"tomatoes","2":"Old German","3":"old german","4":"10"},{"1":"tomatoes","2":"Brandywine","3":"brandywine","4":"10"},{"1":"tomatoes","2":"Black Krim","3":"black krim","4":"10"},{"1":"tomatoes","2":"volunteers","3":"volunteers","4":"10"},{"1":"tomatoes","2":"Amish Paste","3":"amish paste","4":"11"},{"1":"tomatoes","2":"Cherokee Purple","3":"cherokee purple","4":"15"},{"1":"tomatoes","2":"Mortgage Lifter","3":"mortgage lifter","4":"15"},{"1":"zucchini","2":"Romanesco","3":"romanesco","4":"9"}],"options":{"columns":{"min":{},"max":[10]},"rows":{"min":[10],"max":[10]},"pages":{}}}
  </script>
</div>

  6. In the `garden_harvest` data, find all distinct vegetable varieties that have "er" or "ar" in their name. HINT: `str_detect()` with an "or" statement (use the | for "or") and `distinct()`.




## Bicycle-Use Patterns

In this activity, you'll examine some factors that may influence the use of bicycles in a bike-renting program.  The data come from Washington, DC and cover the last quarter of 2014.

<center>

![A typical Capital Bikeshare station. This one is at Florida and California, next to Pleasant Pops.](https://www.macalester.edu/~dshuman1/data/112/bike_station.jpg){300px}


![One of the vans used to redistribute bicycles to different stations.](https://www.macalester.edu/~dshuman1/data/112/bike_van.jpg){300px}

</center>

Two data tables are available:

- `Trips` contains records of individual rentals
- `Stations` gives the locations of the bike rental stations

Here is the code to read in the data. We do this a little differently than usualy, which is why it is included here rather than at the top of this file. To avoid repeatedly re-reading the files, start the data import chunk with `{r cache = TRUE}` rather than the usual `{r}`.


```r
data_site <- 
  "https://www.macalester.edu/~dshuman1/data/112/2014-Q4-Trips-History-Data-Small.rds" 
Trips <- readRDS(gzcon(url(data_site)))
Stations<-read_csv("http://www.macalester.edu/~dshuman1/data/112/DC-Stations.csv")
```

```
## 
## ── Column specification ────────────────────────────────────────────────────────
## cols(
##   name = col_character(),
##   lat = col_double(),
##   long = col_double(),
##   nbBikes = col_double(),
##   nbEmptyDocks = col_double()
## )
```

**NOTE:** The `Trips` data table is a random subset of 10,000 trips from the full quarterly data. Start with this small data table to develop your analysis commands. **When you have this working well, you should access the full data set of more than 600,000 events by removing `-Small` from the name of the `data_site`.**

### Temporal patterns

It's natural to expect that bikes are rented more at some times of day, some days of the week, some months of the year than others. The variable `sdate` gives the time (including the date) that the rental started. Make the following plots and interpret them:

  7. A density plot, which is a smoothed out histogram, of the events versus `sdate`. Use `geom_density()`.
  

  
  8. A density plot of the events versus time of day.  You can use `mutate()` with `lubridate`'s  `hour()` and `minute()` functions to extract the hour of the day and minute within the hour from `sdate`. Hint: A minute is 1/60 of an hour, so create a variable where 3:30 is 3.5 and 3:45 is 3.75.
  

  
  9. A bar graph of the events versus day of the week. Put day on the y-axis.
  

  
  10. Facet your graph from exercise 8. by day of the week. Is there a pattern?
  

  
The variable `client` describes whether the renter is a regular user (level `Registered`) or has not joined the bike-rental organization (`Causal`). The next set of exercises investigate whether these two different categories of users show different rental behavior and how `client` interacts with the patterns you found in the previous exercises. 

  11. Change the graph from exercise 10 to set the `fill` aesthetic for `geom_density()` to the `client` variable. You should also set `alpha = .5` for transparency and `color=NA` to suppress the outline of the density function.
  


  12. Change the previous graph by adding the argument `position = position_stack()` to `geom_density()`. In your opinion, is this better or worse in terms of telling a story? What are the advantages/disadvantages of each?
  

  
  13. In this graph, go back to using the regular density plot (without `position = position_stack()`). Add a new variable to the dataset called `weekend` which will be "weekend" if the day is Saturday or Sunday and  "weekday" otherwise (HINT: use the `ifelse()` function and the `wday()` function from `lubridate`). Then, update the graph from the previous problem by faceting on the new `weekend` variable. 
  

  
  14. Change the graph from the previous problem to facet on `client` and fill with `weekday`. What information does this graph tell you that the previous didn't? Is one graph better than the other?
  

  
### Spatial patterns

  15. Use the latitude and longitude variables in `Stations` to make a visualization of the total number of departures from each station in the `Trips` data. Use either color or size to show the variation in number of departures. We will improve this plot next week when we learn about maps!
  

  
  16. Only 14.4% of the trips in our data are carried out by casual users. Create a plot that shows which area(s) have stations with a much higher percentage of departures by casual users. What patterns do you notice? (Again, we'll improve this next week when we learn about maps).
  

  
### Spatiotemporal patterns

  17. Make a table with the ten station-date combinations (e.g., 14th & V St., 2014-10-14) with the highest number of departures, sorted from most departures to fewest. Save this to a new dataset and print out the dataset. Hint: `as_date(sdate)` converts `sdate` from date-time format to date format. 
  

  
  18. Use a join operation to make a table with only those trips whose departures match those top ten station-date combinations from the previous part.
  

  
  19. Build on the code from the previous problem (ie. copy that code below and then %>% into the next step.) and group the trips by client type and day of the week (use the name, not the number). Find the proportion of trips by day within each client type (ie. the proportions for all 7 days within each client type add up to 1). Display your results so day of week is a column and there is a column for each client type. Interpret your results.

**DID YOU REMEMBER TO GO BACK AND CHANGE THIS SET OF EXERCISES TO THE LARGER DATASET? IF NOT, DO THAT NOW.**

## GitHub link

  20. Below, provide a link to your GitHub page with this set of Weekly Exercises. Specifically, if the name of the file is 03_exercises.Rmd, provide a link to the 03_exercises.md file, which is the one that will be most readable on GitHub.

## Challenge problem! 

This problem uses the data from the Tidy Tuesday competition this week, `kids`. If you need to refresh your memory on the data, read about it [here](https://github.com/rfordatascience/tidytuesday/blob/master/data/2020/2020-09-15/readme.md). 

  21. In this exercise, you are going to try to replicate the graph below, created by Georgios Karamanis. I'm sure you can find the exact code on GitHub somewhere, but **DON'T DO THAT!** You will only be graded for putting an effort into this problem. So, give it a try and see how far you can get without doing too much googling. HINT: use `facet_geo()`. The graphic won't load below since it came from a location on my computer. So, you'll have to reference the original html on the moodle page to see it.


**DID YOU REMEMBER TO UNCOMMENT THE OPTIONS AT THE TOP?**
