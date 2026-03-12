![logo_ironhack_blue 7](https://user-images.githubusercontent.com/23629340/40541063-a07a0a8a-601a-11e8-91b5-2f13e4e6b441.png)

# Lab | t-Tests and Assumption Checks

## Overview

The t-test is one of the most widely used tools in data analysis. Whether you are checking if a manufacturing process hits a target value, comparing conversion rates between two website designs, or measuring the effect of a training programme on the same group of employees, some flavour of the t-test is usually your first stop.

But a t-test is only as trustworthy as its assumptions. If the data are heavily skewed, if variances differ wildly between groups, or if the sample is tiny, the p-value you get back may be misleading. That is why seasoned analysts always pair their tests with diagnostic checks — and have a backup plan (like a permutation test) when the assumptions fail.

In this lab you will run all three major t-test variants on realistic data, systematically check assumptions with visual and formal tests, implement a permutation test to see how results compare, and write up structured decision statements that clearly communicate your findings. By the end, you will have a repeatable workflow you can apply to any hypothesis-testing scenario.

## Learning Goals

By the end of this lab, you should be able to:

- Perform a one-sample t-test and interpret the result in context
- Perform an independent two-sample t-test (with and without equal-variance assumption)
- Perform a paired t-test for before/after or matched designs
- Assess normality using Q-Q plots and the Shapiro-Wilk test
- Implement a permutation test and compare its p-value with the classical t-test
- Write a clear, structured statistical conclusion (test used → statistic → p-value → decision → plain-language interpretation)

## Setup and Context

You will work in a single Jupyter notebook. The lab provides three separate hypothesis-testing scenarios — one for each t-test flavour. For each scenario you will state hypotheses, check assumptions, run the test, and interpret the outcome. All code, plots, and written conclusions belong in the notebook.

### Prerequisites

- Understanding of null and alternative hypotheses, p-values, and significance levels (covered in the lesson)
- Familiarity with `scipy.stats` basics
- Comfortable creating plots with matplotlib / seaborn

## Requirements

1. **Fork** this repo to your GitHub account, then **clone** your fork locally.
2. Make sure you have the following Python packages installed:

```bash
pip install numpy pandas scipy matplotlib seaborn
```

3. Work inside the notebook specified in **Getting Started**.

## Getting Started

1. Create a new Jupyter notebook called **`m3-06-t-tests-assumption-checks.ipynb`** in the root of your cloned repo.
2. At the top of the notebook, import the libraries you will need:

```python
import numpy as np
import pandas as pd
import scipy.stats as st
import matplotlib.pyplot as plt
import seaborn as sns

np.random.seed(42)
```

3. Work through the tasks below in order. Give each task its own Markdown heading in the notebook.

## Tasks

### Task 1: One-Sample t-Test

**Scenario:** A coffee-shop chain claims that the average wait time at their stores is 4.0 minutes. You have collected wait times (in minutes) from a random sample of 35 visits.

1. Generate (or load) the sample data. If generating, use something like:

```python
wait_times = np.random.normal(loc=4.3, scale=1.2, size=35)
```

2. State the null and alternative hypotheses in a Markdown cell.
3. Run `st.ttest_1samp(wait_times, popmean=4.0)`.
4. Report the t-statistic and p-value.
5. At α = 0.05, state whether you reject or fail to reject H₀.
6. Write a one-sentence plain-language interpretation (e.g. "There is / is not sufficient evidence that the average wait time differs from 4 minutes.").

### Task 2: Independent Two-Sample t-Test

**Scenario:** An e-commerce company ran an A/B test on its checkout page. Group A (control, n = 50) saw the old design; Group B (treatment, n = 50) saw a streamlined design. The metric is order value in euros.

1. Generate (or load) data for both groups. Example:

```python
group_a = np.random.normal(loc=52, scale=12, size=50)
group_b = np.random.normal(loc=57, scale=14, size=50)
```

2. State hypotheses (two-tailed: means are equal vs. not equal).
3. Before running the test, check the **equal-variance assumption**:
   - Compute the variance of each group.
   - Run Levene's test (`st.levene`). Report the result.
4. Based on the Levene result, choose the appropriate test:
   - Equal variances → `st.ttest_ind(a, b, equal_var=True)`
   - Unequal variances → `st.ttest_ind(a, b, equal_var=False)` (Welch's t-test)
5. Report the t-statistic and p-value.
6. State your decision and a plain-language interpretation.

### Task 3: Paired t-Test

**Scenario:** A company measures employee productivity scores before and after a new workflow tool is introduced. The same 30 employees are measured at both time points.

1. Generate (or load) paired data:

```python
before = np.random.normal(loc=70, scale=8, size=30)
after = before + np.random.normal(loc=3, scale=5, size=30)
```

2. State hypotheses (one-tailed or two-tailed — justify your choice in a Markdown cell).
3. Compute the differences (`after - before`) and briefly inspect them (histogram or box plot).
4. Run `st.ttest_rel(after, before)`.
5. Report the t-statistic and p-value.
6. State your decision and interpretation.

## Submission

### What to submit

- `m3-06-t-tests-assumption-checks.ipynb` — your completed Jupyter notebook containing all code, diagnostic plots, and written answers.

### Definition of done (checklist)

- [ ] One-sample, two-sample, and paired t-tests correctly implemented and interpreted
- [ ] Levene's test used to decide between equal-variance and Welch's t-test
- [ ] Each task includes hypotheses, test result, and plain-language interpretation
- [ ] Notebook runs top-to-bottom without errors (`Kernel → Restart & Run All`)

### How to submit (Git workflow)

```bash
git add .
git commit -m "lab: t-tests and assumption checks"
git push origin main
```

Then open a **Pull Request** from your fork back to the original repo. Paste the PR link in the student portal.
