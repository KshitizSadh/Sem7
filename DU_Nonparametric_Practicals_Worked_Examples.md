# DU Non-Parametric Methods --- Practical Record & Worked Examples

**Topics covered**

1.  Test for Randomness --- Wald--Wolfowitz Runs Test\
2.  Sign Test\
3.  Wilcoxon Signed-Rank Test\
4.  Kolmogorov--Smirnov One-Sample Test\
5.  Kolmogorov--Smirnov Two-Sample Test\
6.  Mann--Whitney U Test\
7.  Wilcoxon Rank-Sum Test\
8.  Kruskal--Wallis One-Way ANOVA by Ranks\
9.  Spearman's Coefficient of Rank Correlation

> **How to use this file:** Each practical has an aim, objectives,
> theory, hypotheses, formulae, procedure, and two worked illustrations:
> a small sample and a larger sample. The examples are for learning and
> can be copied into a practical record after adapting the wording to
> your teacher's format.

------------------------------------------------------------------------

## General notes before starting

-   Choose the significance level before testing. The examples generally
    use **5% significance level**.
-   "Do not reject (H_0)" is the correct statistical wording; it does
    not prove that (H_0) is true.
-   For rank tests, assign **average ranks** when values are tied.
-   For small samples, use exact distributions or the appropriate
    critical-value table wherever available. Large-sample normal or
    chi-square approximations are not always reliable for very small
    samples.
-   A test can detect a difference in distributions. Interpreting it
    specifically as a difference in medians requires additional
    assumptions, such as similarly shaped distributions.

------------------------------------------------------------------------

# 1. Test for Randomness --- Wald--Wolfowitz Runs Test

## Aim

To test whether a given sequence of observations is random using the
Wald--Wolfowitz Runs Test.

## Objectives

-   To understand the meaning of a run in a sequence.
-   To determine whether observations occur in a random order.
-   To identify possible patterns such as clustering or alternation.

## Theory

A **run** is a consecutive sequence of identical symbols that is
preceded and followed by a different symbol or by the beginning/end of
the sequence. For example,

\[ +++;\|;--;\|;+;\|;---;\|;++ \]

contains five runs.

For a one-sample randomness test, observations are commonly classified
as above or below the median. Values equal to the median are handled
according to the convention specified by the course/software; a common
approach is to omit median ties. The resulting sequence is examined for
its number of runs.

Let:

-   (n_1) = number of observations in category 1
-   (n_2) = number of observations in category 2
-   \(R\) = observed number of runs
-   (N=n_1+n_2)

Expected number of runs:

\[ E(R)=`\frac{2n_1n_2}{N}`{=tex}+1 \]

Variance:

\[ `\operatorname{Var}`{=tex}(R)=
`\frac{2n_1n_2(2n_1n_2-n_1-n_2)}`{=tex} {N\^2(N-1)} \]

For a sufficiently large sample:

\[ Z=`\frac{R-E(R)}{\sqrt{\operatorname{Var}(R)}}`{=tex} \]

## Hypotheses

-   (H_0): The sequence is randomly ordered.
-   (H_1): The sequence is not randomly ordered.

## Procedure

1.  Arrange the observations in their original order.
2.  Find the median and classify observations as above (+) or below (−)
    the median.
3.  Apply the chosen rule to observations equal to the median.
4.  Count (n_1), (n_2), and the observed number of runs (R).
5.  For a large sample, calculate the expected runs, variance, and (Z).
6.  Compare with the appropriate critical value or use the p-value.

## Worked example A --- Small sample (exact runs distribution)

Sequence:

\[
+`\quad`{=tex}+`\quad`{=tex}-`\quad`{=tex}-`\quad`{=tex}+`\quad`{=tex}-`\quad`{=tex}-`\quad`{=tex}+
\]

There are four positive signs and four negative signs:

\[ n_1=4,`\quad`{=tex}n_2=4,`\quad`{=tex}N=8 \]

Runs:

\[ ++;\|;--;\|;+;\|;--;\|;+ \]

Therefore:

\[ R=5 \]

For a small sample, use the **exact runs distribution table** for
(n_1=4,n_2=4), rather than relying on the large-sample normal
approximation. The exact two-sided p-value for (R=5) is approximately
**0.486**.

At the 5% level, (p\>0.05), so **do not reject (H_0)**. There is
insufficient evidence that the sequence is non-random.

## Worked example B --- Larger sample (normal approximation)

Suppose a sequence contains (n_1=50) plus signs and (n_2=50) minus
signs, and the observed number of runs is (R=35).

