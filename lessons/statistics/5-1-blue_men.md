[Think Stats Chapter 5 Exercise 1](http://greenteapress.com/thinkstats2/html/thinkstats2006.html#toc50) (blue men)

**Exercise:** In the BRFSS (see Section 5.4), the distribution of heights is roughly normal with parameters µ = 178 cm and σ = 7.7 cm for men, and µ = 163 cm and σ = 7.3 cm for women.
In order to join Blue Man Group, you have to be male between 5’10” and 6’1” (see http://bluemancasting.com). What percentage of the U.S. male population is in this range? Hint: use `scipy.stats.norm.cdf`.

    import scipy.stats
   
    mu = 178
    sigma = 7.7
    dist = scipy.stats.norm(loc=mu, scale=sigma)
    
    lower = dist.cdf(177.8)
    upper = dist.cdf(185.42)
    print('lower = {}'.format(lower))
    print( 'upper = {}'.format(upper)) 
    print('upper - lower = {}'.format(upper - lower))

lower = 0.48963902786483265

upper = 0.8323858654963063

upper - lower = 0.3427468376314737
