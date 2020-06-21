[Think Stats Chapter 9 Exercise 2](http://greenteapress.com/thinkstats2/html/thinkstats2010.html#toc90) (resampling)

**Exercise:** In Section 9.3, we simulated the null hypothesis by permutation; that is, we treated the observed values as if they represented the entire population, and randomly assigned the members of the population to the two groups.

An alternative is to use the sample to estimate the distribution for the population, then draw a random sample from that distribution. This process is called resampling. There are several ways to implement resampling, but one of the simplest is to draw a sample with replacement from the observed values, as in Section 9.10.

Write a class named `DiffMeansResample` that inherits from `DiffMeansPermute` and overrides `RunModel` to implement resampling, rather than permutation.

Use this model to test the differences in pregnancy length and birth weight. How much does the model affect the results?

    class DiffMeansResample(DiffMeansPermute):
    
      def RunModel(self):
          group1 = np.random.choice(self.pool, self.n, replace=True)
          group2 = np.random.choice(self.pool, self.m, replace=True)
          return group1, group2
    
    def RunResampleTest(firsts, others):
    
    data = firsts.prglngth.values, others.prglngth.values
    ht = DiffMeansResample(data)
    p_value = ht.PValue(iters=10000)
    print('\ndiff means resample preglength')
    print('p-value =', p_value)
    print('actual =', ht.actual)
    print('ts max =', ht.MaxTestStat())
    
    data = (firsts.totalwgt_lb.dropna().values,
           others.totalwgt_lb.dropna().values)
    ht = DiffMeansPermute(data)
    p_value = ht.PValue(iters=10000)
    print('\ndiff means resample birthweight')
    print('p-value =', p_value)
    print('actual =', ht.actual)
    print('ts max =', ht.MaxTestStat())
    
    RunResampleTest(firsts, others)
    
diff means resample preglength
p-value = 0.1688
actual = 0.07803726677754952
ts max = 0.23361188539859512

diff means resample birthweight
p-value = 0.0
actual = 0.12476118453549034
ts max = 0.11229936718712707

Using resampling instead of permutation has a very small effect on the results - there's not much of a difference between models with this data.