\[ N=100 \]

\[ E(R)=`\frac{2(50)(50)}{100}`{=tex}+1=51 \]

\[ `\operatorname{Var}`{=tex}(R)=
`\frac{2(50)(50)(5000-100)}{100^2(99)}`{=tex} `\approx24.747`{=tex} \]

\[ Z=`\frac{35-51}{\sqrt{24.747}}`{=tex}`\approx-3.22`{=tex} \]

At 5% two-tailed significance, the critical values are approximately
(`\pm1.96`{=tex}). Since (\|Z\|=3.22\>1.96), **reject (H_0)**. The
sequence shows evidence of non-randomness (too few runs, suggesting
clustering).

## Decision rule

For a two-tailed test at 5%: - If (\|Z\|`\leq1.96`{=tex}), do not reject
(H_0). - If (\|Z\|\>1.96), reject (H_0).

For small samples, use the exact runs table/p-value. Some references use
a continuity correction for the normal approximation.

------------------------------------------------------------------------

# 2. Sign Test

## Aim

To apply the Sign Test to determine whether a population median equals a
specified value or whether two paired conditions differ.

## Objectives

-   To understand a non-parametric test based only on positive and
    negative signs.
-   To test a population median or compare paired observations.
-   To analyse data without requiring a normal distribution.

## Theory

The **Sign Test** is a non-parametric test. For a one-sample test,
compare each observation (X_i) with the hypothesised median (M_0):

-   Write (+) if (X_i\>M_0).
-   Write (−) if (X_i\<M_0).
-   Ignore observations equal to (M_0).

For paired data, calculate each pair's difference and use its sign.
Under (H_0), positive and negative signs are equally likely:

\[ P(+)=P(-)=0.5 \]

If (n) is the number of non-zero differences, the number of positive
signs follows:

\[ X`\sim`{=tex}`\operatorname{Binomial}`{=tex}(n,0.5) \]

For a large sample, a normal approximation may be used:

\[
`\mu`{=tex}=`\frac`{=tex}n2,`\qquad`{=tex}`\sigma`{=tex}=`\sqrt{\frac n4}`{=tex}
\]

With continuity correction, a common two-sided calculation is:

\[ Z=`\frac{|X-n/2|-0.5}{\sqrt{n/4}}`{=tex} \]

## Hypotheses

For a one-sample test: - (H_0): Population median (=M_0). - (H_1):
Population median (`\neq`{=tex}M_0).

For paired data: - (H_0): Median paired difference (=0). - (H_1): Median
paired difference (`\neq0`{=tex}).

## Procedure

1.  State the hypothesised median or calculate paired differences.
2.  Assign +, −, or zero.
3.  Omit zero differences.
4.  Count positive and negative signs.
5.  Use the exact binomial distribution for small samples.
6.  For large samples, use the normal approximation with continuity
    correction.
7.  Compare the p-value with (`\alpha`{=tex}).

## Worked example A --- Small sample (exact binomial test)

A sample of 10 observations is compared with a hypothesised median.
Eight observations are above it and two are below it.

\[ n=10,`\quad`{=tex}X=8 \]

For a two-sided test, the exact p-value is:

\[ p=2P(X`\leq2`{=tex}) \]

\[
p=2`\left[\frac{\binom{10}{0}+\binom{10}{1}+\binom{10}{2}}{2^{10}}\right]`{=tex}\]

\[ p=2`\left`{=tex}(`\frac{1+10+45}{1024}`{=tex}`\right`{=tex}) =0.1094
\]

Since (0.1094\>0.05), **do not reject (H_0)**. The sample does not
provide sufficient evidence that the population median differs from the
hypothesised value.

## Worked example B --- Larger sample (normal approximation)

Suppose 30 non-zero paired differences give 23 positive signs and 7
negative signs.

\[ n=30,`\quad`{=tex}X=23 \]

\[
`\mu=15`{=tex},`\qquad`{=tex}`\sigma`{=tex}=`\sqrt{7.5}`{=tex}`\approx2.739`{=tex}
\]

With continuity correction:

\[ Z=`\frac{23-15-0.5}{2.739}`{=tex}`\approx2.92`{=tex} \]

Since (2.92\>1.96), **reject (H_0)** at the 5% two-sided level. There is
evidence of a systematic difference in the positive direction.

## Decision rule

Reject (H_0) when the p-value is less than or equal to the significance
level. For a large-sample two-sided test at 5%, compare (\|Z\|) with
1.96.

------------------------------------------------------------------------

# 3. Wilcoxon Signed-Rank Test

## Aim

