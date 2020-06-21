[Think Stats Chapter 7 Exercise 1](http://greenteapress.com/thinkstats2/html/thinkstats2008.html#toc70) (weight vs. age)


    import first

    live, firsts, others = first.MakeFrames()
    live = live.dropna(subset=['agepreg', 'totalwgt_lb'])
    
    thinkplot.Scatter(live.agepreg, live.totalwgt_lb)
    thinkplot.Config(xlabel='Age (years)',
                    ylabel='Birth weight (lbs)',
                    legend=False)
    print('Pearson Correlation', Corr(live.totalwgt_lb, live.agepreg))
    print('Spearman Correlation', SpearmanCorr(live.totalwgt_lb, live.agepreg))
    
Pearson Correlation 0.06883397035410907
Spearman Correlation 0.09461004109658228

![Scatterplot](https://i.imgur.com/2VeEsE7.png)

The scatterplot shows a weak relationship between the variables and the correlations support this. Pearson's is around 0.07, while Spearman's is around 0.09. The difference suggests that there are outliers in the dataset, or a non-linear relationship.
