# DU Non-Parametric Methods — Practical Record & Worked Examples

**Topics covered**

1. Test for Randomness — Wald–Wolfowitz Runs Test
2. Sign Test
3. Wilcoxon Signed-Rank Test
4. Kolmogorov–Smirnov One-Sample Test
5. Kolmogorov–Smirnov Two-Sample Test
6. Mann–Whitney U Test
7. Wilcoxon Rank-Sum Test
8. Kruskal–Wallis One-Way ANOVA by Ranks
9. Spearman's Coefficient of Rank Correlation

> **How to use this file:** Each practical has an aim, objectives,
> theory, hypotheses, formulae, procedure, and two worked illustrations:
> a small sample and a larger sample. The examples are for learning and
> can be copied into a practical record after adapting the wording to
> your teacher's format.

---

## General notes before starting

- Choose the significance level before testing. The examples generally
  use the **5% significance level**.
- "Do not reject $H_0$" is the correct statistical wording; it does
  not prove that $H_0$ is true.
- For rank tests, assign **average ranks** when values are tied.
- For small samples, use exact distributions or the appropriate
  critical-value table wherever available. Large-sample normal or
  chi-square approximations are not always reliable for very small
  samples.
- A test can detect a difference in distributions. Interpreting it
  specifically as a difference in medians requires additional
  assumptions, such as similarly shaped distributions.

---

# 1. Test for Randomness — Wald–Wolfowitz Runs Test

## Aim

To test whether a given sequence of observations is random using the
Wald–Wolfowitz Runs Test.

## Objectives

- To understand the meaning of a run in a sequence.
- To determine whether observations occur in a random order.
- To identify possible patterns such as clustering or alternation.

## Theory

A **run** is a consecutive sequence of identical symbols that is
preceded and followed by a different symbol or by the beginning/end of
the sequence. For example,

```
+++ | -- | + | --- | ++
```

contains five runs.

For a one-sample randomness test, observations are commonly classified
as above or below the median. Values equal to the median are handled
according to the convention specified by the course/software; a common
approach is to omit median ties. The resulting sequence is examined for
its number of runs.

Let:

- $n_1$ = number of observations in category 1
- $n_2$ = number of observations in category 2
- $R$ = observed number of runs
- $N = n_1 + n_2$

Expected number of runs:

$$
E(R) = \frac{2n_1n_2}{N} + 1
$$

Variance:

$$
\operatorname{Var}(R) = \frac{2n_1n_2\,(2n_1n_2 - n_1 - n_2)}{N^2(N-1)}
$$

For a sufficiently large sample:

$$
Z = \frac{R - E(R)}{\sqrt{\operatorname{Var}(R)}}
$$

## Hypotheses

- $H_0$: The sequence is randomly ordered.
- $H_1$: The sequence is not randomly ordered.

## Procedure

1. Arrange the observations in their original order.
2. Find the median and classify observations as above (+) or below (−)
   the median.
3. Apply the chosen rule to observations equal to the median.
4. Count $n_1$, $n_2$, and the observed number of runs $R$.
5. For a large sample, calculate the expected runs, variance, and $Z$.
6. Compare with the appropriate critical value or use the p-value.

## Worked example A — Small sample (exact runs distribution)

Sequence:

```
+  +  -  -  +  -  -  +
```

There are four positive signs and four negative signs:

$$
n_1 = 4, \quad n_2 = 4, \quad N = 8
$$

Runs:

```
++ | -- | + | -- | +
```

Therefore:

$$
R = 5
$$

For a small sample, use the **exact runs distribution table** for
$n_1 = 4,\ n_2 = 4$, rather than relying on the large-sample normal
approximation. Here $E(R) = \frac{2(4)(4)}{8} + 1 = 5$, so the observed
number of runs is exactly what is expected under randomness. From the
exact distribution, $P(R \le 5) = P(R \ge 5) = 44/70 \approx 0.629$, so
the two-sided p-value is $1$ (capped at 1).

At the 5% level, $p > 0.05$, so **do not reject $H_0$**. There is
insufficient evidence that the sequence is non-random. (For
$n_1 = n_2 = 4$, only $R = 2$ or $R = 8$ would be significant at 5%.)

## Worked example B — Larger sample (normal approximation)

Suppose a sequence contains $n_1 = 50$ plus signs and $n_2 = 50$ minus
signs, and the observed number of runs is $R = 35$.

$$
N = 100
$$

