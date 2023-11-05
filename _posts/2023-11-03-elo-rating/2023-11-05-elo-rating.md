---
layout: post
title: "Elo Rating, Logistic Distribution, and Logistic Regression"
description: A probabilistic explanation of Elo rating approach
tags:
    - rating
    - probability
    - large language models
giscus_comments: true
---

**Long story short, this post tries to explain Elo rating with a probabilistic perspective, especially the relation with a logistic distribution that many of the existing blogs failed to do.**

I am currently reading papers about large language models alignment tuning [^1], e.g. reinforcement learning from human feedback (RLHF) used for ChatGPT.
One of many kinds of human feedback is "ranking-based approach" [^1], and then I ran into a related paper by DeepMind that uses Elo rating for the feedback [^2]. Elo rating was initially created for assessing chess palyers' skill but later widely applied for online games such as League of Legends ranking [^3].


There already exist many webpages, including [this Elo rating Wikipedia page](https://en.wikipedia.org/wiki/Elo_rating_system), that try to explain Elo rating with mathematical formulas. But, as far as I can go, they fail to explain clearly to beginners, for example, who is logistically distributed, and how to derive the final logistic regression.

## Elo Rating

The core idea of Elo rating system is to "learn" a score, according to their previous comptetion performance, to reveal each player's skill ability. As a result, the win/loss probability of any two players can be well predicted by comparing the score differnce [^6] [^7]. 
For a win/loss games (i.e. no draw), supposing players A, B are assigned with score $$R_A$$ and $$R_B$$ respectively, the probability of player A to win is calculated by Elo rating as 

$$\text{Pr}(\text{A wins}) = \frac{1}{1+10^{-r_{AB}/400}},$$

where $$r_{AB} = R_A - R_B$$ is the score difference between player A and B.
Apprently, this equation is in a logistic function [^4]. And $$R_A$$ and $$R_B$$ can be updated with optimization methods, e.g. SGD, accordingly [^3]. This updating procedure ends up with formula

$$ R_A \leftarrow R_A + K [S_A - \text{Pr}(\text{A wins})],$$

where $$S_A$$ indicates whether player A wins (1) or not (0) (and also similar for the update of player B).


In machine learning, a *softmax function* links non-bounded values with probability: $$p_i = \frac{e^{x_i}}{\sum e^{x_i}}$$. A special case when there are only two variables $$p_a = \frac{e^a}{e^a + e^b} = \frac{1}{1 + e^{b-a}}$$, which can be usually denoted using *sigmoid function* $$\sigma(t) = \frac{1}{1+e^{-t}}$$ (in this case let $$t = a-b$$, in which only the difference between $$a$$ and $$b$$ matters).

Note that the this process is a litter different from machine learning with a batch size because the competetions are streaming data to process, i.e. update parameters *with given sample order*. 
If we have directly optimize on the whole data instead of streaming update, the question can be now described as: *"How to map the win/loss frequency (or probability) of two players with their skill score difference?"* 
This reduces the degree of freedom of variable to 1 (the difference instead of absolute value of two scores), leading to the fitting problem of $$p = \frac{1}{1+e^{-t/s}}$$, where $$p$$ is the probability of, for example, "A wins" and $$s$$ is a scaling factor. Solving this equation leads to $$t = s \ln \frac{p}{1-p}$$. In Elo rating system, $$s = 400$$, and the base is chosen as $$10$$ instead of $$e$$ for human-friendly interpretation. 
Take a small example of the formula: supposing that player A is 400 score less than player B, the log-odds [^5] $$\log \frac{p}{1-p} = -1$$, and then we get $$p = \frac{1}{11}$$ (or the odd $$\frac{p}{1-p} = 1:10$$). Intuitively, a -400 rating difference means that player A only has a chance of $$\frac{1}{11}$$ to win (learned according to statistics).


## Logistic Distribution behind Elo Rating

So far the calculation of Elo rating scores and the link to probability meaning is introduced.
The assumption is that player performance difference can be modeled vis logistic regression.
A higher rating score generally means a better player. 
But because of the randomness in competetions, it happens that a "generally better player" loses.
There are claims [^3] [^6] [^9] pointing out that player's *performance* is rather logistically distributed instead of normally.
This question confused me a lot:

**"What exactly is logistically distributed such that the Elo rating's logistic regression form can be derived?"**

Let me start by quoting a [this blog](https://www.cantorsparadise.com/the-mathematics-of-elo-ratings-b6bfc9ca1dba) [^6],

> the system assumed that players’ expected scores conform to a normal distribution. Later, both the USCF and FIDE altered their systems when it was found their empirics suggested that performance in chess more resembles a logistic distribution (heavier tails/higher probability of extreme outcomes).


### 1. Player Absolute Performance

It seems that the intuitive purpose of changing model is to describe (better model fitting) how often a *"black swan event"* --- a low-score player beats a high-score player --- happens.
However, recall that the pdf of logistic distribution [^10] is defined as 

$$
f(t) = \frac{u}{s (1+u)^2}, u = e^{-(t-\mu)/s}.
$$

Its cdf is in a logistic function form: $$F(t) = \frac{1}{1+e^{-(t-\mu)/s}}$$.

The actually performance of player A and B is defined as random variable $$\alpha_A$$ and $$\alpha_B$$ respectively.

\begin{align}
\label{eq:awins}
\text{Pr}(\text{A wins}) 
    = \text{Pr}(\alpha_A > \alpha_B)
    = \int_{-\infty}^{+\infty} \text{Pr}(\alpha_A = a) \cdot \text{Pr}(\alpha_B<a) da 
    = \int_{-\infty}^{+\infty} f(a) da \int_{-\infty}^{a} f(b) db.
\end{align}

In Eq.\eqref{eq:awins}, $$f(\cdot)$$ is the pdf of the players' performance.
This equation relates to compute the pdf of a new random variable $$\alpha_C = \alpha_A - \alpha_B$$.
If both $$\alpha_A$$ and $$\alpha_B$$ conform normal distritribution, the resultant rv $$\alpha_C$$ will also follow a normal distribution [^11].
However, this is not the case for logistic distribution [^12] [^13]. So the answer to the question is **NOT** exactly the player's perfomance, i.e. $$\alpha$$.
But according to the close curve look of the norm and logistic distribution, we can assume that "the sum is approximately logistic" [^7] [^12].
By this approximation, we can roughly say that it is the player's performance that conforms logistic distribution.


### 2. Logistically Distributed Variable

If we go precise, the answer should be

> Elo assumes errors are distributed according to a logistic distribution [^14] 

or more accurately

> the difference in the random components of performance is logistically distributed [^9]

as is straightfoward from the equations.
Nevertheless, the *difference in randomness* is no longer intuitive for human to understand anymore. I would like to give an intuition that "the status of players to be both better or worse (than his normal self) is generally the same, e.g. high chance that both perfom worse due to the weather. 

## What's Next
Explanation of the usage in alignment tuning LLMs.

---
## Reference and Footnote

[^1]: W. X. Zhao et al., “A Survey of Large Language Models.” arXiv, Sep. 11, 2023. Accessed: Oct. 24, 2023. [Online]. Available: http://arxiv.org/abs/2303.18223

[^2]: A. Glaese et al., “Improving alignment of dialogue agents via targeted human judgements.” arXiv, Sep. 28, 2022. doi: 10.48550/arXiv.2209.14375.

[^3]: [League of Lengends wiki: Elo Rating](https://leagueoflegends.fandom.com/wiki/Elo_rating_system)

[^4]: The logistic function is modeled as $$p(x) = \frac{1}{1 + e^{\frac{x-\mu}{s}}}$$, where $$\mu$$ and $$s$$ controls the curve shifting and steep respectively (see [wiki](https://en.wikipedia.org/wiki/Logistic_regression) for more). And correspondingly, logistic regression is actually classification model that tries to learn parameters (normally in the format of $$p(x) = \frac{1}{1+e^{\mathbf{w}^\intercal x}}$$) of the logistic function model to correctly (in probability) classify the input into two different categories.

[^5]: [https://en.wikipedia.org/wiki/Logit](https://en.wikipedia.org/wiki/Logit)

[^6]: [The Mathematics of Elo Ratings: Calculating the relative skill of players in zero-sum games](https://www.cantorsparadise.com/the-mathematics-of-elo-ratings-b6bfc9ca1dba)

[^7]: [Kaggle MATH3311-Group4: Logistic Distribution on Elo Chess Data](https://www.kaggle.com/code/dancumming/math3311-group4)

[^8]: [Wikipedia: Black swan theory](https://en.wikipedia.org/wiki/Black_swan_theory)

[^9]: [FiveThirtyEight's Elo Ratings and Logistic Regression](https://nicidob.github.io/nba_elo/)

[^10]: [Wikipedia: Logistic distribution](https://en.wikipedia.org/wiki/Logistic_distribution)

[^11]: [Proof that the Difference of Two Jointly Distributed Normal Random Variables is Normal](https://srabbani.com/bivariate.pdf)

[^12]: [stackoverflow: how do i add variables with logistic distributions?](https://stackoverflow.com/questions/2823468/how-do-i-add-variables-with-logistic-distributions#:~:text=The%20sum%20of%20two%20logistic%20random%20variables%20does%20not%20have,normal%20random%20variables%20is%20normal.)

[^13]: [mathoverflow: explicit expressions of the distribution of sums of i.i.d. logistic random variables](https://mathoverflow.net/questions/157569/explicit-expressions-of-the-distribution-of-sums-of-i-i-d-logistic-random-varia)

[^14]: [stackexchange: What are the Elo formulas when assuming performance to be logistically distributed?](https://stats.stackexchange.com/questions/422243/what-are-the-elo-formulas-when-assuming-performance-to-be-logistically-distribut)
