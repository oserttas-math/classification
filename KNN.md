K-Nearest Neighbors
================

Now we implement the KNN on Smarket data.

``` r
library(ISLR)
attach(Smarket)
train = (Year<2005)
Smarket.2005 = Smarket[!train,]
Direction.2005 = Direction[!train]
```

We will use the knn() function, which is part of the class library. This function works rather differently from the other modelfitting functions that we have encountered thus far. Rather than a two-step approach in which we first fit the model and then we use the model to make predictions, knn() forms predictions using a single command.

The function requires four inputs:

1.  A matrix containing the predictors associated with the training data, labeled train.X below.
2.  A matrix containing the predictors associated with the data for which we wish to make predictions, labeled test.X below.
3.  A vector containing the class labels for the training observations, labeled train.Direction below.
4.  A value for K, the number of nearest neighbors to be used by the classifier.

We use the cbind() function, short for column bind, to bind the Lag1 and Lag2 variables together into two matrices, one for the training set and the other for the test set.

``` r
library(class)
train.X = cbind(Lag1, Lag2)[train,]
test.X = cbind(Lag1,Lag2)[!train,]
train.Direction = Direction[train]
```

Now the knn() function can be used to predict the market’s movement for the dates in 2005. We set a random seed before we apply knn() because if several observations are tied as nearest neighbors, then R will randomly break the tie. Therefore, a seed must be set in order to ensure reproducibility of results.

``` r
set.seed(1)
knn.pred = knn(train.X, test.X, train.Direction, k=1)
table(knn.pred, Direction.2005)
```

    ##         Direction.2005
    ## knn.pred Down Up
    ##     Down   43 58
    ##     Up     68 83

``` r
(83+43)/252
```

    ## [1] 0.5

The results using K = 1 are not very good, since only 50% of the observations are correctly predicted. Of course, it may be that K = 1 results in an overly flexible fit to the data. Below, we repeat the analysis using K = 3.

``` r
knn.pred = knn(train.X, test.X, train.Direction, k=3)
table(knn.pred, Direction.2005)
```

    ##         Direction.2005
    ## knn.pred Down Up
    ##     Down   48 54
    ##     Up     63 87

``` r
mean(knn.pred == Direction.2005)
```

    ## [1] 0.5357143

The results have improved slightly. But increasing K further turns out to provide no further improvements. It appears that for this data, QDA provides the best results of the methods that we have examined so far.
