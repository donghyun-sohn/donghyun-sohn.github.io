---
layout: page
title: "Differential Privacy for Relational Algebra: Improving the Sensitivity Bounds via Constraint Systems"
author: "Donghyun Sohn"
date: "2022/10/24"
tags:
  - DB
use_math: true
---

## Notice

This posting is my personal review of the paper named "[Differential Privacy for Relational Algebra: Improving the
Sensitivity Bounds via Constraint System](https://arxiv.org/pdf/1207.0872.pdf)".<br>
Slides of the paper : [Slides](http://www.lix.polytechnique.fr/~stronati/talks/apvp12.pdf)


# Personal Review

## Discussion with my advisor
Weakness of this paper is that they did not really implement this work. They did not give us an experimental results. <br>
This is little dated, but this is a good first step of how do we start attack this problem.  <br>
This is a good step towards analyizng the operators one at a time, but they did not really compose it into an end-to-end system, which can be weakness. 

## Summary
There is trade-off between the privacy and utility. 
To meet the desired level of differential privacy while keeping efficiency as high as possible, it is important to avoid adding excessive noise. <br>
It is desirable that the computation of the sensitivity is done statically for the efficiency, but we cannot compute the precise sensitivity. We can only approximate it. The goal of this paper is to explore a constraint-based methodology in order to compute strict bounds on the sensitivity. <br>

## Strengths
It suggests methodology that used constraint so that we can compute more efficiently. For example, they showed check constraints to improve the senstivity bounds <br>

## Weakness
usually focused on check constraints. <br>


# Information Organization

## Background 
### Sensitivity
Reference : [https://becominghuman.ai/query-sensitivity-types-and-effects-on-differential-privacy-mechanism-c94fd14b9837](https://becominghuman.ai/query-sensitivity-types-and-effects-on-differential-privacy-mechanism-c94fd14b9837) <br>

DP adds statistical noise to the data for better privacy, but How much noise should we add? It depends on the 4 factors
1. Sensitivity of the Query
2. Desired Epsilon
3. Desired Delta
4. Type of Noise to be added (most commonly used - Gaussian Noise or Laplacian Noise, among which Laplacian noise is easy to use)

Sensitivity parametrize the amount, how much noise perturbation is required in the DP mechanism. To determine the sensitivity, the maximum of possible change in the result needs to be calculated. <br>

The sensitivity of a query can be stated both in the global and local context:
- Global Sensitivity refers to the consideration of all possible data sets differing in at most one element
- Local Sensitivity is the change in one data set with differing at most one element

### Constraints
Reference : [https://www.mygreatlearning.com/blog/sql-constraints/](https://www.mygreatlearning.com/blog/sql-constraints/)
1. Unique Constraints
    - No duplicate values are entered in specific columns that do not participate in a primary key
2. Check Constraints
    - Enforce domain integrity by limiting the values that are accepted by one or more columns
3. Not Null Constraint
4. Default Constraint
    - Specify a default value that is to be entered in any record in a particular column
5. Primary Key Constraint
    - Guarantee for uniqueness for a column or set of columns 
6. Foreign Key Constraint
    - used to prevent operations in a relational database that would destroy links between tables. I.E. if there is no value in the primary key table, such value cannot be inserted into the foreign key table 
7. Index Constraint
    - By creating an index, the performance of data retrieval queries can be greatlyl improved


## Diffrential Privacy
Diffrential privacy is the approach addressing the privacy of individuals in data analysis on statistical databases. Querying the database can disclose someone's identity. To avoid this problem, one of the most commonly used methods consists in introducing some noise on the answer. Instead of giving the exact answer the curator gives an approximated answer, chosen randomly according to some probability distribution. <br>
Differential privacy measures the level of privacy provided by such a randomized mechanism by a parameter $\epsilon$: a mechanism $K$ is $\epsilon$-differentially private if for every pair of adjacent databases $R$ and $R'$(i.e. databases which differ for only one entry), and for every property $p$, the probabilities that $K(R)$ and $K(R')$ satisfy $P$ differ at most by the multiplicative constant ${e}^\epsilon$. <br>
<b><u>The amount of noise that the mechansim must introduce in order to achieve $\epsilon$ differential privacy</u></b> depends on the so-called <b><u>sentivity of the query</u></b>, namely <b><u>the maximum distance between the answers on two adjacent databases</u></b>. <br>

The more noise a mechanism adds, the less precise the reported answer. <br>
Therefore, it is important to avoid adding excessive noise: one should add only the noise strictly necessary to achieve the desired level of differential privacy. <br>
For the sake of efficiency it is desirable that the computation of the sensitivity is done statically. This implies that we cannot compute the precise sensitivity, but only approixmate it from above. The goal of this paper is <u>to explore a constraint-based methodology in order to compute strict bounds on the sensitivity</u>. <br>
The lanuage we chose to conduct our analysis is relational algebra. <u><b>Constraints in RDBMS can be defined on single attributes (coluumn constraints), or on several attributes (table constraints), and help define the structure of the relation, for example by stating whether an lattribute is primary key or a reference to an external key</b></u>.  

### Contribution 
1. Propose a method to compute a bound on the sensitivity of a query in relational algebra in a compositional way
2. Propose the use of constraints and constraint solvers to refine the method and obtain strict bounds on queries which have aggregation functions at the top level

## Preliminaries
- Differentially private queries can only return a value, and for this reason they must end with an aggregation (operator $\gamma$)
- Differentially private mechanisms are usually obtained by adding some random noise to the result of the query
  - The best results are obtained by calibrating the noise distribution according to the so-called sensitivity of the query 

## Differential privacy on arbitrary metrics
- In order to compute the senstivity bounds in a compositional way, we need to cope with different structures at the intermediate steps, and with different notions of distance

## Sensitivity of Row operators
1. Union : 2
2. Intersenction : 2
3. Difference : 2

## Sensitivity of Attribute operators
1. Projection : 1
2. Cartesian product : 
3. Restricted : 

<b>Exploiting the constraints generated by the query can lead to a significant reduction of the sensitivity</b>


