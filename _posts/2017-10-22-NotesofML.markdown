---
layout: post
title: Notes for ML
date: 2017-10-22
categories: Note
comments: true
---

Some old notes with ML class, TBD to pick up again. (who knows when)


# Intro of week 1

## Supervised Learning
Already know what is right answer, what does result looks like. Trying to find if there is a relationship between input and output.

1. regression problem 

We are trying to predict a result in a continuous output. In other words, we are trying to map input variable to some continuous function.

Given data about the size of houses on the real estate market, try to predict their price. Price as a function of size is a continuous output, so this is a regression problem. 

2. classification algorithm

Asked to predict results in a discrete output. In other words, we are trying to map a input variable into a discrete categories.

SVM support vector machine

e.g.
(a) Regression - Given a picture of a person, we have to predict their age on the basis of the given picture

(b) Classification - Given a patient with a tumor, we have to predict whether the tumor is malignant or benign.

## Unsupervised learning
Input are not labeled. There is no feedback based on prediction results.

1. clustering algorithm

e.g. Google news cluster together as results. Understanding genes

Application: organize computing clusters, social network analysis, market segmentation, astronomical data analysis.

2. non-clustering
Cocktail party problem, to find structure in a chaotic environment.


# Model representation

hypothesis: function take input and output estimate value

## Linear regression

Cost function: The difference between the predicted value and the actual value.

choose different parameters. Also called square error function or mean squared error. This takes an average difference (actually a fancier version of an average) of all the results of the hypothesis with inputs from x' s and the actual output y' s. Goal is to minimize cost function.

Use of cost function:
Different theta is a different prediction. Objective function J(theta0, theta1) want to find a value that minimize cost.

Contour graph / plot: instead of showing as 3D graph, in same line share a parameter, a point represent a pair of parameters.

