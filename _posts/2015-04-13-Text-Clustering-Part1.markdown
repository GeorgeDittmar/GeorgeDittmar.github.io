---
layout: post
title:  "Text Clustering Part 1: A Document is Multi Dimensional!"
date:   2015-04-13 22:21:00
categories: Clustering, Machine Learning
---

This begins the first part of a series on clustering text and more specifically documents using simple features and algorithms. Later
in this series I might touch upon how to do this at a larger scale using Spark or Hadoop. The goal of this series is to get people familiar with
some basic Information Retrieval techniques that are applicable to many domains involving text extracting and learning simple models using textual
information. Part of this motivation also comes from a project I started a year ago to do simple text clustering of documents in plain text
using unsupervised learning techniques. [The TextClustering project](https://github.com/GeorgeDittmar/TextClustering) was a java project that I started a few years ago to build a framework to do
clustering of text and other natural language processing techniques. It hasnt moved as far as I would have liked due to other interests
and work taking up time, but I might make sure the last of the basic functionality works at least for the purpose of extending this blog.

## Vector Space Basics

To start off we need to get some terminology down and understand some underlying mathematics we will be using to ultimately create the document clusters.
First off we need to understand what a [Vector Space Model](http://en.wikipedia.org/wiki/Vector_space_model) is and some basic operations we will be performing on such a model.
The linked wiki explains vector space model fairly well but I will try to give a nice summary with an example or two that might make it more clear and concrete.

Say for instance we have two documents D<sub>1</sub> and D<sub>2</sub> which are just 4 word sentences. Our documents D<sub>1</sub> and D<sub>2</sub> can be represented as a vector of terms seen below. 

D<sub>1</sub> = (Cash,is,a,cool,dog)

D<sub>2</sub> = (Chase,puts,up,with,Cash)

A vector space model can be thought of as the space where all of these documents "live" given that each term from our document vectors is a dimension inside that space. So for instance
we could rewrite these vectors to be representative of what they should look like when living inside the vector space. We assume that these documents live within a common vector space
which means we can rewrite them to be a series of 0 or 1's if a term in the vector space is present in the document. So for example our documents then become represented as

Vector space terms = (a, cash, chase, cool, dog, is, puts, up, with)

D<sub>1</sub> = (1,1,0,1,1,1,0)

D<sub>2</sub> = (Chase,puts,up,with,Cash)


We treat the document vectors similar to how we would treat vectors in linear algebra which means we can use the same operations on them!
    

#### Term Vectors

First off lets define what a term vector is. Basically its a vector of terms. That's about it. Well its not THAT simple but its easy to think of it that way.
Say we have a document D and we want to get its term vector. Well the term vector of that document could be thought of as D_t = (w1,w2,w3,...,wn), where w1 through wn are all the words found
in that document upto word n. We want these vectors to contain non-zero count terms because it makes no sense to include a term that is not in D in its term vector. Now the values of these w_i elements
can be as simple as the counts of the number of times a word appears in the

## Information Retrieval Basics

This section covers some of the aspects of Information Retrieval that come into play when dealing with processing documents for machine learning.