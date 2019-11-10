# 数据预处理

1. Ignore cases with missing data, or weight the complete cases (i.e. give more importance to complete data and less importance to incomplete data). 
当有缺失的数据记录在整个数据中只占一个很小的比例时，可以直接删除缺失记录，对余下的完全数据进行处理。但在实际数据中，往往缺失数据占有相当的比重，尤其是多源数据。这时前述的处理将是低效率的，因为这样做丢失了大量信息，并且会产生偏倚，使不完全观测数据与完全观测数据间产生系统差异。
**Ignoring cases with missing data may be an option if you’ve got a large enough sample size. For smaller samples, every data point may be critical.**  

> 首先尝试使用推理插补法，即根据所的信息推断缺失值的方法进行插补。

2. Replace missing values with the mean or median for the set. Generally not recommended unless you have just a few missing values.   

> 均值插补方法只能在完全随机缺失（MCAR）机制下为总体均值或总体总量提供无偏估计。因为所有的插补值集中在均值点上，所以严重扭曲了数据分布，并在均值点上形成尖峰，导致低估估计量的方差。

3. Fill in the blank areas with zeros. Mostly an option if you have a few, non-critical missing points.

4. Use linear regression to fill in the blanks. Linear regression creates a simple model (a line) where it’s easy to extrapolate or interpolate missing values. 
> Only suited for data that is likely to be linear, like height, weight, or income levels.

5. Replace missing values with the value before it. This may work if your values seem to have a trend (as opposed to values that are all over the place).

6. Use a k-nearest neighbor or EM algorithm to generate missing data points. Nearest neighbor matching logically matches one data point with another, most similar, data point.
> 也说是热平台插补的一种.
The EM algorithm works by choosing random values for the missing data points, and using those guesses to estimate a second set of data. The new values are used to create a better guess for the first set, and the process continues until the algorithm converges on a fixed point.
> EM一个重要前提：适用于大样本。有效样本的数量足够以保证ML估计值是渐近无偏的并服从正态分布。但是这种方法可能会陷入局部极值，收敛速度也不是很快，并且计算很复杂。

Bayesian
C4.5

**7. Multiple Imputation**
* Fit your data to an appropriate model. Model fitting takes data from samples and attempts to find the best fit model, like a normal distribution or chi-square distribution. The model could also be some other parametric model gleaned from your data. For the above table, two simple models were created (giving two imputations): nearest neighbor, which took the values for the neighbor above and the neighbor below and nearest neighbor + 25%, which increased the nearest neighbor values to account for nonresponse bias.
* Estimate a missing data point using the selected model. For example, the nearest neighbor model might generate a 9 for missing value Y2. 9 is the value found in one of the nearest neighbors (Y1).
* Repeat steps 1 and 2 (you can use the same model, or different models) 2-5 times for each missing data point (this gives you multiple options for the missing data).  
![](https://www.statisticshowto.datasciencecentral.com/wp-content/uploads/2017/01/missing-data-2.png)   
* Imputations for the missing data points calculated from the two models (rounded to the nearest whole number).  
![](https://www.statisticshowto.datasciencecentral.com/wp-content/uploads/2017/01/completed-sets.png)   

* Perform your data analysis. For example, you might want to run a t-test, or an ANOVA. The test should be run across all missing data point sets. This example generated the four sets below, so your chosen tests would be run four times (once for each set).


Average the values of the parameter estimates, variances or standard errors obtained from each model to give a single point estimate for that model. In other words, you can combine the results from the two data sets generated from model 1, and you can also combine the results from the two data sets generated from model 2.
### 参考文献  
1. https://www.statisticshowto.datasciencecentral.com/multiple-imputation/