$$
E(R) = \frac{2(50)(50)}{100} + 1 = 51
$$

$$
\operatorname{Var}(R) = \frac{2(50)(50)\,(5000 - 100)}{100^2\,(99)} \approx 24.747
$$

$$
Z = \frac{35 - 51}{\sqrt{24.747}} \approx -3.22
$$

At 5% two-tailed significance, the critical values are approximately
$\pm 1.96$. Since $|Z| = 3.22 > 1.96$, **reject $H_0$**. The
sequence shows evidence of non-randomness (too few runs, suggesting
clustering).

## Decision rule

For a two-tailed test at 5%:

- If $|Z| \le 1.96$, do not reject $H_0$.
- If $|Z| > 1.96$, reject $H_0$.

For small samples, use the exact runs table/p-value. Some references use
a continuity correction for the normal approximation.

---

# 2. Sign Test

## Aim

To apply the Sign Test to determine whether a population median equals a
specified value or whether two paired conditions differ.

## Objectives

- To understand a non-parametric test based only on positive and
  negative signs.
- To test a population median or compare paired observations.
- To analyse data without requiring a normal distribution.

## Theory

The **Sign Test** is a non-parametric test. For a one-sample test,
compare each observation $X_i$ with the hypothesised median $M_0$:

- Write (+) if $X_i > M_0$.
- Write (−) if $X_i < M_0$.
- Ignore observations equal to $M_0$.

For paired data, calculate each pair's difference and use its sign.
Under $H_0$, positive and negative signs are equally likely:

$$
P(+) = P(-) = 0.5
$$

If $n$ is the number of non-zero differences, the number of positive
signs follows:

$$
X \sim \operatorname{Binomial}(n,\ 0.5)
$$

For a large sample, a normal approximation may be used:

$$
\mu = \frac{n}{2}, \qquad \sigma = \sqrt{\frac{n}{4}}
$$

With continuity correction, a common two-sided calculation is:

$$
Z = \frac{|X - n/2| - 0.5}{\sqrt{n/4}}
$$

## Hypotheses

For a one-sample test:

- $H_0$: Population median $= M_0$.
- $H_1$: Population median $\neq M_0$.

For paired data:

- $H_0$: Median paired difference $= 0$.
- $H_1$: Median paired difference $\neq 0$.

## Procedure

1. State the hypothesised median or calculate paired differences.
2. Assign +, −, or zero.
3. Omit zero differences.
4. Count positive and negative signs.
5. Use the exact binomial distribution for small samples.
6. For large samples, use the normal approximation with continuity
   correction.
7. Compare the p-value with $\alpha$.

## Worked example A — Small sample (exact binomial test)

A sample of 10 observations is compared with a hypothesised median.
Eight observations are above it and two are below it.

$$
n = 10, \quad X = 8
$$

For a two-sided test, the exact p-value is:

$$
p = 2\,P(X \le 2)
$$

$$
p = 2\left[\frac{\binom{10}{0} + \binom{10}{1} + \binom{10}{2}}{2^{10}}\right]
$$

$$
p = 2\left(\frac{1 + 10 + 45}{1024}\right) = 0.1094
$$

Since $0.1094 > 0.05$, **do not reject $H_0$**. The sample does not
provide sufficient evidence that the population median differs from the
hypothesised value.

## Worked example B — Larger sample (normal approximation)

Suppose 30 non-zero paired differences give 23 positive signs and 7
negative signs.

$$
n = 30, \quad X = 23
$$

$$
\mu = 15, \qquad \sigma = \sqrt{7.5} \approx 2.739
$$

With continuity correction:

$$
Z = \frac{23 - 15 - 0.5}{2.739} \approx 2.74
$$

Since $2.74 > 1.96$, **reject $H_0$** at the 5% two-sided level. There is
evidence of a systematic difference in the positive direction.

## Decision rule

Reject $H_0$ when the p-value is less than or equal to the significance
level. For a large-sample two-sided test at 5%, compare $|Z|$ with
1.96.

---

# 3. Wilcoxon Signed-Rank Test

## Aim

To determine whether there is a significant difference between two
related or paired samples using the Wilcoxon Signed-Rank Test.

## Objectives

- To compare two related measurements.
- To rank the absolute paired differences.
- To test whether the distribution of paired differences is centred at
  zero.

## Theory

The **Wilcoxon Signed-Rank Test** is a non-parametric alternative to the
paired-samples $t$-test. It uses both the **magnitude (through ranks)**
and the **sign** of paired differences.