To determine whether there is a significant difference between two
related or paired samples using the Wilcoxon Signed-Rank Test.

## Objectives

-   To compare two related measurements.
-   To rank the absolute paired differences.
-   To test whether the distribution of paired differences is centred at
    zero.

## Theory

The **Wilcoxon Signed-Rank Test** is a non-parametric alternative to the
paired-samples (t)-test. It uses both the **magnitude (through ranks)**
and the **sign** of paired differences.

For each pair:

\[ d_i=X_i-Y_i \]

Ignore zero differences. Rank the absolute differences (\|d_i\|) from
smallest to largest. Give tied absolute differences their average rank,
then restore the original + or − sign.

Let: - (W\^+) = sum of positive ranks - (W\^-) = sum of negative ranks

For the traditional small-sample table, the test statistic is often:

\[ T=`\min`{=tex}(W^+,W^-) \]

For large samples, a normal approximation can be used. With no ties in
absolute differences:

\[ `\mu`{=tex}\_W=`\frac{n(n+1)}`{=tex}4 \]

\[ `\sigma`{=tex}\_W=`\sqrt{\frac{n(n+1)(2n+1)}{24}}`{=tex} \]

A continuity correction may be applied. Ties require a variance
adjustment.

## Hypotheses

-   (H_0): The median paired difference is zero (under the usual
    symmetry assumption, the difference distribution is centred at
    zero).
-   (H_1): The median paired difference is not zero.

## Procedure

1.  Calculate paired differences.
2.  Remove zero differences.
3.  Rank the absolute differences.
4.  Restore the signs to the ranks.
5.  Calculate (W\^+) and (W\^-).
6.  Use an exact Wilcoxon table for small samples or a normal
    approximation for large samples.
7.  Make the decision using the p-value or critical value.

## Worked example A --- Small sample

Six paired differences are:

\[ +1,;+2,;+3,;+4,;+5,;-6 \]

    Difference (d)   (   d    )
  ---------------- --- --- ----
                +1   1   1   +1
                +2   2   2   +2
                +3   3   3   +3
                +4   4   4   +4
                +5   5   5   +5
                −6   6   6   −6

\[ W\^+=1+2+3+4+5=15 \]

\[ W\^-=6 \]

\[ T=`\min`{=tex}(15,6)=6 \]

For (n=6), the exact two-sided p-value is approximately **0.219**. Since
(p\>0.05), **do not reject (H_0)**. The result is not statistically
significant at the 5% level.

## Worked example B --- Larger sample (normal approximation)

Suppose there are 20 non-zero paired differences. Their absolute values
have ranks 1 through 20, with the two largest ranks (19 and 20) negative
and all other ranks positive.

\[ W\^+=1+2+`\cdots`{=tex}+18=171 \]

\[ W\^-=19+20=39 \]

\[ T=39 \]

For (n=20):

\[ `\mu`{=tex}\_W=`\frac{20(21)}`{=tex}4=105 \]

\[ `\sigma`{=tex}\_W=`\sqrt{\frac{20(21)(41)}{24}}`{=tex}
`\approx26.786`{=tex} \]

Using continuity correction:

\[ Z=`\frac{|39-105|-0.5}{26.786}`{=tex}`\approx2.45`{=tex} \]

Since (2.45\>1.96), **reject (H_0)** at the 5% two-sided level. There is
evidence of a non-zero paired difference.

## Important note

The Signed-Rank Test is for **paired/dependent samples**. It is not the
same as the Wilcoxon Rank-Sum Test, which is for independent samples.

------------------------------------------------------------------------

# 4. Kolmogorov--Smirnov One-Sample Test

## Aim

To determine whether a sample comes from a specified continuous
theoretical probability distribution using the one-sample
Kolmogorov--Smirnov test.

## Objectives

-   To compare an empirical distribution with a theoretical
    distribution.
-   To measure the maximum difference between the two cumulative
    distribution functions.
-   To test whether the sample is consistent with a specified
    distribution.

## Theory

The **one-sample Kolmogorov--Smirnov (K--S) test** compares the
empirical cumulative distribution function (ECDF) of a sample with a
specified theoretical cumulative distribution function (CDF).

For ordered observations (x\_{(1)},`\ldots`{=tex},x\_{(n)}), let (F(x))
be the specified theoretical CDF. Define:

\[ D\^+=`\max`{=tex}\_i`\left[\frac{i}{n}-F(x_{(i)})\right]`{=tex}\]

\[ D\^-=`\max`{=tex}\_i`\left[F(x_{(i)})-\frac{i-1}{n}\right]`{=tex}\]

