# ggplot and data visualization examples

- [Introduction](#Introduction)
- [Load a dataset](#Loading-a-dataset)
- [Draw a density plot](#Draw-a-basic-density-plot)
- [Draw a bar chart](#Draw-a-basic-bar-chart)

# Introduction
Ggplot is a powerful tool in R that is designed to visualize data as it allows flexibility for author to create varies graphs through add or minus elements/layers. However, such flexibility can sometimes cause complications and difficulties because users hardly memorize all codes and arguments for adding ggplot layers. This post is served as a note book that stores examples of varies ggplot and data visualization examples. The content will continue update...


```r
# Load libraries --------------------------------------
if (!require("pacman")) install.packages("pacman")
pacman::p_load(here,  
               tidyverse, 
               janitor, # Cleaning column names  
               scales, # Transform axis scales
               ggplot2,
               lubridate) # Transform string to datetime    
```


# Load a dataset
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
![](https://github.com/tomtomhuang/R_Notes/blob/main/Notes/ggplot_visualization/figure/Dense_plot_1.jpeg)

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
![](https://github.com/tomtomhuang/R_Notes/blob/main/Notes/ggplot_visualization/figure/Dense_plot_2.jpeg)

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

# Draw a basic bar chart
```r
# Define the time frame
sample <- sample %>%
  mutate(started_at = as.Date(started_at),
         ended_at = as.Date(ended_at))

max_ym <- max(format(sample$started_at, "%Y-%m-%d"))
min_ym <- min(format(sample$started_at, "%Y-%m-%d"))

# Create a basic bar chart with a title
bar_chart <- ggplot(data = sample) +
  geom_bar(mapping = aes(x = bike_type, fill = bike_type)) +
  labs(title = "Popularity of Bike Types",
       subtitle = paste0("From ", min_ym, " to ", max_ym),
       x = "Bikes",
       y = "Number of Rides")

bar_chart # visualize the chart
```
![](https://github.com/Lingxi-HUANG/R_Notes/blob/main/Notes/ggplot_visualization/figure/Bar_chart.jpeg)

# Add aesthetic elements
```r
# modify the color, theme and y axis ------------------------
bar_chart <- bar_chart +
  scale_fill_brewer() +
  scale_y_continuous(labels = unit_format(unit = "K", scale = 1e-3)) +
  theme_bw() +
  theme(
    title = element_text(face = "bold", color = "black"),
    axis.title = element_text(face = "bold", color = "black"),
    panel.grid.major.x = element_blank()
  )

bar_chart
```
![](https://github.com/Lingxi-HUANG/R_Notes/blob/main/Notes/ggplot_visualization/figure/Bar_chart_2.jpeg)

```r
# average trip duration in different hour of the week ---------------
avg_trip_day <- sample %>%
  group_by(start_weekday, user_type) %>%
  summarize(avg_trip = mean(trip_duration))

# create a bar chart separated by membership type
bar_chart_1 <- avg_trip_day %>% ggplot() +
  geom_col(mapping = aes(x = start_weekday, y = avg_trip, fill = user_type)) +
  facet_wrap(~user_type) +  # separately show the plot based on user_type
  labs(title = "Trip length across the week",
       subtitle = paste0("From ", min_ym, " to ", max_ym),
       x = "Weekday",
       y = "Trip length",
       fill = "Membership") +
  scale_fill_brewer() + 
  theme_bw() +
  theme(
    title = element_text(face = "bold", color = "black"),
    axis.title = element_text(face = "bold", color = "black"),
    panel.grid.major.x = element_blank()
  )

bar_chart_1 # visualize the plot
```
![](https://github.com/tomtomhuang/R_Notes/blob/main/Notes/ggplot_visualization/figure/Bar_chart_3.jpeg)


```r
# create another style of bar chart ---------------------------------
bar_chart_2 <- avg_trip_day %>% 
  ggplot() +
  geom_col(mapping = aes(x = start_weekday, y = avg_trip, fill = user_type), position = position_dodge(width = 0.9)) +
  labs(title = "Trip length across the week",
       subtitle = paste0("From ", min_ym, " to ", max_ym),
       x = "Weekday",
       y = "Trip length",
       fill = "Membership") +
  scale_fill_brewer() + 
  theme_bw() +
  theme(
    title = element_text(face = "bold", color = "black"),
    axis.title = element_text(face = "bold", color = "black"),
    panel.grid.major.x = element_blank(),
    panel.grid.major.y = element_blank(),
    panel.grid.minor.y = element_blank()
  )

bar_chart_2 # visualize the plot
```
![](https://github.com/tomtomhuang/R_Notes/blob/main/Notes/ggplot_visualization/figure/Bar_chart_4.jpeg)

# Adding value labels of the plot

### Value labels for multiple columns
The key is to position = position_dodge(width = .9) (where .9 is the default width of the bars) instead of position = "dodge", which is just a shortcut without any parameter. Also, we need to tell which variable to dodge on. In this case it is the user_type, so it is necessary to put fill = user_type within the aes().

```r
bar_chart_3 <- bar_chart_2 +
  scale_y_continuous(limits = c(0,70)) + # set the y-axis from 0 to 70
  geom_text(aes(x = start_weekday, y = avg_trip, label = avg_trip, fill = user_type), position = position_dodge(width = 0.9), vjust = -0.5) # The key is to position = position_dodge(width = .9) (where .9 is the default width of the bars) instead of position = "dodge", which is just a shortcut without any parameter. 

bar_chart_3 # visualize the plot
```
![](https://github.com/tomtomhuang/R_Notes/blob/main/Notes/ggplot_visualization/figure/Bar_chart_5.jpeg)

```r
# Labeling values on the plot -------------------------------------

# calculate the median trip duration 
med_trip_day <- sample %>%
  group_by(start_weekday) %>%
  summarize (median_trip_duration = round(median(trip_duration),1))

# create a bar chat with value labels 
bar_chart_4 <- med_trip_day %>%
  ggplot() +
  geom_col(mapping = aes(x = start_weekday, y = median_trip_duration), color = "black", fill = "darkcyan") +
  labs(title = "Trip length across the week",
       subtitle = paste0("From ", min_ym, " to ", max_ym),
       x = "Weekday",
       y = "Trip length") +
       theme_bw() +
  theme(
    title = element_text(face = "bold", color = "black"),
    axis.title = element_text(face = "bold", color = "black"),
    panel.grid.major.x = element_blank(),
    panel.grid.major.y = element_blank(),
    panel.grid.minor.y = element_blank()
  ) +
  scale_y_continuous(limits = c(0, 20)) + # set the y axis spanning from 0 to 20
  geom_text(aes(x = start_weekday, y = median_trip_duration, label = median_trip_duration), vjust = -1)

bar_chart_4
```
![](https://github.com/tomtomhuang/R_Notes/blob/main/Notes/ggplot_visualization/figure/Bar_chart_6.jpeg)

# External Resource
* [Sequential, diverging and qualitative colour scales from ColorBrewer](https://ggplot2.tidyverse.org/reference/scale_brewer.html)
* Tips for value lables in multiple columns from [`here`](https://stackoverflow.com/questions/6017460/position-geom-text-on-dodged-barplot), [`here`](https://stackoverflow.com/questions/26660525/add-text-on-top-of-a-faceted-dodged-bar-chart/26661791#26661791) and [`here`](https://stackoverflow.com/questions/34889766/what-is-the-width-argument-in-position-dodge)

