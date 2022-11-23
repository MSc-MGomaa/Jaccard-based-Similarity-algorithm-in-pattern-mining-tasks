# Jaccard-based Similarity algorithm in pattern mining tasks.
How to use a Jaccard-based similarity matrix to learn the set of patterns that covers the samples' space for a class of interest?

## Main Idea
<p align="justify">
The core idea of this approach comes from the fact that similar rules generally have similar evaluation values on a quality measure. This helped reshape the RuleSet configuration problem into a local optima rule search problem, where Jaccard similarity between two rules r1 and r2 can be defined as:

$$ Similarity (r1, r2) = { extent(r1) ∩ extent(r2) \over extent(r1) ∪ extent(r2)} $$

<p align="justify">
The similarity measure returns a value in between [0,1], zero to indicate the two rules are completely different, while one for the identical rules. With a predefined θ value (which is given as a hyperparameter) > 0, the neighborhoods of the current pattern are determined using the similarity equation.


## Jaccard-based Similarity main phases
<p align="justify">
In the graph below, the main idea of the Jaccard approach is explained. It consists of two main parts, the first is to sort the given pool of rules with respect to the given label in a descending order, so that the rule that best describes that label comes first in the resulting list. After calculating the positive and negative samples covered by that rule, a check is done to see if there are still examples ∈ the current label, that have not yet been covered. If there are still examples ∈ the current label not covered yet, then Jaccard′s similarity is calculated between that rule just added to the RuleSet and the next rules in the sorted list.

<p align="center">
<img width="700" height="350" src="https://github.com/MSc-MGomaa/Jaccard-based-Similarity-algorithm-in-pattern-mining-tasks/blob/c3cb8ca9cd661aab3986bb6a234881054633004f/result3.png">
</p>

### jaccardSimilarity(rule1, rule2) < θ

<p align="justify">
The first aim is to find the rule that has a similarity value less than the predefined θ to the current rule, where θ decides whether the two rules are similar or not. If the value between the two rules is greater than θ, then the two rules are similar. Otherwise, they are not similar. And the goal is to pick the first rule which is not similar to the current rule.

### Additional restrictions
<p align="justify">
It is not enough to find the rule which is not similar to the current best rule to be added to the RuleSet. For example, a rule might be dissimilar, but from the perspective that it covers many negative samples rather than positive samples. Even the positive examples it covers, could have been covered by the rules already in the RuleSet. Therefore, an additional condition was added, whereby a dissimilar rule is considered if it covers positive examples not covered by any of the rules already in the RuleSet. When this condition is added to the dissimilarity, it will be ensured that adding a new rule to the RuleSet will cover a portion of the search space that is not covered by any of the rules already in the RuleSet.

<p align="justify">
Once the rule with the previous constraints is found, the unique positive and negative samples covered by that rule are counted, then the loop is broken, to check if
there are still uncovered positive samples. In case, there is still, The last added rule becomes the current best rule and the process is repeated again. At the end, a rule set covering each positive sample from the given data set is returned.

## Experiments
When using Monte-Carlo Tree Search algorithm to explore the search space, it will retrieve a pool of patterns as exlained in
[Link](https://github.com/MSc-MGomaa/MCTS-For-Rule-learning). For a dataset consists of N classes, N pools of patterns are generated, each of which is generated with respect to a particular class. Now, the goal is to use each pool of rules to create a corresponding ruleset, Which covers every example belongs to that class. Now our aim is to use the Jaccard-similarity approach to do that task.

## An Example for the pattern-sets found, one for each class for "IRIS" dataset:

<p align="center">
<img width="700" height="300" src="https://github.com/MSc-MGomaa/Jaccard-based-Similarity-algorithm-in-pattern-mining-tasks/blob/c3cb8ca9cd661aab3986bb6a234881054633004f/result4.png">

From the results in the graph above, we can see that each class is represented with a set of patterns, NOT just single patterns as we did in [Link](https://github.com/MSc-MGomaa/MCTS-For-Rule-learning). Each set of patterns cover the sample space whcih represent each class, which we will use later to retrive a classifier.


