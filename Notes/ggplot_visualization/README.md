# Introduction

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

# Draw a basic bar chart
```r
# Define the time frame
sample <- sample %>%
  mutate(started_at = as.Date(started_at),
         ended_at = as.Date(ended_at))

max_ym <- max(format(sample$started_at, "%Y-%m"))
min_ym <- min(format(sample$started_at, "%Y-%m"))

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

```r
# create another style of bar chart ---------------------------------
bar_chart_2 <- avg_trip_day %>% 
  ggplot() +
  geom_col(mapping = aes(x = start_weekday, y = avg_trip, fill = user_type), position = "dodge") +
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

# External Resource
* [Sequential, diverging and qualitative colour scales from ColorBrewer](https://ggplot2.tidyverse.org/reference/scale_brewer.html)