For each pair:

$$
d_i = X_i - Y_i
$$

Ignore zero differences. Rank the absolute differences $|d_i|$ from
smallest to largest. Give tied absolute differences their average rank,
then restore the original + or − sign.

Let:

- $W^+$ = sum of positive ranks
- $W^-$ = sum of negative ranks

For the traditional small-sample table, the test statistic is often:

$$
T = \min(W^+,\ W^-)
$$

For large samples, a normal approximation can be used. With no ties in
absolute differences:

$$
\mu_W = \frac{n(n+1)}{4}
$$

$$
\sigma_W = \sqrt{\frac{n(n+1)(2n+1)}{24}}
$$

A continuity correction may be applied. Ties require a variance
adjustment.

## Hypotheses

- $H_0$: The median paired difference is zero (under the usual
  symmetry assumption, the difference distribution is centred at
  zero).
- $H_1$: The median paired difference is not zero.

## Procedure

1. Calculate paired differences.
2. Remove zero differences.
3. Rank the absolute differences.
4. Restore the signs to the ranks.
5. Calculate $W^+$ and $W^-$.
6. Use an exact Wilcoxon table for small samples or a normal
   approximation for large samples.
7. Make the decision using the p-value or critical value.

## Worked example A — Small sample

Six paired differences are:

$$
+1,\ +2,\ +3,\ +4,\ +5,\ -6
$$

| Difference $d$ | $\lvert d \rvert$ | Rank of $\lvert d \rvert$ | Signed rank |
|---:|---:|---:|---:|
| +1 | 1 | 1 | +1 |
| +2 | 2 | 2 | +2 |
| +3 | 3 | 3 | +3 |
| +4 | 4 | 4 | +4 |
| +5 | 5 | 5 | +5 |
| −6 | 6 | 6 | −6 |

$$
W^+ = 1 + 2 + 3 + 4 + 5 = 15
$$

$$
W^- = 6
$$

$$
T = \min(15,\ 6) = 6
$$

For $n = 6$, the exact one-sided probability is
$P(T \le 6) = 14/64 \approx 0.219$, so the exact two-sided p-value is
approximately **0.438**. Since $p > 0.05$, **do not reject $H_0$**. The
result is not statistically significant at the 5% level. (For $n = 6$,
only $T = 0$ is significant at the 5% two-sided level.)

## Worked example B — Larger sample (normal approximation)

Suppose there are 20 non-zero paired differences. Their absolute values
have ranks 1 through 20, with the two largest ranks (19 and 20) negative
and all other ranks positive.

$$
W^+ = 1 + 2 + \cdots + 18 = 171
$$

$$
W^- = 19 + 20 = 39
$$

$$
T = 39
$$

For $n = 20$:

$$
\mu_W = \frac{20(21)}{4} = 105
$$

$$
\sigma_W = \sqrt{\frac{20(21)(41)}{24}} \approx 26.786
$$

Using continuity correction:

$$
Z = \frac{|39 - 105| - 0.5}{26.786} \approx 2.45
$$

Since $2.45 > 1.96$, **reject $H_0$** at the 5% two-sided level. There is
evidence of a non-zero paired difference.

## Important note

The Signed-Rank Test is for **paired/dependent samples**. It is not the
same as the Wilcoxon Rank-Sum Test, which is for independent samples.

---

# 4. Kolmogorov–Smirnov One-Sample Test

## Aim

To determine whether a sample comes from a specified continuous
theoretical probability distribution using the one-sample
Kolmogorov–Smirnov test.

## Objectives

- To compare an empirical distribution with a theoretical
  distribution.
- To measure the maximum difference between the two cumulative
  distribution functions.
- To test whether the sample is consistent with a specified
  distribution.

## Theory

The **one-sample Kolmogorov–Smirnov (K–S) test** compares the
empirical cumulative distribution function (ECDF) of a sample with a
specified theoretical cumulative distribution function (CDF).

For ordered observations $x_{(1)}, \ldots, x_{(n)}$, let $F(x)$
be the specified theoretical CDF. Define:

$$
D^+ = \max_i \left[\frac{i}{n} - F(x_{(i)})\right]
$$

$$
D^- = \max_i \left[F(x_{(i)}) - \frac{i-1}{n}\right]
$$

The test statistic is:

$$
D = \max(D^+,\ D^-)
$$

## Hypotheses

- $H_0$: The sample follows the specified theoretical distribution.
- $H_1$: The sample does not follow the specified theoretical
  distribution.

