# Comp Notes

## Table of Contents

- [Pro Tips](#pro-tips)
- [Basics of Data Viz](#basics-of-data-viz)
- [Choosing a Plot](#choosing-a-plot)
- [The Pipe Operator](#the-pipe-operator)
- [Legends and Accessibility](#legends-and-accessibility)
- [Wrangling Verbs](#wrangling-verbs)

---

## Pro Tips

### Package install failed? Change the CRAN server

**Tools → Global Options → Packages → Change** (Primary CRAN repository)

> [!TIP]
> University of Michigan has been a reliable mirror for me.

---

## Basics of Data Viz

| Term | Meaning |
|------|---------|
| Frame | variables that define the axes |
| Layer | geometric elements we add to the canvas |
| Scales | aesthetics (i.e. color, size, shape) |
| Faceting | splitting data into subplots |
| Theme | additional aesthetic control (font type, background, color scheme) |

observations = # rows

store value: name <- output

| Function | What does it do? |
|----------|------------------|
| dim(dataset) | dimensions of dataset |
| nrow() | number of rows in dataset |
| head() | gives first 6 rows of dataset |
| head(dataset, 3) | gives first 3 rows of dataset |
| tail() | gives last 6 rows |
| names() | gives names of columns |
| str() | gives class types and summary of data parameters & dimensions |

<details>
<summary><b>Class Types</b></summary>

- num / numeric
- int / integer
- chr / character
- factor
- data.frame

</details>

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
- **Histograms:** different opacity with `alpha`
  - or use facets
- **Box plots:** one per category
- **Violin plots:** one per category

### Multivariate (3+ variables)

Any mix of 3 categorical, 3 numerical, or a combination.

- **3 categorical:** use facets

---

## The Pipe Operator

### `|>`: R's native pipe

The pipe passes data from one step to the next, like a Mario pipe.

Object on left is *passed to* the **function** on the right:

(xxx) |> function 
equivalent to:
function(xxx)

**Example:**

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

<details>
<summary><b>Explanation on Temporary Objects</b></summary>

You can *also* store data in temporary objects, but this is tedious and increases risk of typos. It also nests the data into a particular environment, which is ineffective for rendering.
Temporary objects can be useful for cleaning data, but once complete it is likely smarter to save the cleaned data onto your desktop for future use.

</details>

## Legends and Accessibility

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

Add alt text so screen readers can describe your plots for the visually impaired:

```r

#| fig-alt: "text here"

```
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

### 6 Key Wrangling Verbs (action word functions from tidyverse's dplyr package)

Wrangling verbs act on a data frame and yield a different data frame.

| Verb | What it does | How to use it |
|------|--------------|---------------|
| `mutate()` | Creates a new column given context of existing data | ds |> mutate(c2=c2*2, c7=2*5) |
| `group_by()` | Groups *rows* by particular *column(s)*. Splits the data by a **categorical** variable | ds |> group_by(c4) |
| `filter()` | Keeps only the **rows** that meet a condition | ds |> select(c4=='a') |
| `select()` | Keeps only specific **columns** (variables) | ds |> select(c2, c5) |
| `arrange()` | **Sorts** rows by specific *column(s)* | ds |> arrange(desc(c2)) |
| `summarize()`| Generates a numerical **summary** of a *column* | ds |> summarize(mean(c5)) |

### `filter()` + `summarize()`

**Filter** first, then **summarize** the result. Example: the fastest 14-year-old:

```r
running %>%
  filter(age == 14) %>%
  summarize(fastest = min(run_time))
```

But if we are subsetting data from, say, 2 million x 26 --> 10 x 10, then use **`select()`**, then **summarize**
