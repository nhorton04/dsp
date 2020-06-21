[Think Stats Chapter 8 Exercise 3](http://greenteapress.com/thinkstats2/html/thinkstats2009.html#toc77)

**Exercise:** In games like hockey and soccer, the time between goals is roughly exponential. So you could estimate a team’s goal-scoring rate by observing the number of goals they score in a game. This estimation process is a little different from sampling the time between goals, so let’s see how it works.

Write a function that takes a goal-scoring rate, `lam`, in goals per game, and simulates a game by generating the time between goals until the total time exceeds 1 game, then returns the number of goals scored.

Write another function that simulates many games, stores the estimates of `lam`, then computes their mean error and RMSE.

Is this way of making an estimate biased?

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
