---
layout: post
title:  "Text Clustering Part 1: A Document is Multi Dimensional (A Mathy Description)"
date:   2015-05-16 22:21:00
categories: Clustering, Machine Learning
---

This begins the first part of a series on clustering text using simple features and algorithms. Later
in this series I might touch upon how to do this at a larger scale using Spark or Hadoop. The goal of this series is to get people familiar with
some basic Information Retrieval techniques that are applicable to many domains involving text extraction and learning simple models using textual
information. Part of this motivation also comes from a project I started a year ago to do simple text clustering of documents in plain text
using unsupervised learning techniques. [The TextClustering project](https://github.com/GeorgeDittmar/TextClustering) was a java project that I started a few years ago to build a framework to do
clustering of text and other natural language processing techniques. It hasn't moved as far as I would have liked due to other interests
and work taking up time, but I might make sure the last of the basic functionality works. 

There have also been a ton of other blogs all over the internet on this subject so this might not be anything new for anyone but I wanted to take it to a deeper 
understanding than most of the other ones I have read that just go over how to use Python and SciKit Learn to calculate document clusters. In the end I want to give
a clear intuition as to why this works beyond just the black box approach many people take to Machine Learning problems. This should help to see how these problems
can map to other problems you may come across and can be solved using similar techniques. This first post is a little math heavy but I hope to keep it to a minimum
so as to not overwhelm anyone and give just enough insight. Most of the math I cover are basic linear algebra topics, statistics and probability.

## 1 Vector Space Basics

To start off we need to get some terminology down and understand some underlying mathematics we will be using to ultimately create the document clusters.
First off we need to understand what a [Vector Space Model](http://en.wikipedia.org/wiki/Vector_space_model) is and some basic operations we will be performing on such a model.
The linked wiki explains vector space model fairly well but I will try to give a nice summary with an example or two that might make it more clear and concrete.

Say for instance we have two documents D<sub>1</sub> and D<sub>2</sub> which are just 4 words each. Our documents D<sub>1</sub> and D<sub>2</sub> can be represented as a vector of terms seen below. 

D<sub>1</sub> = (Cash,is,a,cool,dog)

D<sub>2</sub> = (Chase,puts,up,with,Cash)

A vector space model can be thought of as the space where all of these documents "live" given that each term from our document vectors is a dimension inside that space. So for instance
we could rewrite these vectors to be representative of what they should look like when living inside the vector space. We assume that these documents live within a common vector space
which means we can rewrite them to be a series of 0 or 1's if a term in the vector space is present in the document. Also the ordering of the terms is not being considered in this case so keep that in mind for future reference. 
Some other approaches to document clustering and classification do look at sentence structure and word orders but we are not covering that for now.

### 1.1 Term Vectors and Representations

So we now should understand the basics of what our problem space looks like and how we can conceptualize documents and the vector space they reside in. There is more that needs to be formalized though in how we represent that space and how to best represent our text documents. In the previous section we said documents D<sub>1</sub> and D<sub>2</sub> can be thought of as a vector of terms that live within the same vector space. One representation of the documents was to just store their actual string values in the vector and another representation was just to store a series of 0's and 1's depending on if a term in our vector space was present in the document itself. As well we added the stipulation that word ordering does not matter. This will help us define what is called the [Bag of Words Model](http://en.wikipedia.org/wiki/Bag-of-words_model) that we will use to help define formally our document representation.

The Bag of Words model doesn't take sentence structure/ordering into account to build a representation of the documents.
This is nice because it makes it easy to then generate our document vectors and compare other documents. Basically the previous section described loosely the bag of words model, where each document is just an unordered vector of terms that
either are present or not.
However this does not tell us much else about a document besides if certain words are present or not. We can learn from the presence or absence of words in a document, but wouldn't it be more powerful to not only know if a word is there but
how much value that word has to the document it is representing. Thus the concept of term weighting comes into play.

One way that terms could be weighted is by their frequency in the document. The intuition comes from the idea that
if a term happens in that document many times it must have higher weight for that class of documents. However there is a flaw in this logic when you consider terms such as "a" and "the" These terms are very common to the english language and most likely appear very often in
many different kinds of texts across multiple document classes. One way to deal with this is to remove out terms that are just too common and do something that is called [TF-IDF](http://en.wikipedia.org/wiki/Tf%E2%80%93idf) weighting which is Term Frequency-Inverse Document Frequency. The idea being that a term that appears many times in one document but not that frequently in the whole corpus of documents should be more important to classifying that document. This should make it easier to differentiate between documents that are talking about medical procedures and ones that are talking about fashion for example.

<center>
$$
\begin{equation}
	TF-IDF_{t,d_i} = \frac{|t|}{|d_i|} \log{ \frac{N}{df_t}}
 	\label{eq:tf}
\end{equation}
$$
Term frequency Inverse Document Frequency formula
</center>
<br />

Where $$N$$ is the number of documents in the collection and $$ \vert df_t \vert $$ is the number documents that contain the term $$t$$. The $$TF$$ equation is what is also known as the normalized term frequency so that we do not let bias enter into the equation with longer documents.
And since we can represent the terms of a document as a series of $$tf-idf$$ scores, then we can represent the document as a vector of term weights which then lets us determine similarities and other cool mathy things about text.
<center>
$$
\begin{equation}
	Doc = (.01, .5, .0025 , ... , .003)
\end{equation}
$$
NOTE: these values are made up and poorly chosen :p
</center>

## 2 How to tell similarity between vectors?

So now that we have some of the essential mathematics out of the way we need to cover one more area so we can start doing some basic work with our documents.
One way to determine document similarity is seeing where our documents end up in our vector space model. Its intuitive to think that given documents with similar terms and weights
that they would end up in similar areas of the n-dimensional space. To calculate the similarity between different document vectors we will use whats called the [cosine similarity](http://en.wikipedia.org/wiki/Cosine_similarity) measure.

To calculate this measure we take the dot product between two vectors in the numerator and the denominator is the euclidean product of the vectors length. This will give us a value between 0 and 1 on how similar
any document is from another document.

<center>
$$
\begin{equation}
cosineSim(d_i,d_j) = \frac{ d_i \cdot d_j } {\vert d_i \vert  \vert d_j \vert}
\end{equation}
$$
Cosine Similarity between documents i and j
</center>

## Conclusion

So with all this math out of the way you should be able to understand some of the reasoning's behind how clustering can work to classify texts. Does this cover all there is to know
about document clustering? Hell no! I only know about this much right now and even then I keep needing to go over it again. The next part will be the application of all this to clustering
book genre's. Now granted there are better ways to learn topics out there such as LDA, but the point of the next few posts are to discuss clustering