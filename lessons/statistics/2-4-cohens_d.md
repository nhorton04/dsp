[Think Stats Chapter 2 Exercise 4](http://greenteapress.com/thinkstats2/html/thinkstats2003.html#toc24) (Cohen's d)

Using the variable `totalwgt_lb`, investigate whether first babies are lighter or heavier than others.

Compute Cohen’s effect size to quantify the difference between the groups. How does it compare to the difference in pregnancy length?

    mean = live.totalwgt_lb.mean()
    var = live.totalwgt_lb.var()
    std = live.totalwgt_lb.std()
    
    print('mean: {}, var: {}, std: {}'.format(mean, var, std))
    print('firstborn totalwgt_lb mean: {} lbs, other totalwgt_lb mean: {} lbs'.format(firsts.totalwgt_lb.mean(), others.totalwgt_lb.mean()))
    print("Difference in lbs: {} lbs".format(firsts.totalwgt_lb.mean() - others.totalwgt_lb.mean()))
    
    firsts.totalwgt_lb.mean() - others.totalwgt_lb.mean()

mean: 7.265628457623368, var: 1.9832904288326532, std: 1.4082934455690168
firstborn totalwgt_lb mean: 7.201094430437772 lbs, other totalwgt_lb mean: 7.325855614973262 lbs  
Difference in lbs: -0.12476118453549034 lbs

### First babies are slightly (0.125lbs) lighter

    difference = CohenEffectSize(firsts.totalwgt_lb, others.totalwgt_lb) 
    print("difference: {}".format(difference))
difference: -0.088672927072602

Compared with difference in pregnancy length between first babies and others:

    CohenEffectSize(firsts.prglngth, others.prglngth)
    
0.028879044654449883

The Cohen Effect Size for the difference between the weight of first babies and others is -3.07x the Cohen Effect Size for the difference between pregnancy lengths for first babies and others (-0.088672927072602 / 0.028879044654449883 = -3.070493783)

