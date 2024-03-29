Homework 2
================
Martin Stelmach

## Examining Jia You’s Online graphic

I believe the graphic is trying to answer the question of “What are the
infected population of the given disease and how does that population
change after having introduced and implemented a vaccine for the given
disease?”. I believe the focus of this graph is to demonstrate that the
infected population shrinks immensely after a vaccine is created. I
believe the author also wanted to show a ressurgence of the diseases as
of the recent trend of antivaxxers. The graphic Jia You’s used is a good
graphic for the question posed because it is easy to determine when each
disease started and when its respective population dynamics changed by
examining the size of the shape. It is also very easy to see key dates
due to colour differentiation of certain dates. One prominent issue with
the graph is the difficulty of viewing the smaller infected populations
on the graph. For example, Mumps begins to make a resurgence in later
years however, on the graph it looks like the disease is completely
erradicated before it shows up again on the graph.

## First Graphic

The first graphic I made was made with the idea that the goal of the
graphic is to correspond to the question. This means we are looking for
something that easily demonstrates the disipation of cases after a
vaccine has been introduced. The graphic shows the rise of the disease
during its year of discovery and then the number of cases plotted
against a single X axis of years. I chose to allow a free Y axis per
disease because, although the size of each disease is not proportionate
to one another, the question we are asking is with resepect to disease
growth after vaccine implementation. I also chose to divide my y axis by
1000 for easier ledgibility. I ordered the diseases by max value of
cases in the disease so they are asthetically pleasing as according to
Cleveland.

Please note in the Rmarkdown file the diseases seem to be squished
together and the y axis seems crowded however when we zoom the image in
R there seems to be alot more space for everything to
    render.

``` r
library(tidyverse)
```

    ## ── Attaching packages ────────────────────────────────────────────────────── tidyverse 1.2.1 ──

    ## ✔ ggplot2 3.2.1     ✔ purrr   0.2.5
    ## ✔ tibble  1.4.2     ✔ dplyr   0.7.8
    ## ✔ tidyr   0.8.2     ✔ stringr 1.3.1
    ## ✔ readr   1.3.0     ✔ forcats 0.3.0

    ## Warning: package 'ggplot2' was built under R version 3.5.2

    ## ── Conflicts ───────────────────────────────────────────────────────── tidyverse_conflicts() ──
    ## ✖ dplyr::filter() masks stats::filter()
    ## ✖ dplyr::lag()    masks stats::lag()

``` r
library(ggthemes)
```

    ## Warning: package 'ggthemes' was built under R version 3.5.2

``` r
df <- readr::read_csv("https://bbolker.github.io/stat744/data/vaccine_data_online.csv")
```

    ## Parsed with column specification:
    ## cols(
    ##   id = col_double(),
    ##   cases = col_double(),
    ##   disease = col_character(),
    ##   year = col_double(),
    ##   vaccine = col_character(),
    ##   event = col_character(),
    ##   event_date = col_character(),
    ##   head = col_logical()
    ## )

``` r
df2 <- (df %>% mutate(cases2=cases/1000,
             #I want the order of biggest max value
             disease = reorder(disease,cases,FUN=function(x) -x[1]))
          )
#this is my order code (possibly just have ordered it twice)
order <- df %>%
  arrange(desc(cases)) %>%
  pull(disease) %>%
  unique()
#apply the order I just made
df2$disease <- factor(df2$disease, levels = order)

#The graph 
g2 <- ggplot(df2, aes(x = year, y = cases2, colours=disease, fill = disease)) +
      geom_area() +
      theme_minimal()+
  theme(plot.background = element_rect(fill = "cornsilk"),
        plot.title = element_text(face = "bold"),
        strip.text = element_text(face = "bold"),
        axis.title.x = element_text(colour = "azure4"),
        panel.grid.major = element_line(colour = "gray87"),
        panel.grid.minor = element_line(colour = "gray87")) +
  labs(title = "Number of Diseases per Year",
       x = "Year",
       y = "Cases (in Thousands)") +
      facet_grid(disease ~.,scales ="free_y")
```

![](hmrw-rmarkdown2_files/figure-gfm/graph%201-1.png)<!-- -->

## Second Plot

I liked that the 1st one was all on one single X axis however I feel
like it would be more legible to have them all on their own respective
graphs and view each disease in the 3x3 set of graphs as seen below. I
stayed with the same colour theme as the previous graph so it would be
easier to compare the two. The seperation of the graphs into a grid is
an easier way to differentate the diseases. I chose to leave the free Y
axis because without it the smaller diseases were unviewable. I
envisioned to have each row a different Y axis rather than each plot
have its own scale however I was unable to figure that out. Similarly to
the previous graphic this one does not represent each disease to scale
with one another but is rather meant to show the significance of the
addition of a
vaccine.

``` r
g4 <- ggplot(df2, aes(x = year, y = cases2, colours=disease, fill = disease)) +
  geom_area() +
  facet_wrap(~disease, ncol  = 3, scales = "free_y")+
  theme_minimal()+
  theme(plot.background = element_rect(fill = "cornsilk"),
        plot.title = element_text(face = "bold"),
        strip.text = element_text(face = "bold"),
        axis.title.x = element_text(colour = "azure4"),
        panel.grid.major = element_line(colour = "gray87"),
        panel.grid.minor = element_line(colour = "gray87")) +
  labs(title = "Number of Diseases per Year",
       x = "Year",
       y = "Cases (in Thousands)" ) 
```

![](hmrw-rmarkdown2_files/figure-gfm/graph%202-1.png)<!-- -->
