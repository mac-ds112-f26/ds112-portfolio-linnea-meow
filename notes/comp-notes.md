# Comp Notes

## Table of Contents

- [Pro Tips](#pro-tips)
- [The Pipe Operator](#the-pipe-operator)
- [Scales, Legends and Accessibility](#scales-legends-and-accessibility)
- [Wrangling Verbs](#wrangling-verbs)
- [Choosing a Plot](#choosing-a-plot)

---

## Pro Tips

### Package install failed? Change the CRAN server

**Tools → Global Options → Packages → Change** (Primary CRAN repository)

> [!TIP]
> University of Michigan has been a reliable mirror for me.

---

## The Pipe Operator

### `|>`: R's native pipe

The pipe passes data from one step to the next, like a Mario pipe.

```r
# Keep only stores in the contiguous US
starbucks_us <- starbucks |>
  filter(Country == "US", !State.Province %in% c("AK", "HI"))
```

### Why use it?

```r
# Traditional nested way (harder to read, inside out)
sqrt(mean(c(1, 4, 9)))

# Native pipe way (reads left to right)
c(1, 4, 9) |> mean() |> sqrt()
```

### `|>` vs `%>%`

| Pipe | Source | Notes |
|------|--------|-------|
| `\|>` | Base R (built in) | No package needed |
| `%>%` | tidyverse (magrittr) | Does the same thing, with a few subtle differences |

---

## Scales, Legends and Accessibility

### Continuous vs. discrete

| Variable type | Also called | Example scale |
|---------------|-------------|---------------|
| **Continuous** | Numerical | `scale_color_viridis_c()` |
| **Discrete** | Categorical | `scale_color_viridis_d()` |

### Titling a legend

```r
labs(color = "penguin species")  # legend title for species color codes
```

### Alt text

Add alt text so screen readers can describe your plots.

---

## Wrangling Verbs

### Creating columns

| Verb | What it does |
|------|--------------|
| `mutate()` | Creates **new columns** from existing ones |

### Summarizing

**Practice:** Fill in the blank to calculate the average spending across all candidates.

```r
elections %>%
  ___(average_spending = mean(spend_total))

#>   average_spending
#> 1            11080
```

<details>
<summary><b>Answer</b></summary>

`___` = **`summarize`**

</details>

### Verbs that pair with `summarize()`

| Verb | What it does |
|------|--------------|
| `group_by()` | Splits the data by a **categorical** variable |
| `filter()` | Keeps only the **rows** that meet a condition |
| `select()` | Keeps only specific **columns** (variables) |
| `arrange()` | **Sorts** rows, usually by a **numerical** variable |

### `filter()` + `summarize()`

Filter first, then summarize the result. Example: the fastest 14-year-old.

```r
running %>%
  filter(age == 14) %>%
  summarize(fastest = min(run_time))
```

---

## Choosing a Plot

*(1) = one variable, (2) = two variables*

### Categorical variables

| # | Plot |
|---|------|
| (1) | Bar graph |
| (2) | Stacked bar graph: raw counts **or** % |
| (2) | Dodged bar graph: adjacent blocks |
| (2) | Filled bar graph: each bar out of 100% |
| (2) | Faceted bar graphs: one per group |

### Numerical variables

| # | Plot | ggplot2 |
|---|------|---------|
| (1) | Density plot | `geom_density()` |
| (1) | Histogram | `geom_histogram()` |
| (1) | Box plot | `geom_boxplot()` |
| (1) | Violin plot | `geom_violin()` |
| (2) | Scatterplot | `geom_point()` |
| (2) | Smooth line (can be layered on a scatterplot; continuous x) | `geom_smooth()` |

### Numerical + categorical

- **Density plots:** one per category
  - use facets, or
  - use color and transparency (`alpha`), or
  - use ridges
- **Histograms:** different colors with `alpha`
  - or use facets
- **Box plots:** one per category
- **Violin plots:** one per category

### Multivariate (3+ variables)

Any mix of 3 categorical, 3 numerical, or a combination.

- **3 categorical:** use facets
