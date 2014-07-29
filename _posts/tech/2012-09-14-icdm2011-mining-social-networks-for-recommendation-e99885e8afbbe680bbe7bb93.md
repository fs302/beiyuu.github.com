---
layout: post
title: Mining Social Networks for Recommendation 阅读总结
category: tech
description: Recommender systems
---
## Recommender systems





	
  * Web Search:

	
    * need contents

	
    * can not feed to users' different interests




	
  * Data:

	
    * User action

	
    * User profile




	
  * Tasks:

	
    * Rating prodiction

	
    * Top-N recommendation

	
    * Link recommendation(only if social network)




	
  * Privacy Issues

	
    * More data,better recommendation

	
    * User need be able to set choice




	
  * Collaborate Filtering

	
    * Nearest Neighbor-based approach

	
      * User-based

	
      * Item-based




	
    * Model-based approch

	
      * MF(Matrix Factorization)

	
        * Outperforms the NN-based CF










	
  * Content-based

	
    * Keywords

	
      * TF-IDF







	
  * Hybrid recommander System

	
    * Approach 1: combine separate recommenders Combine results

	
      * e.g. using linear combination or voting[Pazzani 1999]




	
    * Approach 2: add aspects of content-based method to CF. [Pazzani 1999]

	
    * Approach 3: add aspects of CF to content-based method [Soboroff et al., 1999] .

	
    * Approach 4: Unified recommendation model

	
      * E.g., combine topic model, i.e. Latent Dirichlet Allocation, with MF.




	
    * Latent Dirichlet Allocation (LDA) [Blei et al., 2003]

	
    * Graphical Model

	
    * Collaborative topic regression [Wang et al., 2011]




	
  * Performance Evaluation

	
    * Cross-validation on offline dataset  


	
      * Limitations:


	
        * Measures only accuracy of recommendation

	
        * Does not measure other aspects such as diversity

	
        * Does not measure how recommendation change user behavior

	
        * this is the ultimate goal of a recommender!





	
    * In industry

	
      * Want to evaluate user satisfaction and business profit

	
      * A/B test in online system

	
      * Evaluate Measures:

	
        * click-through rate usage

	
        * return rate of customers profit










	
  * Challenges:

	
    * Cold start problem

	
      * for Users

	
        * Typically,~50% of users cold start

	
        * CF fails-there are no similar users(User-based) and no Item rating to aggregate




	
      * for Items

	
        * CF fails

	
        * Content based methods works













## <!-- more -->Recommendation in social networks





	
  * Formation and Evolution are affect by many effectsTrust network

	
    * Self-interst

	
    * Social and resource exchange

	
    * Balance

	
    * Homophily

	
    * Proximity




	
  * Social rating network

	
    * users are associated with item ratings

	
    * Social influence:

	
      * ratings are influenced by ratings of friends

	
      * ratings are influenced by ratings of actorswith similar ratings




	
    * Benifit:

	
      * Exploit social influence  


	
        * corelational,influence,transitivity,selection




	
      * Can deal with code start problem

	
      * Are more robust to fraud, in particular to profile attacks




	
    * Challenges

	
      * Low probability of finding rater at small network distance

	
      * Noisy ratings at large network distances

	
      * Social network data is very sensitive

	
      * Edges in online social networks are of greatly varying reliability / strength?










## Mining social networks





	
  * Type

	
    * Analysis of social influence

	
    * Models of social rating networks

	
    * Inference of social networks







## Memory based approaches





	
  * Problem defination

	
    * Input

	
      * rating matrix

	
        * real value or binary




	
      * social matrix

	
        * weighted or binary










	
  * Datasets for Recommandation in SNsapproaches

	
    * Epinions-Online product review

	
      * Users review and rate products in differentcategories

	
      * Users express trust on other reviewers

	
      * URL:

	
        * http://www.trustlet.org/wiki/Epinions_dataset

	
        * http://alchemy.cs.washington.edu/data/epinions/







	
    * Flixster

	
      * Social metworking service for rating movies

	
      * http://www.sfu.ca/~sja25/datasets/







	
  * approaches


	
    * Explore the social network for raters

	
    * Aggregate the ratings to compute prediction

	
    * Store the social rating network

	
    * No Learning phase

	
    * Slow in prediction

	
    * Most pioneer works for recommendation in SN are memory based approaches.





## Model based approaches





	
  * approaches


	
    * Learn a model

	
    * Store the model parameters only

	
    * Extra time for learning

	
    * Fast in Prediction

	
    * Most models are based on matrix factorization





## Link prediction





	
  * Problem defination

	
    * Given a user pair (u,v),estimate the probability of creation of the link u->v.

	
    * Given a user u, recommend a list of top users for u to connect to.




	
  * Finding similariry

	
    * cosine similarity

	
    * Pearson corelation

	
    * Jaccard's coefficient







## Social networks with distrust





	
  * Few works have addressed negative relations


	
    * [Leskovec et al., 2010]

	
    * [Kunegis et al., 2009]

	
    * [Brzozowski et al., 2008]

	
    * [Guha et al., 2004]




	
  * Prior work shifted the trust to avoid negative values




## Summary





	
  * State-of-the-art methods for recommendation in social networksLink Prediction

	
    * Memory based approaches

	
      * ModelTrust [Massa 2007], Modified BFS

	
      * TidalTrust [Golbeck 2005], Modified BFS

	
      * TrustWalker [Jamali et al., 2009], Random Walk




	
    * Model based approaches

	
      * SoRec [Ma et al., 2008], Matrix Factorization

	
      * FIP [Yan et al., 2011], Matrix Factorization

	
      * STE [Ma et al., 2009], Matrix Factorization

	
      * SocialMF[Jamali et al., 2010], Matrix Factorization

	
      * GSBM[Jamali et al., 2011], Stochastic BlockModel







	
  * Link prodiction


	
    * Pair-wise profile similarity approaches


	
      * Information theoretic based definition of similarity


	
    * Network topology based approaches


	
      * Common neighbors


	
    * Path based approaches


	
      * Katz, Hitting time, RWR, SimRank



	
  * Social Networks with distrust

	
    * Propagation of distrust

	
    * Theories behind distrust

	
    * Recommendation with distrust [Ma et al., 2009.b]






	
  * Future Research Directions

	
    * Exploring other machine learning models

	
    * Privacy of recommendation in social networks

	
      * How to preserve privacy while employing social networks?




	
    * Improving the diversity of recommendations

	
      * How to evaluate the diversity?




	
    * Recommendation of cold-start items

	
      * They are very important!




	
    * Recommendation in mobile social networks

	
      * Distributed algorithm

	
      * How to exploit the user location?




	
    * Recommendation in social networks with documents (posts)

	
      * E.g., Twitter

	
      * Integration with topic models








