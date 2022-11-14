---
layout: page
title: "Knowledge Inference for Optimizing Secure Multi-party Computation"
author: "Donghyun Sohn"
date: "2022/11/14"
tags:
  - DB
use_math: true
---

## Notice

This posting is my personal review of the paper named "[Knowledge Inference for Optimizing Secure Multi-party Computation](https://mhicks.me/papers/smc-knowledge.pdf)".<br>
Slides : [link](https://studylib.net/doc/14975978/knowledge-inference-for-optimizing-secure-multi-party-com...)


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

### SMC 
Secure multi-party computation (SMC) protocols enable a number of parties to coopereatively compute a function f over their private inputs in a way that every party directly sees only the output $f(x_1, ... , f_n)$ while keeping the variables $x_i$ private. <b>Initial implementations of SMC protocls compute $f$ using a single monolithic protocol, which can be very expensive.</b> A key observervation underlying many optimizations is that while SMC protocols typicall only reveal the final output to each party, <b>a party  may be able to infer the results of interemediate computations given the final output, their inputs and the function being computed</b>. When such inference is possible, the inferable intermediate results need not be cryptographically concealed. <b>Decomposing monolithic protocols into smaller protocols that explicitly reveal intermediate results can significantly improve the performance.</b> <br>
This paper explores <b>methods for inferring when and if variables can be inferred by SMC participants, and thus may enable protocol optimzations</b>. Specifically, we consider two related problems: <i>knolwedge inference</i> and <i>constructive knowledge inference</i>. A solution to the knowledge inference problem states which parties can learn which additional variables, if any, from a cooperative run of the unoptimized protocol. We call a knowledge inference solution constructive if, in addtion to correctly asserting that a party p knows a variable y, the solution also gives an evidence of party p's knowledge of y in the form of a program that computes y from p's private data and the final output.  

## Overview
In our setting, party $p$ knows the (deterministic) program (call it $S$), his own input set $I$, and his output $O$. We say party $p$ can infer the value of local variable $y ∈ S$ if there exists a function $F$ such that $y = F(I, O)$ in all runs of $S$. Another way of putting it is that no matter the values of $p$’s inputs or those of other participants of the SMC, $p$ can always compute $y$ given knowledge of only his inputs and the final result. Our goal to find all those variables in $S$ that $p$ can infer. We can do this by either showing merely that the required function $F$ exists, without saying what it is, or we can produce $F$ directly, thus constituting a constructive proof. In this paper we present approaches to both tasks.

### Knowledge inference
$\phi_{post}$ : soundly approximates the final state of the probram $S$ (that is, the final vaules of all program variables) for all possible program runs. We use program analysis called <b>symbolic execution</b>. Each feasible program path is characterized by a path condition $\psi_i$, which is a set of predicates relating the program variables. <br>


### Constructive Knoledge Inference
Knowledge inference can establish taht there exists some function $F$ such that $y = F(I,O)$, where $y$ is an intermediate variable in $S$, and $I$ and $O$ are variables known to party $p$. However, this technique cannot say what $F$ actually is. To construct $F$ we can leverage ideas from template based program verification. 

## Questions 
For a SMC program : <br>
- Can we infer which variables are known to a party ? 
- Can we infer an evidence for a party's knowledge of a variable ? 

## Contributions
- Formalization of knowledge 
  - Formalize what it means for a party $p$ to know a variable $x$
- Knowledge inference algorithm
  - Algorithm to infer if $p$ knows $x$
  - Proof of soundness and completeness
- Constructive knowledge inference algorithm
  - Algorithm to construct an evidence of $p$'s knowledge of $x$
  - Proof of soundness and completeness 
  
<img src = "./smc-knowledge/figure1.png" width = "300"> <br>
<img src = "./smc-knowledge/figure2.png" width = "300"> <br>
<img src = "./smc-knowledge/figure3.png" width = "300"> <br>
<img src = "./smc-knowledge/figure4.png" width = "300"> <br>
<img src = "./smc-knowledge/figure5.png" width = "300"> <br>
<img src = "./smc-knowledge/figure6.png" width = "300"> <br>
<img src = "./smc-knowledge/figure7.png" width = "300"> <br>
<img src = "./smc-knowledge/figure8.png" width = "300"> <br>
<img src = "./smc-knowledge/figure9.png" width = "300"> <br>
<img src = "./smc-knowledge/figure10.png" width = "300"> <br>
<img src = "./smc-knowledge/figure11.png" width = "300"> <br>
<img src = "./smc-knowledge/figure12.png" width = "300"> <br>
<img src = "./smc-knowledge/figure13.png" width = "300"> <br>
<img src = "./smc-knowledge/figure14.png" width = "300"> <br>
<img src = "./smc-knowledge/figure15.png" width = "300"> <br>
<img src = "./smc-knowledge/figure16.png" width = "300"> <br>
<img src = "./smc-knowledge/figure17.png" width = "300"> <br>
<img src = "./smc-knowledge/figure18.png" width = "300"> <br>
<img src = "./smc-knowledge/figure19.png" width = "300"> <br>