The test statistic is:

\[ D=`\max`{=tex}(D^+,D^-) \]

## Hypotheses

-   (H_0): The sample follows the specified theoretical distribution.
-   (H_1): The sample does not follow the specified theoretical
    distribution.

## Procedure

1.  Specify the continuous theoretical distribution under (H_0).
2.  Arrange the sample observations in ascending order.
3.  Calculate the theoretical cumulative probability (F(x_i)) for each
    observation.
4.  Calculate (D\^+) and (D\^-).
5.  Take the larger value as (D).
6.  Compare with the appropriate critical value or obtain the p-value.

## Worked example A --- Small sample

Test whether the following five observations are consistent with a
Uniform(0,1) distribution:

\[ 0.10,;0.25,;0.40,;0.70,;0.90 \]

For Uniform(0,1), (F(x)=x).

  ---------------------------------------------------------------------------------------
     \(i\)   (x\_{(i)})    (i/n)   ((i-1)/n)   (F(x_i))   (i/n-F(x_i))   (F(x_i)-(i-1)/n)
  -------- ------------ -------- ----------- ---------- -------------- ------------------
         1         0.10     0.20        0.00       0.10           0.10               0.10

         2         0.25     0.40        0.20       0.25           0.15               0.05

         3         0.40     0.60        0.40       0.40           0.20               0.00

         4         0.70     0.80        0.60       0.70           0.10               0.10

         5         0.90     1.00        0.80       0.90           0.10               0.10
  ---------------------------------------------------------------------------------------

\[ D^+=0.20,`\qquad`{=tex}D^-=0.10 \]

\[ D=0.20 \]

For (n=5), the 5% exact critical value is approximately 0.565. Since
(0.20\<0.565), **do not reject (H_0)**. The sample is not significantly
different from Uniform(0,1).

## Worked example B --- Larger sample

Suppose (n=40) observations are tested against Uniform(0,1). Assume 20
observations are between 0.01 and 0.20, and the remaining 20 are between
0.71 and 0.90.

At (x=0.20):

\[ F_n(0.20)=`\frac{20}{40}`{=tex}=0.50 \]

\[ F(0.20)=0.20 \]

Therefore, the difference at this point is:

\[ \|0.50-0.20\|=0.30 \]

Thus (D`\geq0.30`{=tex}). The 5% large-sample approximate critical value
is:

\[ D\_{critical}=`\frac{1.36}{\sqrt{40}}`{=tex}`\approx0.215`{=tex} \]

Since (D`\geq0.30`{=tex}\>0.215), **reject (H_0)**. The observations are
inconsistent with the specified Uniform(0,1) distribution.

## Important notes

-   The usual one-sample K--S critical values assume the theoretical
    distribution is fully specified in advance.
-   If parameters such as the normal mean and standard deviation are
    estimated from the same sample, the ordinary K--S critical values
    are not valid; a correction such as the Lilliefors test or a
    suitable simulation method is needed.
-   The classical K--S test is designed for continuous distributions.

------------------------------------------------------------------------

# 5. Kolmogorov--Smirnov Two-Sample Test

## Aim

To determine whether two independent samples come from the same
continuous probability distribution.

## Objectives

-   To compare the empirical distributions of two independent samples.
-   To calculate the maximum difference between their ECDFs.
-   To test whether the two population distributions differ.

## Theory

The **two-sample K--S test** compares the empirical cumulative
distribution functions of two independent samples.

Let (F\_{1,n}(x)) and (F\_{2,m}(x)) be the empirical CDFs of samples of
sizes (n_1) and (n_2).

The test statistic is:

\[ D=`\sup`{=tex}*x\|F*{1,n}(x)-F\_{2,m}(x)\| \]

It is the largest vertical distance between the two empirical CDFs.

## Hypotheses

-   (H_0): Both samples come from the same continuous distribution.
-   (H_1): The two population distributions differ.

## Procedure

1.  Combine and sort the distinct observations from both samples.
2.  At each distinct value, calculate the cumulative proportion in
    sample 1 and sample 2.
3.  Find the absolute difference between the two cumulative proportions.
4.  The largest difference is (D).
5.  Compare with the critical value or use the p-value.

## Worked example A --- Small sample

Sample 1:

\[ 1,;2,;3,;4,;5 \]

Sample 2:

\[ 6,;7,;8,;9,;10 \]

The two samples do not overlap. After the first sample's five
observations, its ECDF is 1 while the second sample's ECDF is 0.
Therefore:

\[ D=1.00 \]