## Procedure

1. Specify the continuous theoretical distribution under $H_0$.
2. Arrange the sample observations in ascending order.
3. Calculate the theoretical cumulative probability $F(x_{(i)})$ for each
   observation.
4. Calculate $D^+$ and $D^-$.
5. Take the larger value as $D$.
6. Compare with the appropriate critical value or obtain the p-value.

## Worked example A — Small sample

Test whether the following five observations are consistent with a
Uniform(0, 1) distribution:

$$
0.10,\ 0.25,\ 0.40,\ 0.70,\ 0.90
$$

For Uniform(0, 1), $F(x) = x$.

| $i$ | $x_{(i)}$ | $i/n$ | $(i-1)/n$ | $F(x_{(i)})$ | $i/n - F(x_{(i)})$ | $F(x_{(i)}) - (i-1)/n$ |
|---:|---:|---:|---:|---:|---:|---:|
| 1 | 0.10 | 0.20 | 0.00 | 0.10 | 0.10 | 0.10 |
| 2 | 0.25 | 0.40 | 0.20 | 0.25 | 0.15 | 0.05 |
| 3 | 0.40 | 0.60 | 0.40 | 0.40 | 0.20 | 0.00 |
| 4 | 0.70 | 0.80 | 0.60 | 0.70 | 0.10 | 0.10 |
| 5 | 0.90 | 1.00 | 0.80 | 0.90 | 0.10 | 0.10 |

$$
D^+ = 0.20, \qquad D^- = 0.10
$$

$$
D = 0.20
$$

For $n = 5$, the 5% exact critical value is approximately 0.565. Since
$0.20 < 0.565$, **do not reject $H_0$**. The sample is not significantly
different from Uniform(0, 1).

## Worked example B — Larger sample

Suppose $n = 40$ observations are tested against Uniform(0, 1). Assume 20
observations are between 0.01 and 0.20, and the remaining 20 are between
0.71 and 0.90.

At $x = 0.20$:

$$
F_n(0.20) = \frac{20}{40} = 0.50
$$

$$
F(0.20) = 0.20
$$

Therefore, the difference at this point is:

$$
|0.50 - 0.20| = 0.30
$$

Thus $D \ge 0.30$. The 5% large-sample approximate critical value
is:

$$
D_{\text{critical}} = \frac{1.36}{\sqrt{40}} \approx 0.215
$$

Since $D \ge 0.30 > 0.215$, **reject $H_0$**. The observations are
inconsistent with the specified Uniform(0, 1) distribution.

## Important notes

- The usual one-sample K–S critical values assume the theoretical
  distribution is fully specified in advance.
- If parameters such as the normal mean and standard deviation are
  estimated from the same sample, the ordinary K–S critical values
  are not valid; a correction such as the Lilliefors test or a
  suitable simulation method is needed.
- The classical K–S test is designed for continuous distributions.

---

# 5. Kolmogorov–Smirnov Two-Sample Test

## Aim

To determine whether two independent samples come from the same
continuous probability distribution.

## Objectives

- To compare the empirical distributions of two independent samples.
- To calculate the maximum difference between their ECDFs.
- To test whether the two population distributions differ.

## Theory

The **two-sample K–S test** compares the empirical cumulative
distribution functions of two independent samples.

Let $F_{1,n_1}(x)$ and $F_{2,n_2}(x)$ be the empirical CDFs of samples of
sizes $n_1$ and $n_2$.

The test statistic is:

$$
D = \sup_x \left| F_{1,n_1}(x) - F_{2,n_2}(x) \right|
$$

It is the largest vertical distance between the two empirical CDFs.

## Hypotheses

- $H_0$: Both samples come from the same continuous distribution.
- $H_1$: The two population distributions differ.

## Procedure

1. Combine and sort the distinct observations from both samples.
2. At each distinct value, calculate the cumulative proportion in
   sample 1 and sample 2.
3. Find the absolute difference between the two cumulative proportions.
4. The largest difference is $D$.
5. Compare with the critical value or use the p-value.

## Worked example A — Small sample

Sample 1:

$$
1,\ 2,\ 3,\ 4,\ 5
$$

Sample 2:

$$
6,\ 7,\ 8,\ 9,\ 10
$$

The two samples do not overlap. After the first sample's five
observations, its ECDF is 1 while the second sample's ECDF is 0.
Therefore:

$$
D = 1.00
$$

