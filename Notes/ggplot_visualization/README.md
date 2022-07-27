# Introduction

```r
# Load libraries --------------------------------------
if (!require("pacman")) install.packages("pacman")
pacman::p_load(here,  
               tidyverse, 
               janitor, # Cleaning column names  
               scales, # Transform axis scales
               ggplot2) 
```

2. Content

# Loading a dataset
```r
# Load the dataset -------------------------------------
sample <- read.csv(here("sample.csv"), header = TRUE)

# Manually edit column names ---------------------------
sample <- sample %>%
  rename(bike_type = bikes)

# Visualize the first few columns of the dataset
sample %>%
  head(10) %>%
```

# Draw a basic density plot
```r
# Change the trip duration column to numeric format ---
sample$trip_duration <- sample$trip_duration %>%
  as.numeric() %>%
  round(2) # round up to 2 decimals

dens_plot <- sample %>%
  filter(trip_duration <240) %>% # only include trip less than 240 minutes
  ggplot() +
  geom_density(mapping = aes(x = trip_duration, fill = user_type), # mapping the x axis and the legend
               alpha = 0.6, color = "black") # 

dens_plot # visualize the plot
```

# Adding layers into the plot
```r
# Add title, ubtitle, axis title ----------------------
dens_plot <- dens_plot +
  labs(title = "Density Map of Trip Duration", 
       subtitle = "Trip Duration less than Four Hours",
       x = "Trip Duration",
       y = "Density",
       fill = "Membership")

dens_plot
```
# Modify how it looks
```r
# Add theme elements into the plot -------------------
dens_plot <- dens_plot +
  theme_bw() +
  theme(
    title = element_text(face = "bold", color = "black"),
    axis.title = element_text(face = "bold", color = "black"),
    panel.grid.major.x = element_blank()
  )

dens_plot
```
![](https://github.com/Lingxi-HUANG/R_Notes/blob/main/Notes/ggplot_visualization/figure/Dense_plot.jpeg)

4. Column graph in various forms (step by step to add layer)
5. 
