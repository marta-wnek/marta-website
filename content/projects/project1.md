---
categories:
- ""
- ""
date: "2017-10-31T22:26:09-05:00"
description: How political affiliation translated to Brexit Voting
draft: false
image: pic09.jpg
keywords: ""
slug: magna
title: Brexit
---
---

The following ***layout settings*** are made:


```{r, setup, echo=FALSE}
knitr::opts_chunk$set(
  message = FALSE, 
  warning = FALSE, 
  tidy=FALSE,     # display code as typed
  size="small")   # slightly smaller font for code
options(digits = 3)

# default figure size
knitr::opts_chunk$set(
  fig.width=6.75, 
  fig.height=6.75,
  fig.align = "center"
)
```


The following ***packages*** are installed in RStudio:



```{r load-libraries, echo=FALSE}
library(tidyverse)  # Load ggplot2, dplyr, and all the other tidyverse packages
library(mosaic)
library(ggthemes)
library(GGally)
library(readxl)
library(here)
library(skimr)
library(janitor)
library(broom)
library(tidyquant)
library(infer)
library(openintro)
```




# Brexit Plot

## Objectives:

Using my data manipulation and visualisation skills, I used the Brexit results dataframe and conducted an analysis of how political affiliation translated into the Brexit voting. 

*In the plot I used the correct colour for each party (google "UK Political Party Web Colours"), and found the appropriate hex code for colours, instead of using the colours that R gives by default.*



```{r brexit_challenge, echo=FALSE, out.width="100%"}
knitr::include_graphics(here::here("images", "brexit.png"), error = FALSE)

#load the data into R
brexit_results <- read_csv("https://raw.githubusercontent.com/kostis-christodoulou/am01/master/data/brexit_results.csv")

#gain a first understanding of the dataframe
glimpse(brexit_results)
skim(brexit_results)

#we want to have an overview of how the percentage of the party affilitation is related to the percentage in the brexit voting share. Therefore, we want to create a long table that displays the voting percentage per parliament constituency

#only select the most important information
brexit_results_plot_df <- brexit_results %>% 
  select(-c(born_in_uk, male, unemployed, degree, age_18to24)) %>% 
  #create a longer format dataset
  rename (Conservative = con_2015,
          Labour = lab_2015,
          "Lib Dems" = ld_2015,
          UKIP = ukip_2015) %>% 
   pivot_longer(cols = 2:5,
               names_to = "Party",
               values_to = "Party_share")

ggplot(data = brexit_results_plot_df, aes(x = Party_share, y = leave_share, colour = Party)) +
  geom_point(size = 0.2) + 
  geom_lm(interval = "confidence") +
  scale_colour_manual(values = c("#0087DC", "#E4003B", "#FAA61A", "#6D3177"), name="") +
  ylim(0,100) +
  labs(title = "How political affiliation traanslated to Brexit Voting", 
       y = "Leave % in the 2016 Brexit referendum",
       x = "Party % in the UK 2015 general election") +
  theme_light() +
  theme(plot.title = element_text(size = 10),
        axis.title = element_text(size = 10),
        axis.text = element_text(size = 8),
        strip.text.x = element_text(size = 8),
        legend.position = "bottom")

```



![BrexitPlot](brexit1.jpg)





**NOTE:**{.underline}

The plot shows several interesting patterns. Generalizing conclusions should be made cautiously which is why especially the relationship of the parliament constituencies' share of votes for the labour and/or conservative party and the corresponding leave share votes should not be over interpreted. 
However, the stronger trends for the Liberal Democrats as well as the Independent Party may show some interesting tendencies. While parliament constituencies in which the vote share for the Liberal Democrats was relatively low showed above 50% vote shares for Brexit, this vote share quickly depletes as the share of Liberal Democrats affiliation increases. 
An inverse relation with stronger magnitude is to be observed for the UKIP. In underrepresented parliament constituencies the vote share to leave the EU was relatively low but strongly increases as the affiliation to the UKIP grows. 
At around 20% representation of the UKIP in the parliament constituencies the vote shares were up to 75%. This finding should not come as a surprise, as the main campaign positions of the UKIP was to be independent of the EU whereas the Liberal Democrats campaigned to stay.