For (n_1=n_2=5), the exact two-sided 5% critical value is 0.80. Since
(1.00\>0.80), **reject (H_0)**. There is evidence that the two
distributions differ.

## Worked example B --- Larger sample

Let sample 1 contain the integers 1 through 40, and sample 2 contain the
integers 21 through 60.

At (x=20):

\[ F_1(20)=`\frac{20}{40}`{=tex}=0.50 \]

\[ F_2(20)=0 \]

Therefore:

\[ D`\geq0.50`{=tex} \]

For (n_1=n_2=40), the approximate 5% critical value is:

\[ D\_{critical}=1.36`\sqrt{\frac{40+40}{40(40)}}`{=tex} \]

\[ D\_{critical}`\approx0.304`{=tex} \]

Since (D`\geq0.50`{=tex}\>0.304), **reject (H_0)**. The two samples show
significantly different distributions.

## Important note

The two-sample K--S test compares the **whole distributions**, not only
their means or medians. The standard test assumes independent samples
and continuous distributions.

------------------------------------------------------------------------

# 6. Mann--Whitney U Test

## Aim

To determine whether two independent samples differ significantly using
the Mann--Whitney U Test.

## Objectives

-   To compare two independent groups using ranks.
-   To apply a non-parametric alternative to the independent-samples
    (t)-test.
-   To determine whether observations in one group tend to be higher or
    lower than those in the other group.

## Theory

The **Mann--Whitney U test** is a non-parametric test for comparing two
independent samples. Combine the observations, rank them from smallest
to largest, and calculate the sum of ranks for each group.

Let: - (n_1,n_2) = sample sizes - (R_1,R_2) = rank sums

The U statistics are:

\[ U_1=R_1-`\frac{n_1(n_1+1)}`{=tex}2 \]

\[ U_2=R_2-`\frac{n_2(n_2+1)}`{=tex}2 \]

\[ U=`\min`{=tex}(U_1,U_2) \]

For large samples, a normal approximation may be used. Without ties:

\[ `\mu`{=tex}\_U=`\frac{n_1n_2}{2}`{=tex} \]

\[ `\sigma`{=tex}\_U=`\sqrt{\frac{n_1n_2(n_1+n_2+1)}{12}}`{=tex} \]

A continuity correction and tie adjustment may be needed.

## Hypotheses

-   (H_0): The two population distributions are the same.
-   (H_1): The two population distributions differ.

If the two distributions have the same shape and spread, the test is
often interpreted as a test of location/median difference.

## Procedure

1.  Take two independent samples.
2.  Combine all observations.
3.  Rank them in ascending order; use average ranks for ties.
4.  Calculate each group's rank sum.
5.  Calculate (U_1), (U_2), and the smaller U.
6.  Use an exact table for small samples or a normal approximation for
    large samples.
7.  Make the decision.

## Worked example A --- Small sample

Sample A: (4,5,6,7)\
Sample B: (1,2,3,8)

    Value  Group    Combined rank
  ------- ------- ---------------
        1    B                  1
        2    B                  2
        3    B                  3
        4    A                  4
        5    A                  5
        6    A                  6
        7    A                  7
        8    B                  8

\[ R_A=4+5+6+7=22 \]

\[ R_B=1+2+3+8=14 \]

With (n_A=n_B=4):

\[ U_A=22-`\frac{4(5)}`{=tex}2=12 \]

\[ U_B=14-`\frac{4(5)}`{=tex}2=4 \]

\[ U=`\min`{=tex}(12,4)=4 \]

For (n_1=n_2=4), the exact two-sided p-value is approximately 0.343.
Since (p\>0.05), **do not reject (H_0)**.

## Worked example B --- Larger sample (normal approximation)

Sample A has values 21 through 40; sample B has values 1 through 20.
Thus every A observation is greater than every B observation.

\[ n_1=n_2=20 \]

The U statistic for the lower-ranked group is:

\[ U=0 \]

\[ `\mu`{=tex}\_U=`\frac{20(20)}`{=tex}2=200 \]

\[
`\sigma`{=tex}\_U=`\sqrt{\frac{20(20)(41)}{12}}`{=tex}`\approx36.97`{=tex}
\]

Using continuity correction:

\[ Z=`\frac{|0-200|-0.5}{36.97}`{=tex}`\approx5.40`{=tex} \]

Since (5.40\>1.96), **reject (H_0)**. The samples differ significantly.

------------------------------------------------------------------------

# 7. Wilcoxon Rank-Sum Test

## Aim

To compare two independent samples using the Wilcoxon Rank-Sum Test.

