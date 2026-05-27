---
tags:
  - subject
  - data-science
  - data-handling
category: Data Handling
---

# Exploratory Data Analysis

> Summarizing and visualizing data to uncover patterns, anomalies, and hypotheses before modeling.

## Overview

- Understanding the problem and the data
- Import and inspect the data
- Handling missing values
- Explore data characteristics
- Perform data transformation
- Visualise data relationships
- Handling outliers
- Communicate findings and insights 

## Key Concepts

- Summary statistics
```python
.shape()
.select()
.filter()
.describe()

missing = (
    df_pl.select(pl.all().is_null().sum())
    .melt(value_name="missing")
    .filter(pl.col("missing") > 0)
)
static = (
    df_pl.select(pl.all().n_unique())
    .melt(value_name="unique")
    .filter(pl.col("unique") == 1)
)
```
- Distribution plots
- Correlation analysis
	- Heatmaps
- Univariate & bivariate analysis
- Outlier detection
	- Normalize data
	- Convert Cartesian coordoniates
	- Divide distribution

## Skills

### Learned
*Skills I'm confident with. Tag with `#learned`.*

-

### Learning
*Skills I'm actively working on. Tag with `#learning`.*

-

### To Learn
*Skills on the roadmap. Tag with `#to-learn`.*

-

## Projects

*Projects that use this subject. These will also show up as backlinks when project notes link here with `[[Exploratory Data Analysis]]`.*



## Resources

*Books, courses, papers, videos.*

-

## Related Subjects

- [[Statistics]]
- [[Data Visualization]]
- [[Data Wrangling]]

## Notes

*Free-form notes, gotchas, insights.*

-