For $n_1 = n_2 = 5$, $D = 1$ is the most extreme value possible. It
occurs only when one sample lies entirely below the other, so the exact
two-sided p-value is $2 / \binom{10}{5} = 2/252 \approx 0.008$. Since
$0.008 < 0.05$, **reject $H_0$**. There is evidence that the two
distributions differ.

## Worked example B — Larger sample

Let sample 1 contain the integers 1 through 40, and sample 2 contain the
integers 21 through 60.

At $x = 20$:

$$
F_1(20) = \frac{20}{40} = 0.50
$$

$$
F_2(20) = 0
$$

Therefore:

$$
D \ge 0.50
$$

For $n_1 = n_2 = 40$, the approximate 5% critical value is:

$$
D_{\text{critical}} = 1.36\sqrt{\frac{40 + 40}{40(40)}}
$$

$$
D_{\text{critical}} \approx 0.304
$$

Since $D \ge 0.50 > 0.304$, **reject $H_0$**. The two samples show
significantly different distributions.

## Important note

The two-sample K–S test compares the **whole distributions**, not only
their means or medians. The standard test assumes independent samples
and continuous distributions.

---

# 6. Mann–Whitney U Test

## Aim

To determine whether two independent samples differ significantly using
the Mann–Whitney U Test.

## Objectives

- To compare two independent groups using ranks.
- To apply a non-parametric alternative to the independent-samples
  $t$-test.
- To determine whether observations in one group tend to be higher or
  lower than those in the other group.

## Theory

The **Mann–Whitney U test** is a non-parametric test for comparing two
independent samples. Combine the observations, rank them from smallest
to largest, and calculate the sum of ranks for each group.

Let:

- $n_1, n_2$ = sample sizes
- $R_1, R_2$ = rank sums

The U statistics are:

$$
U_1 = R_1 - \frac{n_1(n_1+1)}{2}
$$

$$
U_2 = R_2 - \frac{n_2(n_2+1)}{2}
$$

$$
U = \min(U_1,\ U_2)
$$

For large samples, a normal approximation may be used. Without ties:

$$
\mu_U = \frac{n_1 n_2}{2}
$$

$$
\sigma_U = \sqrt{\frac{n_1 n_2 (n_1 + n_2 + 1)}{12}}
$$

A continuity correction and tie adjustment may be needed.

## Hypotheses

- $H_0$: The two population distributions are the same.
- $H_1$: The two population distributions differ.

If the two distributions have the same shape and spread, the test is
often interpreted as a test of location/median difference.

## Procedure

1. Take two independent samples.
2. Combine all observations.
3. Rank them in ascending order; use average ranks for ties.
4. Calculate each group's rank sum.
5. Calculate $U_1$, $U_2$, and the smaller U.
6. Use an exact table for small samples or a normal approximation for
   large samples.
7. Make the decision.

## Worked example A — Small sample

- Sample A: 4, 5, 6, 7
- Sample B: 1, 2, 3, 8

| Value | Group | Combined rank |
|---:|:---:|---:|
| 1 | B | 1 |
| 2 | B | 2 |
| 3 | B | 3 |
| 4 | A | 4 |
| 5 | A | 5 |
| 6 | A | 6 |
| 7 | A | 7 |
| 8 | B | 8 |

$$
R_A = 4 + 5 + 6 + 7 = 22
$$

$$
R_B = 1 + 2 + 3 + 8 = 14
$$

With $n_A = n_B = 4$:

$$
U_A = 22 - \frac{4(5)}{2} = 12
$$

$$
U_B = 14 - \frac{4(5)}{2} = 4
$$

$$
U = \min(12,\ 4) = 4
$$

For $n_1 = n_2 = 4$, the exact two-sided p-value is approximately 0.343.
Since $p > 0.05$, **do not reject $H_0$**.

## Worked example B — Larger sample (normal approximation)

Sample A has values 21 through 40; sample B has values 1 through 20.
Thus every A observation is greater than every B observation.

$$
n_1 = n_2 = 20
$$

The U statistic for the lower-ranked group is:

$$
U = 0
$$

$$
\mu_U = \frac{20(20)}{2} = 200
$$

$$
\sigma_U = \sqrt{\frac{20(20)(41)}{12}} \approx 36.97
$$

Using continuity correction:

$$
Z = \frac{|0 - 200| - 0.5}{36.97} \approx 5.40
$$

Since $5.40 > 1.96$, **reject $H_0$**. The samples differ significantly.

---