## Objectives

-   To understand rank-sum testing for independent samples.
-   To calculate and compare the sums of ranks.
-   To determine whether two independent population distributions
    differ.

## Theory

The **Wilcoxon Rank-Sum Test** is a non-parametric test for two
independent samples. It is mathematically equivalent to the
Mann--Whitney U test; the difference is mainly the way the statistic is
reported.

Combine both samples, rank all observations, and calculate the rank sum
for each sample.

Let (R_1) be the rank sum for sample 1. The corresponding U statistic
is:

\[ U_1=R_1-`\frac{n_1(n_1+1)}`{=tex}2 \]

Similarly:

\[ U_2=R_2-`\frac{n_2(n_2+1)}`{=tex}2 \]

The relationship is:

\[ U_1+U_2=n_1n_2 \]

## Hypotheses

-   (H_0): The two population distributions are the same.
-   (H_1): The two population distributions differ.

## Procedure

1.  Combine the two independent samples.
2.  Arrange observations in ascending order.
3.  Assign ranks; average ranks for ties.
4.  Find the rank sum of each sample.
5.  Convert the rank sum to U if required by the critical-value
    table/software.
6.  Compare the statistic or p-value with the chosen significance level.

## Worked example A --- Small sample

Use the same data as the small Mann--Whitney example:

-   Sample A: (4,5,6,7)
-   Sample B: (1,2,3,8)

The rank sums are:

\[ R_A=22,`\qquad`{=tex}R_B=14 \]

The rank-sum statistic for A is (W_A=22). Convert to U:

\[ U_A=22-`\frac{4(5)}`{=tex}2=12 \]

For B:

\[ U_B=14-`\frac{4(5)}`{=tex}2=4 \]

The smaller U is 4. The exact two-sided p-value is approximately 0.343.
Since (p\>0.05), **do not reject (H_0)**.

## Worked example B --- Larger sample

Sample A: values 21 through 40.\
Sample B: values 1 through 20.

All B observations receive ranks 1--20 and all A observations receive
ranks 21--40.

\[ R_A=21+22+`\cdots`{=tex}+40=610 \]

\[ R_B=1+2+`\cdots`{=tex}+20=210 \]

For A:

\[ U_A=610-`\frac{20(21)}`{=tex}2=400 \]

For B:

\[ U_B=210-`\frac{20(21)}`{=tex}2=0 \]

Thus (U=0). The large-sample normal approximation gives a very large
absolute Z value (about 5.4 with continuity correction), so **reject
(H_0)**.

## Important practical note

**Mann--Whitney U and Wilcoxon Rank-Sum are two forms of the same
test.** If both appear separately in the practical list, explain that
the rank-sum form reports (R), while the Mann--Whitney form reports (U).

------------------------------------------------------------------------

# 8. Kruskal--Wallis One-Way ANOVA by Ranks

## Aim

To determine whether three or more independent samples differ
significantly using the Kruskal--Wallis test.

## Objectives

-   To compare three or more independent groups.
-   To rank observations from all groups together.
-   To apply a non-parametric alternative to one-way ANOVA.

## Theory

The **Kruskal--Wallis test** is a non-parametric method for comparing
three or more independent groups. It is based on the ranks of all
observations rather than their original values.

Let: - (k) = number of groups - (n_i) = size of group (i) -
(N=`\sum`{=tex}n_i) = total sample size - (R_i) = sum of ranks in group
(i)

The test statistic is:

\[ H=`\frac{12}{N(N+1)}`{=tex}
`\sum`{=tex}\_{i=1}\^{k}`\frac{R_i^2}{n_i}`{=tex} -3(N+1) \]

When there are no ties, (H) is approximately distributed as chi-square
with:

\[ df=k-1 \]

For tied observations, apply the tie correction:

\[ C=1-`\frac{\sum(t_j^3-t_j)}{N^3-N}`{=tex} \]

\[ H\_{`\text{corrected}`{=tex}}=`\frac{H}{C}`{=tex} \]

where (t_j) is the number of observations in the (j)-th tied group.

## Hypotheses

-   (H_0): All group distributions are the same.
-   (H_1): At least one group distribution differs.

## Procedure

1.  Take three or more independent samples.
2.  Combine all observations and rank them.
3.  Assign average ranks to ties.
4.  Calculate the rank sum for each group.
5.  Substitute the rank sums and group sizes into the H formula.
6.  Apply tie correction if needed.
7.  Compare H with the chi-square critical value with (k-1) degrees of
    freedom.
8.  If significant, use a post-hoc test to identify which groups differ.

