---
layout: page
title: "MIT 6.875 - Lecture 2"
author: "Donghyun Sohn"
date: "2022/4/20"
tags:
  - cryptography
use_math: true
---

### Notice

This posting is based on Prof. Vinod Vaikuntanathan's <b>MIT 6.875 Foundations of Cryptography (Fall 2021)</b> lecture. <br>

Lecture link : [http://mit6875.org](http://mit6875.org)

Reference : [The Joy of Crpytography](https://joyofcryptography.com) by Mike Rosulek
  <br><br>

In the last lecture, we learned that oen-time pad is a perfectly secure scheme, but it has limitation that keys need to be equal or longer than messages. Therefore, we relax the definition that EVE is an arbitrary computationally bounded algorithm. <br>

This lecture, we start from how can we overcome Shannon's conundrum. 
#### Perfect Indistinguishability: a Turing test
<img src = "./lecture_2/figure1.png" width = "500">
Before relaxing the definition, let's recall the perfect indistinguishability. <br>
In the blue box definition, you pick a random key from key space, encrypt the m0 with key k to c, give EVE c, and see what is the chance for EVE guess 0. (0 means that you guess you are in world 0) <br>
Second experiment is exactly same, but encrypt m1. And the probability of two experiement is same.  
We can change the definition as follows. 
<img src = "./lecture_2/figure2.png" width = "500">
Pick a key from key space, pick random bit from {0,1} meaning which world that I am going to in, 
and make EVE to guess which world she is in. <br>
This should be half. <br>
This is what we are now, and <b>we want to change this definition. </b>


#### Computationally Bounded Adversaries

#####  The Axiom of Modern Crypto

p.p.t. = probabilistic polynomial-time<br>
Alice and Bob are fixed p.p.t. algorithms. <br>
Eve is any p.p.t. algorithm that we have no control. <br>

Given this axiom, we can reformulate the definition. 

#####  Computational Indistinguishability (take 1)
<img src = "./lecture_2/figure3.png" width = "500">
Before, it was for <b>all</b> EVE, but now it is for all <b>p.p.t.</b> EVE.  <br>
This is still subject to Shannon's impossibility. <br>
You cannot have encryption scheme that satisfies this definition and encrypts n+1 bit message, given an n bit key. <br>
Why? <br>
<img src = "./lecture_2/figure4.png" width = "500">
It is slightly larger than half. <br>
<b>We insisted on the adversary having no advantage better than a half. </b> <br>

What should we do next ? <br>

##### Negligible Functions
Functions that grow slower than $1/p(n)$ for any polynomial p. 

<img src = "./lecture_1/figure5.png" width = "500">

Good property of negligible function : If q(n) be a negligible function and p(n) a polynomial function, then q(n)*p(n) is a negligible function

##### Security Parameter : n (sometimes λ)
<img src = "./lecture_1/figure6.png" width = "500">
n is input to all these algorithms. 

#####  Computational Indistinguishability (take 2)

<img src = "./lecture_1/figure7.png" width = "500">

The probability that every p.p.t. EVE guesses the world correctly is at most half + negligible function in n. 