# 7. Wilcoxon Rank-Sum Test

## Aim

To compare two independent samples using the Wilcoxon Rank-Sum Test.

## Objectives

- To understand rank-sum testing for independent samples.
- To calculate and compare the sums of ranks.
- To determine whether two independent population distributions
  differ.

## Theory

The **Wilcoxon Rank-Sum Test** is a non-parametric test for two
independent samples. It is mathematically equivalent to the
Mann–Whitney U test; the difference is mainly the way the statistic is
reported.

Combine both samples, rank all observations, and calculate the rank sum
for each sample.

Let $R_1$ be the rank sum for sample 1. The corresponding U statistic
is:

$$
U_1 = R_1 - \frac{n_1(n_1+1)}{2}
$$

Similarly:

$$
U_2 = R_2 - \frac{n_2(n_2+1)}{2}
$$

The relationship is:

$$
U_1 + U_2 = n_1 n_2
$$

## Hypotheses

- $H_0$: The two population distributions are the same.
- $H_1$: The two population distributions differ.

## Procedure

1. Combine the two independent samples.
2. Arrange observations in ascending order.
3. Assign ranks; average ranks for ties.
4. Find the rank sum of each sample.
5. Convert the rank sum to U if required by the critical-value
   table/software.
6. Compare the statistic or p-value with the chosen significance level.

## Worked example A — Small sample

Use the same data as the small Mann–Whitney example:

- Sample A: 4, 5, 6, 7
- Sample B: 1, 2, 3, 8

The rank sums are:

$$
R_A = 22, \qquad R_B = 14
$$

The rank-sum statistic for A is $W_A = 22$. Convert to U:

$$
U_A = 22 - \frac{4(5)}{2} = 12
$$

For B:

$$
U_B = 14 - \frac{4(5)}{2} = 4
$$

The smaller U is 4. The exact two-sided p-value is approximately 0.343.
Since $p > 0.05$, **do not reject $H_0$**.

## Worked example B — Larger sample

- Sample A: values 21 through 40.
- Sample B: values 1 through 20.

All B observations receive ranks 1–20 and all A observations receive
ranks 21–40.

$$
R_A = 21 + 22 + \cdots + 40 = 610
$$

$$
R_B = 1 + 2 + \cdots + 20 = 210
$$

For A:

$$
U_A = 610 - \frac{20(21)}{2} = 400
$$

For B:

$$
U_B = 210 - \frac{20(21)}{2} = 0
$$

Thus $U = 0$. The large-sample normal approximation gives a very large
absolute $Z$ value (about 5.4 with continuity correction), so **reject
$H_0$**.

## Important practical note

**Mann–Whitney U and Wilcoxon Rank-Sum are two forms of the same
test.** If both appear separately in the practical list, explain that
the rank-sum form reports $R$ (or $W$), while the Mann–Whitney form
reports $U$.

---

# 8. Kruskal–Wallis One-Way ANOVA by Ranks

## Aim

To determine whether three or more independent samples differ
significantly using the Kruskal–Wallis test.

## Objectives

- To compare three or more independent groups.
- To rank observations from all groups together.
- To apply a non-parametric alternative to one-way ANOVA.

## Theory

The **Kruskal–Wallis test** is a non-parametric method for comparing
three or more independent groups. It is based on the ranks of all
observations rather than their original values.

Let:

- $k$ = number of groups
- $n_i$ = size of group $i$
- $N = \sum n_i$ = total sample size
- $R_i$ = sum of ranks in group $i$

The test statistic is:

$$
H = \frac{12}{N(N+1)} \sum_{i=1}^{k} \frac{R_i^2}{n_i} - 3(N+1)
$$

When there are no ties, $H$ is approximately distributed as chi-square
with:

$$
df = k - 1
$$

For tied observations, apply the tie correction:

$$
C = 1 - \frac{\sum (t_j^3 - t_j)}{N^3 - N}
$$

$$
H_{\text{corrected}} = \frac{H}{C}
$$

where $t_j$ is the number of observations in the $j$-th tied group.

## Hypotheses

- $H_0$: All group distributions are the same.
- $H_1$: At least one group distribution differs.

## Procedure

1. Take three or more independent samples.
2. Combine all observations and rank them.
3. Assign average ranks to ties.
4. Calculate the rank sum for each group.
5. Substitute the rank sums and group sizes into the $H$ formula.
6. Apply tie correction if needed.
7. Compare $H$ with the chi-square critical value with $k - 1$ degrees of
   freedom.