## Worked example A --- Small sample

Three independent groups:

-   A: (1,2,3,4)
-   B: (5,6,7,8)
-   C: (9,10,11,12)

All values are distinct, so assign ranks 1 through 12.

   Group  Ranks             Rank sum (R_i)   (n_i)
  ------- --------------- ---------------- -------
     A    1, 2, 3, 4                    10       4
     B    5, 6, 7, 8                    26       4
     C    9, 10, 11, 12                 42       4

\[ N=12,`\quad`{=tex}k=3 \]

\[ H=`\frac{12}{12(13)}`{=tex}
`\left`{=tex}(`\frac{10^2}{4}`{=tex}+`\frac{26^2}{4}`{=tex}+`\frac{42^2}{4}`{=tex}`\right`{=tex})-3(13)
\]

\[ H`\approx9.846`{=tex} \]

Degrees of freedom:

\[ df=k-1=2 \]

At 5% significance, (`\chi`{=tex}\^2\_{0.05,2}=5.991). Since
(9.846\>5.991), **reject (H_0)**. At least one group distribution
differs.

## Worked example B --- Larger sample

Three independent groups, each with 10 observations:

-   A: 1 through 10
-   B: 11 through 20
-   C: 21 through 30

The combined ranks are 1 through 30.

\[ N=30,`\quad`{=tex}n_A=n_B=n_C=10 \]

Rank sums:

\[ R_A=1+`\cdots`{=tex}+10=55 \]

\[ R_B=11+`\cdots`{=tex}+20=155 \]

\[ R_C=21+`\cdots`{=tex}+30=255 \]

\[ H=`\frac{12}{30(31)}`{=tex}
`\left`{=tex}(`\frac{55^2}{10}`{=tex}+`\frac{155^2}{10}`{=tex}+`\frac{255^2}{10}`{=tex}`\right`{=tex})-3(31)
\]

\[ H`\approx25.806`{=tex} \]

\[ df=3-1=2 \]

Since (25.806\>5.991), **reject (H_0)**. There is a significant
difference among the group distributions.

## Important note

A significant Kruskal--Wallis result tells us that **at least one group
differs**. It does not identify the pair(s) responsible. Use an
appropriate post-hoc procedure, such as Dunn's test with a
multiple-comparison correction.

------------------------------------------------------------------------

# 9. Spearman's Coefficient of Rank Correlation

## Aim

To calculate Spearman's Rank Correlation Coefficient and determine the
strength and direction of association between two variables.

## Objectives

-   To measure association using ranks.
-   To identify positive, negative, or absent monotonic association.
-   To analyse ordinal data or data that do not meet the assumptions of
    Pearson correlation.

## Theory

**Spearman's rank correlation coefficient**, denoted by (r_s) or
(`\rho`{=tex}\_s), measures the strength and direction of a **monotonic
association** between two variables. It is calculated as the Pearson
correlation between the ranks of the two variables.

When there are no tied ranks, the shortcut formula is:

\[ r_s=1-`\frac{6\sum d_i^2}{n(n^2-1)}`{=tex} \]

where: - (d_i) = difference between the two ranks for pair (i) - (n) =
number of paired observations

The coefficient ranges from −1 to +1: - (+1): perfect positive rank
association - (0): no rank correlation - (−1): perfect negative rank
association

When tied values occur, assign average ranks and calculate the Pearson
correlation of the two rank columns. Do not use the shortcut formula
without an appropriate tie treatment.

## Hypotheses

-   (H_0): There is no population rank correlation
    ((`\rho`{=tex}\_s=0)).
-   (H_1): There is a population rank correlation
    ((`\rho`{=tex}\_s`\neq0`{=tex})).

## Procedure

1.  Record paired observations for variables X and Y.
2.  Rank X values and Y values separately.
3.  Use average ranks for tied values.
4.  Calculate (d_i=R\_{Xi}-R\_{Yi}).
5.  Square each difference and calculate (`\sum`{=tex}d_i\^2).
6.  Use the shortcut formula if there are no ties.
7.  If ties occur, calculate Pearson correlation between the rank
    columns.
8.  Interpret the coefficient and, if required, test its significance.

## Worked example A --- Small sample (no ties)

    Person    X   Rank X    Y   Rank Y   \(d\)   (d\^2)
  -------- ---- -------- ---- -------- ------- --------
         1   10        1   20        2      −1        1
         2   20        2   10        1       1        1
         3   30        3   30        3       0        0
         4   40        4   50        5      −1        1
         5   50        5   40        4       1        1

