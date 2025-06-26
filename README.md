#  Introduction to ANOVA (Analysis of Variance): Beginner-Friendly Notes with Analogies & Examples

---

## 1. What is ANOVA?

**ANOVA** (Analysis of Variance) is a statistical test used to determine whether **there are significant differences between the means of three or more independent groups**.

### Real-World Analogy:

Imagine you're judging a cooking competition with **three chefs**. You taste each dish and want to know: "Is **at least one chef** significantly better than the others?"

That's what ANOVA helps you figure out — **do the group means differ enough** that it's unlikely to be due to chance?

---

## 2. When to Use ANOVA?

Use **ANOVA** when:

* You have **3 or more independent groups**
* You want to compare their **means**
* Data is **approximately normally distributed**
* Variances are **roughly equal** (homogeneity of variance)

### Examples:

* Comparing **test scores** across 3 different schools
* Testing **customer satisfaction** across 4 store locations
* Measuring **plant growth** under 5 different fertilizers

---

## 3. Types of ANOVA

### A. One-Way ANOVA

* **Used when**: You have **one independent variable** with **3 or more groups**.
* **Goal**: To determine if the **group means** are significantly different.

### Example Analogy:

You're testing **3 brands of fertilizer** to see which one grows the tallest plants. There's **one factor (fertilizer type)** and **one outcome (plant height)**.

### B. Two-Way ANOVA

* **Used when**: You have **two independent variables**.
* **Goal**: To analyze both the **main effects** and any **interaction** effects.

### Example Analogy:

You're testing how **fertilizer type** and **sunlight hours** affect plant growth. Here, you're checking:

* Does fertilizer type matter?
* Does sunlight amount matter?
* Do fertilizer and sunlight interact in some way?

---

## 4. The Hypotheses

### Null Hypothesis (H0):

All group means are **equal** (no real difference)

### Alternative Hypothesis (H1):

At least **one group mean** is **different**

---

## 5. ANOVA Formula

The ANOVA F-statistic is:

```math
F = \frac{\text{Variance Between Groups}}{\text{Variance Within Groups}}
```

If the **between-group variance** is much larger than the **within-group variance**, we reject the null hypothesis.

---

## 6. One-Way ANOVA Examples (5)

### Example 1: Comparing Exam Scores Across Teachers

```python
from scipy.stats import f_oneway

teacher_A = [85, 88, 90, 87, 86]
teacher_B = [78, 82, 85, 80, 81]
teacher_C = [92, 94, 96, 91, 93]

f_stat, p_value = f_oneway(teacher_A, teacher_B, teacher_C)
print("F-statistic:", f_stat)
print("p-value:", p_value)
```

### Example 2: Fertilizer Type and Plant Growth

```python
fertilizer_1 = [10, 12, 11, 13, 14]
fertilizer_2 = [15, 16, 14, 17, 16]
fertilizer_3 = [20, 22, 21, 23, 24]

f_stat, p_value = f_oneway(fertilizer_1, fertilizer_2, fertilizer_3)
print("F-statistic:", f_stat)
print("p-value:", p_value)
```

### Example 3: Comparing Product Ratings

```python
product_A = [4.2, 4.3, 4.0, 4.5, 4.1]
product_B = [3.8, 3.7, 3.9, 4.0, 3.6]
product_C = [4.8, 4.7, 4.9, 5.0, 4.6]

f_stat, p_value = f_oneway(product_A, product_B, product_C)
print("F-statistic:", f_stat)
print("p-value:", p_value)
```

### Example 4: Training Methods on Performance

```python
method_A = [75, 80, 78, 76, 77]
method_B = [85, 88, 84, 86, 87]
method_C = [65, 67, 64, 66, 63]

f_stat, p_value = f_oneway(method_A, method_B, method_C)
print("F-statistic:", f_stat)
print("p-value:", p_value)
```

### Example 5: Store Locations vs Sales

```python
store_1 = [150, 160, 158, 155, 162]
store_2 = [180, 185, 183, 182, 181]
store_3 = [130, 135, 132, 134, 133]

f_stat, p_value = f_oneway(store_1, store_2, store_3)
print("F-statistic:", f_stat)
print("p-value:", p_value)
```

---

## 7. Two-Way ANOVA Examples (5)

We'll use `statsmodels` for two-way ANOVA:

```python
import pandas as pd
import statsmodels.api as sm
from statsmodels.formula.api import ols
```

### Example 1: Gender and Method on Scores

```python
data = pd.DataFrame({
    'score': [88, 90, 85, 78, 80, 75, 92, 94, 91, 84],
    'gender': ['M', 'M', 'M', 'F', 'F', 'F', 'M', 'M', 'M', 'F'],
    'method': ['A', 'A', 'B', 'A', 'A', 'B', 'B', 'B', 'A', 'B']
})

model = ols('score ~ C(gender) + C(method) + C(gender):C(method)', data=data).fit()
anova_table = sm.stats.anova_lm(model, typ=2)
print(anova_table)
```

### Example 2: Diet and Exercise on Weight Loss

```python
data = pd.read_csv('https://raw.githubusercontent.com/selva86/datasets/master/ToothGrowth.csv')
data['supp'] = data['supp'].astype('category')
data['dose'] = data['dose'].astype('category')

model = ols('len ~ C(supp) + C(dose) + C(supp):C(dose)', data=data).fit()
sm.stats.anova_lm(model, typ=2)
```

### Example 3: Teaching Style & Class Type on Scores

```python
data = pd.DataFrame({
    'score': [85, 88, 90, 87, 86, 82, 80, 84, 83, 81],
    'style': ['Lecture', 'Lecture', 'Interactive', 'Interactive', 'Lecture',
              'Lecture', 'Interactive', 'Interactive', 'Lecture', 'Interactive'],
    'class_type': ['Morning', 'Morning', 'Morning', 'Morning', 'Evening',
                   'Evening', 'Evening', 'Evening', 'Evening', 'Morning']
})

model = ols('score ~ C(style) + C(class_type) + C(style):C(class_type)', data=data).fit()
sm.stats.anova_lm(model, typ=2)
```

### Example 4: Product Category & Store Region on Sales

```python
data = pd.DataFrame({
    'sales': [210, 220, 215, 180, 190, 185, 250, 255, 260, 230],
    'category': ['A', 'A', 'A', 'B', 'B', 'B', 'C', 'C', 'C', 'A'],
    'region': ['North', 'South', 'North', 'South', 'North', 'South', 'North', 'South', 'North', 'South']
})

model = ols('sales ~ C(category) + C(region) + C(category):C(region)', data=data).fit()
sm.stats.anova_lm(model, typ=2)
```

### Example 5: Training Type and Gender on Exam Scores

```python
data = pd.DataFrame({
    'score': [70, 75, 72, 68, 74, 80, 85, 78, 77, 83],
    'training': ['X', 'X', 'Y', 'Y', 'X', 'X', 'Y', 'Y', 'X', 'Y'],
    'gender': ['M', 'F', 'M', 'F', 'F', 'M', 'F', 'M', 'F', 'M']
})

model = ols('score ~ C(training) + C(gender) + C(training):C(gender)', data=data).fit()
sm.stats.anova_lm(model, typ=2)
```

---

## 8. How to Interpret Results

* Look at the **p-values** for each factor and their interaction.
* **p < 0.05** means a statistically significant effect.

---

## 9. Final Analogy Recap

> One-Way ANOVA is like comparing **3 different diets** to see which one works best.
>
> Two-Way ANOVA is like comparing **3 diets** *and* **2 exercise routines** — to see both their **individual effects** and whether they **work better together**.