8. If significant, use a post-hoc test to identify which groups differ.

## Worked example A — Small sample

Three independent groups:

- A: 1, 2, 3, 4
- B: 5, 6, 7, 8
- C: 9, 10, 11, 12

All values are distinct, so assign ranks 1 through 12.

| Group | Ranks | Rank sum $R_i$ | $n_i$ |
|:---:|:---|---:|---:|
| A | 1, 2, 3, 4 | 10 | 4 |
| B | 5, 6, 7, 8 | 26 | 4 |
| C | 9, 10, 11, 12 | 42 | 4 |

$$
N = 12, \quad k = 3
$$

$$
H = \frac{12}{12(13)} \left(\frac{10^2}{4} + \frac{26^2}{4} + \frac{42^2}{4}\right) - 3(13)
$$

$$
H \approx 9.846
$$

Degrees of freedom:

$$
df = k - 1 = 2
$$

At 5% significance, $\chi^2_{0.05,\,2} = 5.991$. Since
$9.846 > 5.991$, **reject $H_0$**. At least one group distribution
differs.

> With only 4 observations per group, the chi-square approximation is
> rough; an exact Kruskal–Wallis table gives the same decision here.

## Worked example B — Larger sample

Three independent groups, each with 10 observations:

- A: 1 through 10
- B: 11 through 20
- C: 21 through 30

The combined ranks are 1 through 30.

$$
N = 30, \quad n_A = n_B = n_C = 10
$$

Rank sums:

$$
R_A = 1 + \cdots + 10 = 55
$$

$$
R_B = 11 + \cdots + 20 = 155
$$

$$
R_C = 21 + \cdots + 30 = 255
$$

$$
H = \frac{12}{30(31)} \left(\frac{55^2}{10} + \frac{155^2}{10} + \frac{255^2}{10}\right) - 3(31)
$$

$$
H \approx 25.806
$$

$$
df = 3 - 1 = 2
$$

Since $25.806 > 5.991$, **reject $H_0$**. There is a significant
difference among the group distributions.

## Important note

A significant Kruskal–Wallis result tells us that **at least one group
differs**. It does not identify the pair(s) responsible. Use an
appropriate post-hoc procedure, such as Dunn's test with a
multiple-comparison correction.

---

# 9. Spearman's Coefficient of Rank Correlation

## Aim

To calculate Spearman's Rank Correlation Coefficient and determine the
strength and direction of association between two variables.

## Objectives

- To measure association using ranks.
- To identify positive, negative, or absent monotonic association.
- To analyse ordinal data or data that do not meet the assumptions of
  Pearson correlation.

## Theory

**Spearman's rank correlation coefficient**, denoted by $r_s$ or
$\rho_s$, measures the strength and direction of a **monotonic
association** between two variables. It is calculated as the Pearson
correlation between the ranks of the two variables.

When there are no tied ranks, the shortcut formula is:

$$
r_s = 1 - \frac{6 \sum d_i^2}{n(n^2 - 1)}
$$

where:

- $d_i$ = difference between the two ranks for pair $i$
- $n$ = number of paired observations

The coefficient ranges from −1 to +1:

- $+1$: perfect positive rank association
- $0$: no rank correlation
- $-1$: perfect negative rank association

When tied values occur, assign average ranks and calculate the Pearson
correlation of the two rank columns. Do not use the shortcut formula
without an appropriate tie treatment.

## Hypotheses

- $H_0$: There is no population rank correlation ($\rho_s = 0$).
- $H_1$: There is a population rank correlation ($\rho_s \neq 0$).

## Procedure

1. Record paired observations for variables X and Y.
2. Rank X values and Y values separately.
3. Use average ranks for tied values.
4. Calculate $d_i = R_{X_i} - R_{Y_i}$.
5. Square each difference and calculate $\sum d_i^2$.
6. Use the shortcut formula if there are no ties.
7. If ties occur, calculate Pearson correlation between the rank
   columns.
8. Interpret the coefficient and, if required, test its significance.

## Worked example A — Small sample (no ties)

| Person | X | Rank X | Y | Rank Y | $d$ | $d^2$ |
|---:|---:|---:|---:|---:|---:|---:|
| 1 | 10 | 1 | 20 | 2 | −1 | 1 |
| 2 | 20 | 2 | 10 | 1 | 1 | 1 |
| 3 | 30 | 3 | 30 | 3 | 0 | 0 |
| 4 | 40 | 4 | 50 | 5 | −1 | 1 |
| 5 | 50 | 5 | 40 | 4 | 1 | 1 |