\[ n=5,`\qquad`{=tex}`\sum`{=tex}d_i\^2=4 \]

\[ r_s=1-`\frac{6(4)}{5(5^2-1)}`{=tex} \]

\[ r_s=1-`\frac{24}{120}`{=tex}=0.80 \]

**Interpretation:** There is a strong positive rank correlation between
X and Y.

## Worked example B --- Larger sample (no ties)

Suppose (n=20), with X ranks 1 through 20 and Y ranks:

\[ 2,1,4,3,6,5,8,7,10,9,12,11,14,13,16,15,18,17,20,19 \]

Each adjacent pair is swapped. Therefore, each pair contributes two
squared rank differences of 1, giving:

\[ `\sum`{=tex}d_i\^2=20 \]

\[ r_s=1-`\frac{6(20)}{20(20^2-1)}`{=tex} \]

\[ r_s=1-`\frac{120}{7980}`{=tex}`\approx0.985`{=tex} \]

**Interpretation:** There is a very strong positive rank correlation
between X and Y.

## Interpretation guide

    Value of (r_s) General interpretation
  ---------------- ---------------------------------------
       Close to +1 Strong positive monotonic association
        Close to 0 Weak or no monotonic association
       Close to −1 Strong negative monotonic association

The exact strength labels (weak, moderate, strong) are guidelines and
may vary by subject or reference book.

------------------------------------------------------------------------

# Quick comparison of all practicals

  --------------------------------------------------------------------------
  Test                        Number of Relationship   Main purpose
                         groups/samples                
  ----------------- ------------------- -------------- ---------------------
  Wald--Wolfowitz          One sequence Ordered        Test randomness
  Runs                                  sequence       

  Sign Test               One sample or One-sample /   Test median or paired
                         paired samples paired         direction

  Wilcoxon                          Two Paired         Compare paired
  Signed-Rank                                          measurements

  K--S One-Sample                   One Sample vs      Goodness of fit
                                        theoretical    
                                        distribution   

  K--S Two-Sample                   Two Independent    Compare entire
                                                       distributions

  Mann--Whitney U                   Two Independent    Compare
                                                       ranks/distributions

  Wilcoxon Rank-Sum                 Two Independent    Compare
                                                       ranks/distributions

  Kruskal--Wallis         Three or more Independent    Compare
                                                       ranks/distributions

  Spearman Rank           Two variables Paired         Measure monotonic
  Correlation                           observations   association
  --------------------------------------------------------------------------

## Most important distinctions for the practical exam

1.  **Runs Test:** Is the order of observations random?
2.  **Sign Test:** Are positive and negative differences equally likely?
3.  **Wilcoxon Signed-Rank:** Are two paired measurements different,
    using signed ranks?
4.  **K--S One-Sample:** Does one sample follow a specified theoretical
    distribution?
5.  **K--S Two-Sample:** Do two independent samples have the same
    distribution?
6.  **Mann--Whitney U / Wilcoxon Rank-Sum:** Do two independent groups
    differ in their rank distributions? These are equivalent forms of
    the same test.
7.  **Kruskal--Wallis:** Do three or more independent groups differ?
8.  **Spearman:** How strongly and in what direction are two variables
    monotonically associated?

## Formula revision sheet

  ----------------------------------------------------------------------------------------------------
  Practical              Main statistic
  ---------------------- -----------------------------------------------------------------------------
  Runs Test              (Z=`\dfrac{R-E(R)}{\sqrt{\operatorname{Var}(R)}}`{=tex})

  Sign Test              (X`\sim`{=tex}B(n,0.5))

  Wilcoxon Signed-Rank   (T=`\min`{=tex}(W^+,W^-))

  K--S One-Sample        (D=`\max`{=tex}(D^+,D^-))

  K--S Two-Sample        (D=`\sup`{=tex}\_x

  Mann--Whitney U        (U_1=R_1-`\dfrac{n_1(n_1+1)}`{=tex}2)

  Wilcoxon Rank-Sum      (W=R_1), with conversion to U if needed

  Kruskal--Wallis        (H=`\dfrac{12}{N(N+1)}`{=tex}`\sum`{=tex}`\dfrac{R_i^2}{n_i}`{=tex}-3(N+1))

  Spearman               (r_s=1-`\dfrac{6\sum d_i^2}{n(n^2-1)}`{=tex}), when there are no ties
  ----------------------------------------------------------------------------------------------------

**Final exam reminder:** Always write the hypotheses, identify whether
the samples are independent or paired, state the significance level,
calculate the correct statistic, make the decision, and finish with a
conclusion in the context of the question.
