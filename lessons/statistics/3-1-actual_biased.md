[Think Stats Chapter 3 Exercise 1](http://greenteapress.com/thinkstats2/html/thinkstats2004.html#toc31) (actual vs. biased)

**Exercise:** Something like the class size paradox appears if you survey children and ask how many children are in their family. Families with many children are more likely to appear in your sample, and families with no children have no chance to be in the sample.
Use the NSFG respondent variable `numkdhh` to construct the actual distribution for the number of children under 18 in the respondents' households.
Now compute the biased distribution we would see if we surveyed the children and asked them how many children under 18 (including themselves) are in their household.
Plot the actual and biased distributions, and compute their means.

    resp = nsfg.ReadFemResp()
    
    dist_under_18 = thinkstats2.Hist(resp.numkdhh, label='numkdhh')
    thinkplot.Hist(dist_under_18)
    thinkplot.Config(xlabel='Number of Children less than age 18', ylabel='count')
    
![Hist](https://i.imgur.com/HmcH089.png)

    n_18 = dist_under_18.Total()
    pmf_n = dist_under_18.Copy()

    for x, freq in dist_under_18.Items():
        pmf_n[x] = freq / n

    thinkplot.Hist(pmf_n)
    thinkplot.Config(xlabel='Number of Children less than age 18', ylabel='PMF')

![PMF](https://i.imgur.com/eCtzKbY.png)

    pmf_object = thinkstats2.Pmf(resp.numkdhh, label='actual')
    thinkplot.Pmf(pmf_object)
    thinkplot.Config(xlabel='# of children under age 18', ylabel='pmf')
    
![PMF_Object](https://i.imgur.com/JsYS5Kl.png)

def BiasPmf(pmf_object, label):
    new_pmf = pmf_object.Copy(label=label)
    
    for x, p in pmf_object.Items():
        new_pmf.Mult(x, x)
        
    new_pmf.Normalize()
    return new_pmf
    
    biased_pmf = BiasPmf(pmf_object, label='biased')

    thinkplot.Pmf(biased_pmf)
    thinkplot.Config(xlabel='# of children under 18', ylabel='PmF')
    
![PMF_Object](https://i.imgur.com/qzRgSS0.png)

    thinkplot.PrePlot(2)
    thinkplot.Pmfs([pmf_object, biased_pmf])
    thinkplot.Config(xlabel='Number of Children under age 18', ylabel='PmF')
    
![Dual_PMFs](https://i.imgur.com/MLY5ES2.png)

    print('Actual mean', pmf_object.Mean())
    print('Biased mean', biased_pmf.Mean())
    
Actual mean 1.024205155043831
Biased mean 2.403679100664282