$$
n = 5, \qquad \sum d_i^2 = 4
$$

$$
r_s = 1 - \frac{6(4)}{5(5^2 - 1)}
$$

$$
r_s = 1 - \frac{24}{120} = 0.80
$$

**Interpretation:** There is a strong positive rank correlation between
X and Y in this sample. (With only $n = 5$, $r_s = 0.80$ is not
significant at the 5% two-sided level; the exact critical value is
$r_s = 1.00$.)

## Worked example B — Larger sample (no ties)

Suppose $n = 20$, with X ranks 1 through 20 and Y ranks:

$$
2, 1, 4, 3, 6, 5, 8, 7, 10, 9, 12, 11, 14, 13, 16, 15, 18, 17, 20, 19
$$

Each adjacent pair is swapped. Therefore, every one of the 20
observations has a squared rank difference of 1, giving:

$$
\sum d_i^2 = 20
$$

$$
r_s = 1 - \frac{6(20)}{20(20^2 - 1)}
$$

$$
r_s = 1 - \frac{120}{7980} \approx 0.985
$$

**Interpretation:** There is a very strong positive rank correlation
between X and Y.

## Interpretation guide

| Value of $r_s$ | General interpretation |
|:---|:---|
| Close to +1 | Strong positive monotonic association |
| Close to 0 | Weak or no monotonic association |
| Close to −1 | Strong negative monotonic association |

The exact strength labels (weak, moderate, strong) are guidelines and
may vary by subject or reference book.

---

# Quick comparison of all practicals

| Test | Number of groups/samples | Relationship | Main purpose |
|:---|:---|:---|:---|
| Wald–Wolfowitz Runs | One sequence | Ordered sequence | Test randomness |
| Sign Test | One sample or paired samples | One-sample / paired | Test median or paired direction |
| Wilcoxon Signed-Rank | Two | Paired | Compare paired measurements |
| K–S One-Sample | One | Sample vs theoretical distribution | Goodness of fit |
| K–S Two-Sample | Two | Independent | Compare entire distributions |
| Mann–Whitney U | Two | Independent | Compare ranks/distributions |
| Wilcoxon Rank-Sum | Two | Independent | Compare ranks/distributions |
| Kruskal–Wallis | Three or more | Independent | Compare ranks/distributions |
| Spearman Rank Correlation | Two variables | Paired observations | Measure monotonic association |

## Most important distinctions for the practical exam

1. **Runs Test:** Is the order of observations random?
2. **Sign Test:** Are positive and negative differences equally likely?
3. **Wilcoxon Signed-Rank:** Are two paired measurements different,
   using signed ranks?
4. **K–S One-Sample:** Does one sample follow a specified theoretical
   distribution?
5. **K–S Two-Sample:** Do two independent samples have the same
   distribution?
6. **Mann–Whitney U / Wilcoxon Rank-Sum:** Do two independent groups
   differ in their rank distributions? These are equivalent forms of
   the same test.
7. **Kruskal–Wallis:** Do three or more independent groups differ?
8. **Spearman:** How strongly and in what direction are two variables
   monotonically associated?

## Formula revision sheet

| Practical | Main statistic |
|:---|:---|
| Runs Test | $Z = \dfrac{R - E(R)}{\sqrt{\operatorname{Var}(R)}}$ |
| Sign Test | $X \sim B(n,\ 0.5)$ |
| Wilcoxon Signed-Rank | $T = \min(W^+,\ W^-)$ |
| K–S One-Sample | $D = \max(D^+,\ D^-)$ |
| K–S Two-Sample | $D = \sup_x \lvert F_1(x) - F_2(x) \rvert$ |
| Mann–Whitney U | $U_1 = R_1 - \dfrac{n_1(n_1+1)}{2}$ |
| Wilcoxon Rank-Sum | $W = R_1$, with conversion to U if needed |
| Kruskal–Wallis | $H = \dfrac{12}{N(N+1)} \sum \dfrac{R_i^2}{n_i} - 3(N+1)$ |
| Spearman | $r_s = 1 - \dfrac{6 \sum d_i^2}{n(n^2 - 1)}$, when there are no ties |

**Final exam reminder:** Always write the hypotheses, identify whether
the samples are independent or paired, state the significance level,
calculate the correct statistic, make the decision, and finish with a
conclusion in the context of the question.
