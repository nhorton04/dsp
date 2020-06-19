[Think Stats Chapter 6 Exercise 1](http://greenteapress.com/thinkstats2/html/thinkstats2007.html#toc60) (household income)

The distribution of income is famously skewed to the right. In this exercise, we’ll measure how strong that skew is.
The Current Population Survey (CPS) is a joint effort of the Bureau of Labor Statistics and the Census Bureau to study income and related variables. Data collected in 2013 is available from http://www.census.gov/hhes/www/cpstables/032013/hhinc/toc.htm. I downloaded `hinc06.xls`, which is an Excel spreadsheet with information about household income, and converted it to `hinc06.csv`, a CSV file you will find in the repository for this book. You will also find `hinc2.py`, which reads this file and transforms the data.

The dataset is in the form of a series of income ranges and the number of respondents who fell in each range. The lowest range includes respondents who reported annual household income “Under \$5000.” The highest range includes respondents who made “\$250,000 or more.”

To estimate mean and other statistics from these data, we have to make some assumptions about the lower and upper bounds, and how the values are distributed in each range. `hinc2.py` provides `InterpolateSample`, which shows one way to model this data. It takes a `DataFrame` with a column, `income`, that contains the upper bound of each range, and `freq`, which contains the number of respondents in each frame.

It also takes `log_upper`, which is an assumed upper bound on the highest range, expressed in `log10` dollars. The default value, `log_upper=6.0` represents the assumption that the largest income among the respondents is $10^6$, or one million dollars.

`InterpolateSample` generates a pseudo-sample; that is, a sample of household incomes that yields the same number of respondents in each range as the actual data. It assumes that incomes in each range are equally spaced on a `log10` scale.

    def InterpolateSample(df, log_upper=6.0):
        """Makes a sample of log10 household income.

        Assumes that log10 income is uniform in each range.

        df: DataFrame with columns income and freq
        log_upper: log10 of the assumed upper bound for the highest range

        returns: NumPy array of log10 household income
        """
        # compute the log10 of the upper bound for each range
        df['log_upper'] = np.log10(df.income)

        # get the lower bounds by shifting the upper bound and filling in
        # the first element
        df['log_lower'] = df.log_upper.shift(1)
        df.loc[0, 'log_lower'] = 3.0

        # plug in a value for the unknown upper bound of the highest range
        df.loc[41, 'log_upper'] = log_upper

        # use the freq column to generate the right number of values in
        # each range
        arrays = []
        for _, row in df.iterrows():
            vals = np.linspace(int(row.log_lower), int(row.log_upper), int(row.freq))
            arrays.append(vals)

        # collect the arrays into a single sample
        log_sample = np.concatenate(arrays)
        return log_sample

In the above code InterpolateSample, I had to change the code on line 39. It previously read:
`vals = np.linspace(row.log_lower, row.log_upper, row.freq)`
it now reads:
`vals = np.linspace(int(row.log_lower), int(row.log_upper), int(row.freq))`

I was getting an error about it not accepting a float, so I changed the values that were throwing the error.

    import hinc
    income_df = hinc.ReadData()
    log_sample = InterpolateSample(income_df, log_upper=6.0)
    log_cdf = thinkstats2.Cdf(log_sample)
    thinkplot.Cdf(log_cdf)
    thinkplot.Config(xlabel='Household income (log $)',
               ylabel='CDF')

![CDF_log_sample](https://imgur.com/sANqWja "CDF Log Sample")

    sample = np.power(10, log_sample)
    cdf = thinkstats2.Cdf(sample)
    thinkplot.Cdf(cdf)
    thinkplot.Config(xlabel='Household income ($)',
               ylabel='CDF')
![CDF](https://imgur.com/lBbKaI7 "CDF")

### Compute the median, mean, skewness and Pearson’s skewness of the resulting sample. What fraction of households report a taxable income below the mean? How do the results depend on the assumed upper bound?

    Mean(sample), Median(sample), Skewness(sample), PearsonMedianSkewness(sample)
(34330.52510748183, 10000.0, 6.775200881176056, 0.9595638278153094)

    print('Percentage of households reporting a taxable income below the mean: ~ {}%'.format(int(100 * cdf.Prob(Mean(sample)))))
Percentage of households reporting a taxable income below the mean: ~ 79%

### All of this is based on an assumption that the highest income is one million dollars, but that's certainly not correct.  What happens to the skew if the upper bound is 10 million?

Changing the variable log_upper from 6.0 to 7.0 increased the skewness from 6.7752 to 12.9785 and Pearson Median Skewness from 0.9595 to 0.3935.

    log_sample = InterpolateSample(income_df, log_upper=7.0)

![CDF_log_sample](https://imgur.com/M3OHI27 "CDF Log Sample")

    cdf = thinkstats2.Cdf(sample)
    thinkplot.Cdf(cdf)
    thinkplot.Config(xlabel='Household income ($)',
               ylabel='CDF')

![CDF](https://imgur.com/yEeFXsc "CDF")

    Mean(sample), Median(sample), Skewness(sample), PearsonMedianSkewness(sample)
(76164.28739916759, 10000.0, 12.978502161571178, 0.39350664138128166)
