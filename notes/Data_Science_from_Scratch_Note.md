##Data Science from Scratch Note

###Statistic

####Describe The dataset

- min: minimum value of the dataset
- max: maximum value of the dataset
- mean: sum of the dataset divided by dataset's size
- median: if dataset's size is odd, median is the middle-most value. if it's even, median is the mean of the two middle-most values
- quantile: the generalization of median, value less than a certain percentile of the dataset.
- mode: most-commom values, most frequent values
- dispersion: data range = max - min
- variance: 

  x is a discrete variable:  $\frac{1}{n-1} \sum_{i=1}^{n}{(x_i - x_{mean})^2}$

- standard deviation: 

  x is a discrete variable:  $\sqrt{\frac{1}{n-1} \sum_{i=1}^{n}{(x_i - x_{mean})^2}}$

- interquartile_range: quantile(0.75) - quantile(0.25)

####Correlation

###Probability Theory

####Variable

1. independent vs dependent: 
    v1 gives no information about v2, then v1 and v2 are independent
    
    `P(E, F) = P(E)P(F)` E、F are independent
    
    `P(E|F) = P(E, F)/P(F) P(E, F) = P(E|F)P(F)` E、F are dependent


####Bernoulli Distribution(伯努利分布、两点分布、0-1分布)

