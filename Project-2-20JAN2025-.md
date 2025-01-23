Data Visualisation Portfolio Project
================
Md Mominul Islam
2025-01-24

- [Introduction](#introduction)
- [Task 1: How has life expectancy changed over time in different
  continents?](#task-1-how-has-life-expectancy-changed-over-time-in-different-continents)
- [Task 2: Is there a relationship between GDP per capita and life
  expectancy?](#task-2-is-there-a-relationship-between-gdp-per-capita-and-life-expectancy)
- [Task 3: Which countries have experienced the fastest population
  growth?](#task-3-which-countries-have-experienced-the-fastest-population-growth)
- [Conclusion](#conclusion)

# Introduction

This project analyzes the `gapminder` dataset to explore trends in life
expectancy, GDP per capita, and population growth over time. The
following tasks are covered:

1.  How has life expectancy changed over time in different continents?
2.  Is there a relationship between GDP per capita and life expectancy?
3.  Which countries have experienced the fastest population growth?

# Task 1: How has life expectancy changed over time in different continents?

``` r
# Summarize data by year and continent
life_expectancy_summary <- gapminder %>%
  group_by(year, continent) %>%
  summarize(avg_lifeExp = mean(lifeExp), .groups = "drop")

# Plot life expectancy over time by continent
ggplot(life_expectancy_summary, aes(x = year, y = avg_lifeExp, color = continent)) +
  geom_line(size = 1.2) +
  labs(
    title = "Life Expectancy Over Time by Continent",
    x = "Year",
    y = "Average Life Expectancy",
    color = "Continent"
  ) +
  theme_minimal()
```

![](Project-2-20JAN2025-_files/figure-gfm/life-expectancy-over-time-1.png)<!-- -->

# Task 2: Is there a relationship between GDP per capita and life expectancy?

``` r
# Scatter plot of GDP per capita vs. life expectancy
ggplot(gapminder, aes(x = gdpPercap, y = lifeExp, color = continent)) +
  geom_point(alpha = 0.6) +
  scale_x_log10() +
  labs(
    title = "Relationship Between GDP per Capita and Life Expectancy",
    x = "GDP per Capita (log scale)",
    y = "Life Expectancy",
    color = "Continent"
  ) +
  theme_minimal()
```

![](Project-2-20JAN2025-_files/figure-gfm/gdp-vs-life-expectancy-1.png)<!-- -->

# Task 3: Which countries have experienced the fastest population growth?

``` r
# Load necessary libraries (I want to visualize data in map. its maybe easy for this type of data)
library(rworldmap)
library(rworldxtra)

# Calculate population growth rate
population_growth <- gapminder %>%
  group_by(country) %>%
  summarize(
    pop_growth_rate = (max(pop) - min(pop)) / min(pop),
    continent = unique(continent),
    .groups = "drop"
  ) %>%
  arrange(desc(pop_growth_rate))

# Top 20 countries with fastest population growth (it's extra)
top20_countries <- population_growth %>% head(20)

# Merge with map data
map_data <- joinCountryData2Map(population_growth, joinCode = "NAME", nameJoinColumn = "country")
```

    ## 139 codes from your data successfully matched countries in the map
    ## 3 codes from your data failed to match with a country code in the map
    ## 104 codes from the map weren't represented in your data

``` r
# Plot population growth on the map
mapParams <- mapCountryData(map_data, nameColumnToPlot = "pop_growth_rate", 
                            mapTitle = "Fastest Population Growth by Country",
                            catMethod = "quantiles", 
                            colourPalette = "heat")
```

![](Project-2-20JAN2025-_files/figure-gfm/fastest-population-growth-1.png)<!-- -->

# Conclusion

This project explored trends in life expectancy, GDP per capita, and
population growth using the `gapminder` dataset. Key insights include: -
Life expectancy has generally increased over time across all
continents. - There is a positive relationship between GDP per capita
and life expectancy, though it varies by continent. - Certain countries,
particularly in Africa and Asia, have experienced the fastest population
growth.
