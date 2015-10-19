---
layout:     post
title:      Visualizing distributions of data 
date:       2015-09-15 11:21:29
summary:    Plotting Distributions 
categories: plotting jupyter 
minutes:    15
---

This notebook demonstrates different approaches to graphically representing distributions of data, specifically focusing on the tools provided by the [seaborn](https://github.com/mwaskom/seaborn) package.


<pre><code class="python">
%matplotlib inline
</code></pre>


<pre><code class="python">
import numpy as np
from numpy.random import randn
import pandas as pd
from scipy import stats
import matplotlib as mpl
import matplotlib.pyplot as plt
import seaborn as sns
</code></pre>


```python
sns.set_palette("deep", desat=.6)
sns.set_context(rc={"figure.figsize": (8, 4)})
sns.set_style("white")
np.random.seed(9221999)
```

## Basic visualization with histograms

The most basic and common way of representing a distributions is with a histogram. We can do this directly through the `hist` function that is part of matplotlib.


```python
data = randn(75)
plt.hist(data);
```


![png](/images/plotting_distributions_files/plotting_distributions_7_0.png)


By default, `hist` separates the data into 10 bins of equal widths and plots the number of observations in each bin. Thus, the main parameter is the number of bins, which we can change.

The more bins you have, the more sensitive you will be to high-frequency patterns in the distribution. But, sometimes those high-frequency patterns will be noise. Often you want to try different values until you think you have best captured what you see in the data.


```python
plt.hist(data, 6, color=sns.desaturate("indianred", .75));
```


![png](plotting_distributions_files/plotting_distributions_9_0.png)


The `normed` argument can also be useful if you want to compare two distributions that do not have the same number of observations. Note also that `bins` can be a sequence of where each bin starts.


```python
data1 = stats.poisson(2).rvs(100)
data2 = stats.poisson(5).rvs(120)
max_data = np.r_[data1, data2].max()
bins = np.linspace(0, max_data, max_data + 1)
plt.hist(data1, bins, normed=True, color="#6495ED", alpha=.5)
plt.hist(data2, bins, normed=True, color="#F08080", alpha=.5);
```


![png](plotting_distributions_files/plotting_distributions_11_0.png)


The `hist` function has quite a few other options, which you can explore in its docstring. Here we'll just highlight one more that can be useful when plotting many observations (such as following a resampling procedure).


```python
x = stats.gamma(3).rvs(5000)
plt.hist(x, 70, histtype="stepfilled", alpha=.7);
```


![png](plotting_distributions_files/plotting_distributions_13_0.png)


You can also represent a joint distribution with the histogram method, using a hexbin plot. This is similar to a histogram, except instead of coding the number of observations in each bin with a position on one of the axes, it uses a color-mapping to give the plot three quantitative dimensions.

In `seaborn`, you can draw a hexbin plot using the `jointplot` function and setting `kind` to `"hex"`. This will also plot the marginal distribution of each variable on the sides of the plot using a histrogram:


```python
y = stats.gamma(5).rvs(5000)
with sns.axes_style("white"):
    sns.jointplot(x, y, kind="hex");
```


![png](plotting_distributions_files/plotting_distributions_15_0.png)


## Estimating the density of the observations: `kdeplot` and `rugplot`

A superior, if more computationally intensive, approach to estimating a distribution is known as a kernel density estimate, or KDE. To motivate the KDE, let's first think about rug plots. A rug plot is a very simple, but also perfectly legitimate, way of representing a distribution. To create one, simply draw a vertical line at each observed data point. Here, the height is totally arbitrary.


```python
sns.set_palette("hls", 1)
data = randn(30)
sns.rugplot(data)
plt.ylim(0, 1);
```


![png](plotting_distributions_files/plotting_distributions_18_0.png)


You can see where the density of the distribution is by how dense the tick-marks are. Before talking about kernel density plots, let's connect the rug plot to the histogram. The connection here is very direct: a histogram just creates bins along the range of the data and then draws a bar with height equal to the number of ticks in each bin


```python
plt.hist(data, alpha=.3)
sns.rugplot(data);
```


![png](plotting_distributions_files/plotting_distributions_20_0.png)


A kernel density plot is also a transformation from the tick marks to a height-encoded measure of density. However, the transformaiton is a bit more complicated. Instead of binning each tick mark, we will instead represent each tick with a gaussian basis function.


```python
# Draw the rug and set up the x-axis space
sns.rugplot(data);
xx = np.linspace(-4, 4, 100)

# Compute the bandwidth of the kernel using a rule-of-thumb
bandwidth = ((4 * data.std() ** 5) / (3 * len(data))) ** .2
bandwidth = len(data) ** (-1. / 5)

# We'll save the basis functions for the next step
kernels = []

# Plot each basis function
for d in data:
    
    # Make the basis function as a gaussian PDF
    kernel = stats.norm(d, bandwidth).pdf(xx)
    kernels.append(kernel)
    
    # Scale for plotting
    kernel /= kernel.max()
    kernel *= .4
    plt.plot(xx, kernel, "#888888", alpha=.5)
plt.ylim(0, 1);
```


![png](plotting_distributions_files/plotting_distributions_22_0.png)


We then estimate the distribution that our samples came from from by summing these basis functions (and normalizing so, as a proper density, the function integrates to 1).

There is also a function in the `scipy.stats` module that will perform a kernel density estimate (it actually returns an object that can be called on some values to return the density). We see that plotting the values from this object give us basically the same results as summing the gaussian basis functions.


```python
# Set up the plots
f, (ax1, ax2) = plt.subplots(2, 1, sharex=True)
c1, c2 = sns.color_palette("husl", 3)[:2]

# Plot the summed basis functions
summed_kde = np.sum(kernels, axis=0)
ax1.plot(xx, summed_kde, c=c1)
sns.rugplot(data, c=c1, ax=ax1)
ax1.set_yticks([])
ax1.set_title("summed basis functions")

# Use scipy to get the density estimate
scipy_kde = stats.gaussian_kde(data)(xx)
ax2.plot(xx, scipy_kde, c=c2)
sns.rugplot(data, c=c2, ax=ax2)
ax2.set_yticks([])
ax2.set_title("scipy gaussian_kde")
f.tight_layout()
```


![png](plotting_distributions_files/plotting_distributions_24_0.png)


The seaborn package has a high-level function for plotting a kernel density estimate in one quick step, along with some additional nice features, such as shading in the density.


```python
sns.kdeplot(data, shade=True);
```


![png](plotting_distributions_files/plotting_distributions_26_0.png)


Much like how the `bins` parameter controls the fit of the histogram to the data, you can adjust the bandwidth (`bw`) of the kernel to make the densisty estimate more or less sensitive to high-frequency structure.


```python
pal = sns.blend_palette([sns.desaturate("royalblue", 0), "royalblue"], 5)
bws = [.1, .25, .5, 1, 2]

for bw, c in zip(bws, pal):
    sns.kdeplot(data, bw=bw, color=c, lw=1.8, label=bw)

plt.legend(title="kernel bandwidth")
sns.rugplot(data, color="#333333");
```


![png](plotting_distributions_files/plotting_distributions_28_0.png)


Although the gaussian kernel is the most common, and probably the most useful, there are a variety of kernels you can use to fit the density estimate. The kernel you choose generally has less influence on the resulting estimate thean the bandwidth size, but there may be situations where you want to experiment with different choices.


```python
kernels = ["biw", "cos", "epa", "gau", "tri", "triw"]
pal = sns.color_palette("hls", len(kernels))
for k, c in zip(kernels, pal):
    sns.kdeplot(data, kernel=k, color=c, label=k)
plt.legend();
```


![png](plotting_distributions_files/plotting_distributions_30_0.png)


The `cut` and `clip` parameters allows you to control how far outside the range of the data the estimate extends: `cut` influences only the range of the support, while `clip` affects the fitting of the KDE as well.


```python
with sns.color_palette("Set2"):
    f, (ax1, ax2) = plt.subplots(2, 1, figsize=(8, 8), sharex=True)
    for cut in [4, 3, 2]:
        sns.kdeplot(data, cut=cut, label=cut, lw=cut * 1.5, ax=ax1)

    for clip in [1, 2, 3]:
        sns.kdeplot(data, clip=(-clip, clip), label=clip, ax=ax2);
```


![png](plotting_distributions_files/plotting_distributions_32_0.png)


As in the case of the histogram, plotting shaded density plots on top of each other can be a good way to ask whether two samples are from the same distribution. This also implements a simple classification operation. For a given `x` value with an unknown label, you should conclude it was drawn from the distribution with a greater density at that value.

A legend can be helpful when overlaying several densities. This is done either by providing a `label` keyword to `kdeplot`, which explicitly assigns a value to the data, or by inferring the name if you pass an object (like a Pandas Series) with a `name` attribute.


```python
f, (ax1, ax2) = plt.subplots(2, 1, sharex=True, figsize=(8, 6))
c1, c2, c3 = sns.color_palette("Set1", 3)

dist1, dist2, dist3 = stats.norm(0, 1).rvs((3, 100))
dist3 = pd.Series(dist3 + 2, name="dist3")

sns.kdeplot(dist1, shade=True, color=c1, ax=ax1)
sns.kdeplot(dist2, shade=True, color=c2, label="dist2", ax=ax1)

sns.kdeplot(dist1, shade=True, color=c2, ax=ax2)
sns.kdeplot(dist3, shade=True, color=c3, ax=ax2);
```


![png](plotting_distributions_files/plotting_distributions_34_0.png)


If you want to plot the density along the y axis, use the `vertical` keyword.


```python
plt.figure(figsize=(4, 7))
data = stats.norm(0, 1).rvs((3, 100)) + np.arange(3)[:, None]

with sns.color_palette("Set2"):
    for d, label in zip(data, list("ABC")):
        sns.kdeplot(d, vertical=True, shade=True, label=label)
```


![png](plotting_distributions_files/plotting_distributions_36_0.png)


You can also use `kdeplot` to estimate the cumulative distribution function (CDF) of the population from your data. This plot will tell you what proportion of the distribution falls at smaller values than a given point on the `x` axis:


```python
with sns.color_palette("Set1"):
    for d, label in zip(data, list("ABC")):
        sns.kdeplot(d, cumulative=True, label=label)
```


![png](plotting_distributions_files/plotting_distributions_38_0.png)


## Multivariate density estimation with `kdeplot`

You can also use the kernel density method with multidimensional data. For visualization, we are mostly concerned with plotting joint bivariate distributions. As with the hexbin plot, we will color-encode the density estimate over a 2D space. The `kdeplot` function tries to infer whether it should draw a univariate or bivariate plot based on the type and shape of the `data` argument. If using a 2d array or a DataFrame, the array is assumed to be shaped (`n_units`, `n_variables`).


```python
data = np.random.multivariate_normal([0, 0], [[1, 2], [2, 20]], size=1000)
data = pd.DataFrame(data, columns=["X", "Y"])
mpl.rc("figure", figsize=(6, 6))
```


```python
sns.kdeplot(data);
```


![png](plotting_distributions_files/plotting_distributions_42_0.png)


You can also pass in two vectors as the first positional arguments, which will also draw a bivariate density.


```python
sns.kdeplot(data.X, data.Y, shade=True);
```


![png](plotting_distributions_files/plotting_distributions_44_0.png)


As in the univariate case, you can also set the kernel bandwidth and choose different points at which to clip the data or cut the estimate. However, only the gaussian kernel is availible for the multidimensional kde.

When specifying the colormap, the multivariate `kdeplot` accepts a special token where colormaps that end with `_d` are plotted in a way that maintains the overall color palette but that uses darker colors at one extreme so the contour lines are fully visible.


```python
sns.kdeplot(data, bw="silverman", gridsize=50, cut=2, clip=(-11, 11), cmap="BuGn_d");
```


![png](plotting_distributions_files/plotting_distributions_46_0.png)



```python
sns.kdeplot(data, bw=1, clip=[(-4, 4), (-11, 11)], cmap="PuRd_d");
```


![png](plotting_distributions_files/plotting_distributions_47_0.png)



```python
sns.kdeplot(data.values, shade=True, bw=(.5, 1), cmap="Purples");
```


![png](plotting_distributions_files/plotting_distributions_48_0.png)


## Bivariate and univariate plots using `jointplot`:

The `jointplot` function allows you to simultaneously visualize the joint distribution of two variables and the marginal distribution of each. Above, we showed how to use it to draw a hexbin plot. You can also use it to draw a kernel density estimate:


```python
with sns.axes_style("white"):
    sns.jointplot("X", "Y", data, kind="kde");
```


![png](plotting_distributions_files/plotting_distributions_51_0.png)


## Combining plot styles: `distplot`

Each of these styles has advantages and disadvantages. Fortunately, it is easy to combine multiple styles using the `distplot` function in seaborn. `distplot` provides one interface for plotting histograms, kernel density plots, rug plots, and plotting fitted probability distributions.

By default, you'll get a kernel density over a  histogram. Unlike the default matplotlib `hist` function, `distplot` tries to use a good number of bins for the dataset you have, although all of the options for specifying bins in `hist` can be used.


```python
sns.set_palette("hls")
mpl.rc("figure", figsize=(8, 4))
data = randn(200)
sns.distplot(data);
```


![png](plotting_distributions_files/plotting_distributions_54_0.png)


`hist`, `kde`, and `rug` are boolean arguments to turn those features on and off.


```python
sns.distplot(data, rug=True, hist=False);
```


![png](plotting_distributions_files/plotting_distributions_56_0.png)


You can also pass a distribution family from `scipy.stats`, and `distplot` will fit the parameters using maximum likelihood and plot the resulting function.


```python
sns.distplot(data, kde=False, fit=stats.norm);
```


![png](plotting_distributions_files/plotting_distributions_58_0.png)


To control any of the underlying plots, pass keyword arguments to the `[plot]_kws` argument.


```python
sns.distplot(data,
             kde_kws={"color": "seagreen", "lw": 3, "label": "KDE"},
             hist_kws={"histtype": "stepfilled", "color": "slategray"});
```


![png](plotting_distributions_files/plotting_distributions_60_0.png)


You can also draw the distribution vertically, if for example you wanted to plot marginal distributions on a scatterplot (as in the `jointplot` function):


```python
plt.figure(figsize=(4, 7))
sns.distplot(data, color="dodgerblue", vertical=True);
```


![png](plotting_distributions_files/plotting_distributions_62_0.png)


If the data has a `name` attribute (e.g. it is a pandas `Series`), the name will become the label for the dimension on which the distributio is plotted, unless you use `axlabel=False`. You can also provide a string, which will override this behavior and label nameless data.


```python
sns.distplot(pd.Series(data, name="score"), color="mediumpurple");
```


![png](plotting_distributions_files/plotting_distributions_64_0.png)


## Comparing distributions: `boxplot` and `violinplot`


```python
sns.set(rc={"figure.figsize": (6, 6)})
sns.set_style("white")
```

Frequently, you will want to compare two or more distributions. Although above we showed one method to do this above, it's generally better to plot them separately but in the way that allows for easy comparisons.

The traditional approach in this case is to use a boxplot. There is a `boxplot` function in matplotlib we could use...


```python
data = [randn(100), randn(100) + 1]
plt.boxplot(data);
```


![png](plotting_distributions_files/plotting_distributions_68_0.png)


...but, it is quite ugly by default. To get more aesthetically pleasing plots, use the `seaborn.boxplot` function:


```python
sns.boxplot(data);
```


![png](plotting_distributions_files/plotting_distributions_70_0.png)


The default rules for a boxplot are that the box encompasses the inter-quartile range with the median marked. The "whiskers" extend to 1.5 * IQR past the closest quartile, and any observations outside this range are marked as outliers.

This is quite a mouthfull though, and the outliers can be distracting, so you can just make the whiskers extend all the way out. Let's also tweak the aesthetics a bit.


```python
sns.boxplot(data, names=["group1", "group1"], whis=np.inf, color="PaleGreen");
```


![png](plotting_distributions_files/plotting_distributions_72_0.png)


If fits better with the plot you are drawing, the boxplot can be horiztonal. This just uses the matplotlib parameter, which is awkwardly named `vert`:


```python
sns.boxplot(data, names=["group1", "group1"], linewidth=2, widths=.5, color="skyblue", vert=False);
```


![png](plotting_distributions_files/plotting_distributions_74_0.png)


In some cases, you may want to plot repeated-measures data. In this case, a subtle effect that is consistent across subjects can be masked and look non-consequential.

To show such an effect, use the `join_rm` argument.


```python
pre = randn(25)
post = pre + np.random.rand(25)
sns.boxplot([pre, post], names=["pre", "post"], color="coral", join_rm=True);
```


![png](plotting_distributions_files/plotting_distributions_76_0.png)


The boxplot is more informative than a bar plot, but it still compresses the a distribution to about five points. Just as the kernel density plot is a modern alternative to the histogram, we can use our computing power to bring more information using a kernel density estimate to these comparative plots.

These plots are known as "violin" (apparently, sometimes "viola") plots. They essentially combine a boxplot with a kernel density estimate.

Let's create a toy case that demonstrates why we might prefer the increased information in the violin plot.


```python
d1 = stats.norm(0, 5).rvs(100)
d2 = np.concatenate([stats.gamma(4).rvs(50),
                     -1 * stats.gamma(4).rvs(50)])
data = pd.DataFrame(dict(d1=d1, d2=d2))
```

First, draw a boxplot. Note that the `color` argument can take anything that can be used as a palette in addition to any single valid matplotlib color, and that the function is Pandas-aware and will try to label the axes appropriately.


```python
sns.boxplot(data, color="pastel", widths=.5);
```


![png](plotting_distributions_files/plotting_distributions_80_0.png)


Based on this plot, it looks like we basically have two samples from the same distribution.

But, let's just see what the violin plot looks like:


```python
sns.violinplot(data, color="pastel");
```


![png](plotting_distributions_files/plotting_distributions_82_0.png)


Woah! Now it looks like the distribution on the left is roughly normal, but the distribution on the right is bimodal with peaks at $+/-$ 5.

It may be rare to run into such data, but more information doesn't hurt even in non-pathological cases, and might catch problems that otherwise could slip through.

(Of course, if you looked at each distribution with a histogram/KDE plot as above, you might have caught this before making any comparisons.)

Both the boxplot and violin functions can take a Pandas Series object as the data and an object that can be used to perform a `groupby` on the data to group it into the boxes/violins.


```python
y = np.random.randn(200)
g = np.random.choice(list("abcdef"), 200)
for i, l in enumerate("abcdef"):
    y[g == l] += i // 2
df = pd.DataFrame(dict(score=y, group=g))
sns.boxplot(df.score, df.group);
```


![png](plotting_distributions_files/plotting_distributions_85_0.png)


 

Much like the `kdeplot`, you can tune the bandwidth of the kernel used to fit the density estimate in the violin.


```python
sns.violinplot(df.score, df.group, color="Paired", bw=1);
```


![png](plotting_distributions_files/plotting_distributions_88_0.png)


If using a groupby, the default is to plot the boxes or violins in the sorted order of the group labels. You can override this, though:


```python
order = list("cbafed")
sns.boxplot(df.score, df.group, order=order, color="PuBuGn_d");
```


![png](plotting_distributions_files/plotting_distributions_90_0.png)


The violin plot by default plots the median, along with the 25th and 75th percentile -- the same information we get from the boxplot. There are, however, other options. You might want to plot each observation (similar to what we do with a rug plot under a KDE). There are two ways to accomplish this.


```python
data = pd.melt(data.ix[:50], value_name="y", var_name="group")
```


```python
f, (ax_l, ax_r) = plt.subplots(1, 2)
sns.violinplot(data.y, data.group, "points", positions=[1, 2], color="RdBu", ax=ax_l)
sns.violinplot(data.y, data.group, "stick", positions=[3, 4], color="PRGn", ax=ax_r)
plt.tight_layout()
```


![png](plotting_distributions_files/plotting_distributions_93_0.png)


Of course, you can plot repeated-measures data with the violin as well.


```python
pre = randn(20)
data = pd.DataFrame(dict(pre=pre, post=pre + 1 + randn(20)), columns=["pre", "post"])
sns.violinplot(data, inner="points", join_rm=True, color="RdGy_r");
```


![png](plotting_distributions_files/plotting_distributions_95_0.png)


Using a palette colorscheme can be particularly useful if you have many bins.


```python
mpl.rc("figure", figsize=(9, 6))
```


```python
data = randn(50, 8) + np.random.uniform(3, 5, 8)
sns.boxplot(data);
```


![png](plotting_distributions_files/plotting_distributions_98_0.png)


Chose the color scheme carefully! The above is good for categorigal bins, but perhaps there is some ordering to the grouping variable:


```python
data.sort(axis=1)
sns.boxplot(data, widths=.8, color="cubehelix");
```


![png](plotting_distributions_files/plotting_distributions_100_0.png)


Different kinds of relationships lend themselves to different kinds of color palettes:


```python
data = data * -1 + data.mean()
sns.violinplot(data, color="coolwarm_r", lw=2);
```


![png](plotting_distributions_files/plotting_distributions_102_0.png)


 

 
