---
layout: post
title: Naive Bayes From Scratch
comments: false
---

As both the title of this blog post and name of this algorithm suggest, this post will be discussing Naive Bayes, a simple, and powerful classification algorithm based on [Bayes Theorem](https://en.wikipedia.org/wiki/Bayes%27_theorem).

Technically Bayes Theorem is written as:

![](/assets/bayes.svg)

Where P(A | B) is the likelihood of event A occurring given that B is true.

In layman's terms, Bayes Theorem tells us that we must include a prior probability of a certain condition (class). Put another way, we are trying to make predictions of a label (or class) given the presence of some series of features, *as well as* the likelihood the label exists.

**Assuming a set of features (columns)...**

The following links provide a more thorough (read: better) explanation of Bayes Theorem and Naive Bayes.
- ["Naive Bayes classifier" - Wikipedia](https://en.wikipedia.org/wiki/Naive_Bayes_classifier)
- ["Naive Bayes for Machine Learning" - Jason Brownlee](https://machinelearningmastery.com/naive-bayes-for-machine-learning/)
- ["In Depth: Naive Bayes Classification" - Jake VanderPlas](https://jakevdp.github.io/PythonDataScienceHandbook/05.05-naive-bayes.html)
- ["Naïve Bayes Algorithm: Everything you need to know - KDnuggets"](https://www.kdnuggets.com/2020/06/naive-bayes-algorithm-everything.html)
- ["An Intuitive (and Short) Explanation of Bayes' Theorem - BetterExplained](https://betterexplained.com/articles/an-intuitive-and-short-explanation-of-bayes-theorem/)

Naive Bayes is often used as a simple, baseline model as it is quick to implement - no lines to fit or neural networks to optimize. 

However with this speed comes a tradeoff. The algorithm is called "naive" as the probability for each feature is considered independent of one another. This allows for quick calculations of probability, but is a strong assumption that is unlikely to hold across typical data.

### My Implementation From Scratch

*You can see my source code, [here](https://github.com/michael-rowland/naive-bayes-from-scratch).*

I have found the best way for me to understand how an algorithm works "under the hood", is to try and understand its component parts and code the algorithm from scratch. This is my attempt to do that with Naive Bayes. 

This implementation is basic and limited, and is not an attempt to replace other open-source implementations, such as [scikit-learn](https://scikit-learn.org/stable/modules/naive_bayes.html). 

My implementation class consists of two primary methods: `fit` and `predict`.

First the `fit` method:

```
def fit(self, X, y):
        # calculates prior/conditional probabilities for each classification
        self.priors = self.prior_probabilities(y)
        self.conds = self.conditional_probabilities(X, y, self.k)
```

As the `fit` method simply trains the model, and doesn't return anything, we simply need to establish two variables:
- `priors`: the prior probability of seeing each class (P(A) or P(B) from the definition above). This is stored as a dictionary.
- `conds`: the conditional probabilities for each feature (word, in my case) given each class. These are also stored a dictionary. So if a feature, say for example the word "hello", is more common in class 1, than class 2, `self.conds` will store this relationship.

Next, the `predict` method (and associated `class_probability` helper function):

```
def class_probability(self, word_counts, cond_probs, prior):
        # multiplies the two lists by each other, sums them, multiplies by prior
        products = [a*b for a, b in zip(word_counts, cond_probs)]
        return np.log(sum(products) * prior)
        
def predict(self, word_counts):
    self.results = {}
    for i, probs in self.conds.items():
        self.results[i] = self.class_probability(
            word_counts,
            probs,
            self.priors[i]
        )
    return max(self.results, key=self.results.get)
```

Finally, using the dictionaries we defined in the `fit` method, we iterate over all classes and the associated prior and conditional probabilities, and use the `class_probability` function to assign a score to each class. We then return the max of these scores. The `get_proba` method is available to access all class probabilities, if necessary.

### Scikit-Learn Implementation

Throughout this process, as has been the case numerous times before, I have come away impressed and grateful to have a free, fast, high-quality, and open-source implementation of this algorithm. Scikit-learn consistently proves to be a fast, reliable, and well documented.

When comparing my implementation, to the scikit-learn variation, a few things become clear. 
- **Speed**: scikit-learn is considerably faster, running a on the "spam" dataset in 0.29 seconds, compared to my implementation of 9.32 seconds.
- **Math**: my algorithm does simple multiplication (dot-product), however, while scikit-learn may do something similar, it is clear when looking at the probability calculations, the underlying math is not identical. It should be noted that for the smaller dataset, the final classification was correct, only the underlying probability was different.
- **Model Variations**: my algorithm was a simple multinomial implementation, however scikit-learn provides implementations for multinomial, Gaussian, Bernoulli, and others, covering a larger cross section of potential data.

While this exercise was helpful in my understanding of this algorithm, the power and ease-of-use scikit-learn provides makes it an easy choice for future development.

I hope by seeing some code, and walking through my thought process was in some small way helpful in your understanding of Naive Bayes.

### Further Development

- It should be noted that, while my algorithm was successful across the small datasets, when applied to the larger, spam dataset, as there was a larger corpus of words, many data points on a single row were 0. Despite the basic attempts at smoothing (using psuedocounts) in my code, the results for all classes were not close to the scikit-learn implementation. My best guess is this is that scikit-learn does some "behind-the-scenes" smoothing to avoid multiplying several numbers by probabilities close to 0.
- Research scikit-learn source code, see how to handle large feature-sets
- Gaussian implementation and understanding