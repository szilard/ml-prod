
## Machine Learning in Production

This repo is intended to stimulate discussions about how to use machine learning in 
production (system components, processes, challenges/pitfalls etc.).
It may lead to some kind of "best practices" blog post or paper eventually. 
Discussions/comments are welcome from anyone via github issues.



## Initial ideas/outline:

The following figure summarizes the components/processes involved:

![img](https://raw.githubusercontent.com/szilard/MLprod-1slide/master/MLprod-1slide.png)



### Historical data

Can be in database, csv file(s), data warehouse, HDFS



### Feature engineering

On typical structured/tabular business data it can involve joins and aggregates (e.g. how many clicks from
a given user in given time period)

This "ETL" is heavy processing, not suited for operational systems (e.g. MySQL), usually
in "analytical" database (Vertica, Redshift) or maybe Spark

Figuring out good features is trial-error/iterative/researchy/exploratory/time consuming (as in general
the whole upper part of the Figure above, i.e. FE, model training and evaluation)

Categorial variables: some modeling tools require transformation to numeric (e.g. one-hot encoding)



### Training, tuning

The result of feature engineering is a "data matrix" with features and labels (in case of supervised
learning)

This data is usually smaller and most often does not require distributed systems 

The algos with best performance are usually: gradient boosting (GBM), random forests, 
neural networks (and deep learning), support vector machines (SVM)

In certain cases (sparse data, model interpretability required) linear models must be
used (e.g. logitic regression)

There are good open source tools for all this (R packages, Python sklearn, xgboost, VW, H2O etc.)

The name of the game is avoid overfitting (and techniques such as regularization are used)

Also need unbiased evaluation, see next point

Models can be tuned by search in the hyperparameter space (grid or random search, Bayesian optimization methods etc.)

Performance can be increased further by ensembling several models (averaging, stacking etc.)



### Model evaluation

This is super-important, spend a lot of time here

Unbiased evaluation with test set, cross validation (some algos have "early stoping" requiring a validation set)

If you did hyperparameter tuning, that also needed a separate validation set

The real world is non-stationary, also use a time gapped test set

Diagnostics: distribution of probability scores, ROC curves etc.

Also do evaluation using various business metrics (impact of model in business terms)



### Model deployment

Scoring of live data

Considered usually an "engineering" task (thrown over a "wall" from data scientists to software engineers)

Use same tool to deploy, do not rewrite in other "language" or tool (SQL, PMML, Java, C++, custom
format such as JSON) (unless the export is done by the same tool/vendor doing the training)

Different servers (train require more CPU/RAM; score require low latency, high-availability, maybe
scalability)

Live data comes from different system, often FE needs to be replicated (approximately duplicate code is evil,
but may be unavoidable); transformations/data cleaning already in the historical data might need to be
replicated here as well

Scoring can be batch (easier, can read from database, score and write result to database) or
real-time (the modern way to do it is via http REST API)



### Taking action






### Evaluate & monitor



### Learn & improve







