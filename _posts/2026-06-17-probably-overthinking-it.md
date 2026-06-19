---
title: "Probably Overthinking It"
description: "These are my notes about the book “Probably Overthinking It” by Allen B. Downey."
date: 2026-06-17 10:01:00 -0400
author: conrad
categories: [Computing]
---

During my career as a bioinformatics scientist, I devoted some nights and
weekends to learning statistics, a smattering of machine learning, and what I
could about data analysis methods and approaches. I had already retired by the
time [Allen B. Downey](https://www.allendowney.com/) published
<cite>[Probably Overthinking It: How to Use Data to Answer Questions, Avoid Statistical Traps, and Make Better Decisions](https://greenteapress.com/wp/probably-overthinking-it/)</cite>,
but I bought and read it to reinforce what I had learned during my working
years.

The subtitle succinctly summarizes this book, which is about using data for
making decisions. Each chapter explores one or many real data sets and examines
in detail potential and real pitfalls in each analysis that can lead to
erroneous conclusions.

## Chapters

### Are You Normal? Hint: No

Chapter 1, "Are You Normal? Hint: No," looks at Gaussian (normal) distributions.
Individual traits, such as forearm length, can be modeled fairly accurately
using the Gaussian distribution. But when you examine many traits, virtually no
one is normal or average. In Dr. Downey’s example, using data from 93
measurements, he found that no one was average for all 93 measurements. In fact,
90% of the 800,000 people measured were not average for at least 57 of the 93
measurements.

### Relay Races and Revolving Doors

In Chapter 2, "Relay Races and Revolving Doors," Dr. Downey examines length-biased
sampling error and the
[inspection paradox](https://en.wikipedia.org/wiki/Renewal_theory#The_inspection_paradox).
The first example uses a data set where college students were asked the average
size of their classes. Dr. Downey demonstrates that because of length-biased
sampling error, a dean can report that mean class size is 35 students but
students can report that mean class size is 90 students. The answer depends on
whether the sample is selected from classrooms (the dean's sample) or from
students. Dr. Downey presents several other examples of the inspection paradox.

### Defy Tradition, Save the World

Chapter 3, "Defy Tradition, Save the World," explores family sizes. First,
average family size reported by mothers is smaller than average family size
reported by children. This is because children from large families are
overrepresented, driving the average up. Second, there can be a seeming paradox
where average family size can increase even when women have fewer children than
their mothers. Dr. Downey provides a simple example that demonstrates this.

### Extremes, Outliers, and GOATs

In Chapter 4, "Extremes, Outliers, and GOATs," Dr. Downey looks at examples of
the lognormal distributions. For a lognormal distribution, the distribution of
the logarithm of the measurement is a normal (Gaussian) distribution. Among the
examples that Dr. Downey examines are weights of people, running speeds, and
chess rankings. If the things we measure are the result of adding many random
factors, the sums tend to follow the normal distribution. If the things we
measure are the result of multiplying many factors, the products tend to follow
the lognormal distribution.

### Better Than New

Chapter 5, "Better Than New," looks at survival curves of incandescent light
bulbs, lengths of pregnancies (a type of survival curve), cancer survival times
for glioblastoma patients, and historical and present-day life expectancies at
birth and later ages. The results are sometimes counterintuitive.

### Jumping to Conclusions

Chapter 6, "Jumping to Conclusions," explores
[Berkson's Paradox](https://en.wikipedia.org/wiki/Berkson%27s_paradox)
for SAT scores, hospital data, Covid-19, and psychological conditions. As in
Chapter 5, the results are sometimes counterintuitive.

### Causation, Collision, and Confusion

In Chapter 7, "Causation, Collision, and Confusion," Dr. Downey examines the low
birth weight paradox, discovered in 1971, where the mortality rate of
underweight babies of mothers who smoked had a lower mortality rate than the
underweight babies of mothers who did not smoke. The explanation, which was
difficult for me to understand, was that low birth weight can be caused by other
factors than smoking, and one factor is birth defects.

> Maternal smoking is still bad for babies, but it is not as bad as birth
> defects. …Low birth rate generally has a cause, and if the cause is not
> smoking, it is more likely to be something else, including a birth defect.
> …By selecting babies with no congenital anomalies observed at birth,
> …we find that babies of smokers have higher mortality weights in nearly
> every weight category.

Dr. Downey continues by examining other paradoxical data of this type.

### The Long Tail of Disaster

Chapter 8, "The Long Tail of Disaster," looks at "the small probabilities of
large events" such as earthquakes, tropical cyclones, wildfires, floods, and
tornadoes. The lognormal distribution can be used to model the costs of most
disasters since that distribution has a long tail, but the observed tail is even
longer than predicted from the lognormal distribution. This means prediction of
the frequency of extremely large disasters using a lognormal model
underestimates their true frequency.

Dr. Downey chooses the Student’s *t* distribution, which has longer tails than
the normal distribution, and he shows that modeling the data using what he calls
the log-*t* distribution results in better predictions. He goes on to model
solar flares, the sizes of craters on the Moon, the sizes of asteroids, stock
market crashes, and what Nicholas Taleb Nassim dubbed "black swan" events.

### Fairness and Fallacy

Chapter 9, "Fairness and Fallacy," explores the
[base rate fallacy](https://en.wikipedia.org/wiki/Base_rate_fallacy)
using three examples: medical tests, measuring blood alchol, and effectiveness
of a vaccine. This topic is difficult to summarize in a paragraph, so I'll turn
to a numerical example from the book.

All tests have error rates that result in false positive results. Given a
disease with a prevalence (base rate) of 1/1000 and a test with a false positive
rate of 5% (95% specificity; this is typical of medical tests) and a sensitivity
of 99% (99 out of 100 people with the disease would be detected by the test),
what is the probability that a person with a positive test actually has the
disease?

Here is the table of what would be expected from testing 100,000 different
people:

|              | # of people | Probability positive test | # positive tests | % true/false positive |
|--------------|------------:|--------------------------:|-----------------:|----------------------:|
| Infected     |         100 |                      0.99 |               99 |                  1.94 |
| Not infected |       99900 |                      0.05 |             4995 |                 98.06 |

With a low base rate of one in a thousand, the test finds 99 true positive and
4995 false positives. Given the base rate, a positive test, and no other
information, the probability that a person with a positive test has the disease
is 99&nbsp;/&nbsp;(99&nbsp;+&nbsp;4995) or only 1.94%.

This surprisingly counterintuitive result causes many problems with the
interpretations of test results, and Dr. Downey explores these problems in
detail. He concludes the chapter by looking at an example of predicting crime
and the use of data and algorithms in the criminal justice system.

### Penguins, Pessimists, and Paradoxes

Chapter 10, "Penguins, Pessimists, and Paradoxes," examines
[Simpson’s paradox](https://en.wikipedia.org/wiki/Simpson%27s_paradox), which
is described in Wikipedia:

> Simpson's paradox is a phenomenon in probability and statistics in which a
> trend appears in several groups of data but disappears or reverses when the
> groups are combined.

Dr. Downey provides several excellent examples of Simpson's paradox, analyzing
data sets for optimism, real wages, penguins, and the effectiveness of vaccines.

### Changing Hearts and Minds

Chapter 11, "Changing Hearts and Minds," continues Dr. Downey's exploration of
Simpson’s paradox, introducing age-period-cohort analysis and the concept of the
[Overton window](https://en.wikipedia.org/wiki/Overton_window).
Using age-period-cohort analysis, a powerful tool that dissects Simpson's
paradox, Dr. Downey's analysis reveals that people in the U.S. have grown less
sexist, less racist, and less homophobic over time.

### Chasing the Overton Window

In Chapter 12, "Chasing the Overton Window," Dr. Downey finishes exploring
Simpson's Paradox, age-period-cohort analysis, and the Overton window. He
explores the labels "conservative" and "liberal" and shows that, although older
people tend to be more conservative, nearly every age cohort becomes more
liberal as its members grow older.

## <cite>Probably Overthinking It</cite> as a Resource for Students

Since Dr. Downey has made available his source code, written in Python using
Jupyter notebooks, this book could serve as the basis for a course in data
analysis for advanced undergraduate students or beginning graduate students. A
person considering a career in data science or wanting to expand their skills
should work through the code to understand each analytical approach.

If I were teaching a beginning course in data analysis, I would add this book
and the source code as resources.

Rating: Five out of five stars, excellent.

## Other Writing by Allen B. Downey

Dr. Downey is the author of many other books, most of which are available at no
cost, including (in alphabetical order):

*   <cite>[Astronomical Data in Python](https://allendowney.github.io/AstronomicalData/README.html)</cite>
*   <cite>[Data Structures and Information Retrieval in Python](https://greenteapress.com/wp/data-structures-and-information-retrieval-in-python/)</cite>
*   <cite>[Elements of Data Science](https://greenteapress.com/wp/elements-of-data-science/)</cite>
*   <cite>[The Little Book of Semaphores](https://greenteapress.com/wp/semaphores/)</cite>
*   <cite>[Modeling and Simulation in Python](https://greenteapress.com/wp/modsimpy/)</cite>
*   <cite>[Physical Modeling in MATLAB](https://greenteapress.com/wp/physical-modeling-in-matlab/)</cite>
*   <cite>[Think Bayes](https://greenteapress.com/wp/think-bayes/)</cite>
*   <cite>[Think C/C++](https://greenteapress.com/wp/think-c/)</cite>
*   <cite>[Think Complexity](https://greenteapress.com/wp/think-complexity-2e/)</cite>
*   <cite>[Think Data Structures](https://greenteapress.com/wp/think-data-structures/)</cite>
*   <cite>[Think DPS: Digital Signal Processing in Python](https://greenteapress.com/wp/think-dsp/)</cite>
*   <cite>[Think Java](https://greenteapress.com/wp/think-java-2e/)</cite>
*   <cite>[Think OS: A Brief Introduction to Operating Systems](https://greenteapress.com/wp/think-os/)</cite>
*   <cite>[Think Python](https://greenteapress.com/wp/think-python-3rd-edition/)</cite>
*   <cite>[Think Stats](https://greenteapress.com/wp/think-stats-3e/)</cite>

Dr. Downey blogs at
[Probably Overthinking It](https://www.allendowney.com/blog/) and on
[Substack](https://allendowney.substack.com).
