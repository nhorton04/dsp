[Think Stats Chapter 8 Exercise 3](http://greenteapress.com/thinkstats2/html/thinkstats2009.html#toc77)

    def Estimated(lam=2, m=1000000):

        estimates = []
        for i in range(m):
            L = SimulateGame(lam)
            estimates.append(L)

        print('Experiment 4')
        print('rmse L', RMSE(estimates, lam))
        print('mean error L', MeanError(estimates, lam))

        pmf = thinkstats2.Pmf(estimates)
        thinkplot.Hist(pmf)
        thinkplot.Config(xlabel='Goals scored', ylabel='PMF')

    Estimated()
    
Experiment 4
rmse L 1.413521135321294
mean error L 0.001204

![Hist](https://i.imgur.com/YYcrSWO.png)
