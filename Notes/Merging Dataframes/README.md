# Merging Dataframes

## Introduction
One of the important steps of data cleaning is to put several dataset together. so I list out a few examples of dataframe merging. 

### Import data into RStudio
```{r}
GPA <- read.csv(".../.../GPA.csv", header = T)
DOB <- read.csv(".../.../DOB.csv", header = T)
Postal <- read.csv(".../.../Postal Code.csv", header = T)
```

<p align="center">  
<img src="https://github.com/Lingxi-HUANG/R_Notes/blob/main/Notes/Merging%20Dataframes/Figure/DOB.JPG"
width="600"></center>  
</p>  

### Merge two dataframes
Merge the first two dataframes with only overlapping observations
```{r}
M1 <- merge(GPA, DOB)
View(M1)
```

Merge the first two dataframes with all observations where empty values to be reflected with NA
```{r}
M2 <- merge(GPA, DOB, all = TRUE)
View(M2)
```

### Merge dataframes which column names needed to be mapped and specificed
```{r}
M3 <- merge(M2, Postal, by.x = c("Name", "Gender", "ID"), by.y = c("col.1", "col.2", "ID"), all = TRUE)
View(M3)
```


### Left join to merge dataframes using base R
```{r}
M4 <- merge(M2, Postal, by = "ID", by.x = c("Name", "Gender", "ID"), by.y = c("col.1", "col.2", "ID"), all = TRUE)
```
