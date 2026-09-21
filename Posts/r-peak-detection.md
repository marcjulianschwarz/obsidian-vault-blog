---
blog-title: Automatic R Peak Detection Based on Hierarchical Clustering in Python
blog-published: 2023-12-31
blog-tags:
  - Python
  - Math
  - Data Science
blog-skip: true
---

# Automatic R Peak Detection Based on Hierarchical Clustering

So, about a year ago I was playing around with some Apple Watch data by writing a Python package for quickly loading relevant data from an Apple Health export. One essential part of the exported data are electrocardiograms (ECGs). 
One important step in ECG analysis is the detection of R peaks, which correspond to the highest point of the QRS complex in the ECG waveform. R peak detection is crucial for accurate measurement of heart rate and other ECG features. 
Back then I was trying to detect these peaks by measuring the slope of the signal and given some parameters (window length, minimum amplitude of slope signal, minimal amplitude of ECG signal) estimate the points where it is the highest. Using this method I was able to detect R-wave peaks with relatively high accuracy. However I had to set several parameters and adjust them depending on the dataset that was used.

In this blog post I will implement an automatic R peak detection method based on hierarchical clustering in Python. This method should be more robust as there are no parameters that have to be set.

Again, I will be using ECG data that has been collected from an Apple Watch to demonstrate it.

## Loading Data  

First, we use the `WatchLoader` class to load the ECG data from the Apple Health Export files and use the first ECG as an example.

```python
from watch import WatchLoader 
import numpy as np

wl = WatchLoader(data_path="data", export_name="marc")
ecgs = wl.ecgs()
ecg = np.array(next(ecgs))
```


## Overview 


![[ECG R Peak Cluster Extraction.canvas]]


## Preprocessing of the raw ECG signal

The "raw" ECG signal we get from an Apple Watch health export is already preprocessed and most of the noise has been filtered out. 
Thus the only preprocessing step left to do is to normalize the signal to a fixed range of values. We define the following function to normalize the ECG data to the interval from zero to one.

```python
def normalize_ecg(ecg):
	return (ecg - min(ecg)) / (max(ecg) - min(ecg))
```

Applying the normalization to our selected ECG will yield the following plot. As we can see the values are now all between zero and one.

```python
import matplotlib.pyplot as plt

def plot_ecg(ecg, width=10, height=3, title="", x_label="Samples", y_label="Amplitude"):
    fig, ax = plt.subplots(figsize=(width, height))
    ax.plot(ecg)
    ax.set_title(title)
    ax.set_xlabel(x_label)
    ax.set_ylabel(y_label)
    plt.show()

ecg = normalize_ecg(ecg)
ecg = ecg[:1000]
plot_ecg(ecg, title="Normalized ECG Signal")
```

![[norm_ecg.png]]

## Feature Extraction 

The clustering will be done on two features, the ECG signal itself and the slope of the signal at every point in time.
One simple way to get the slope at a given position is to take the average of the absolute values of the amplitude differences between the previous and next value.
$$S[i]= \begin{cases}|A[i+1]-A[i]| & i=1 \\ (|A[i+1]-A[i]|+|A[i]-A[i-1]|) / 2 & 1<i<n \\ |A[i]-A[i-1]| & i=n\end{cases}$$

This follows the general definition of differentiation 
$$L=\lim _{h \rightarrow 0} \frac{f(a+h)-f(a)}{h}$$
where we have 
$$f(i)=A[i] \quad \text{and} \quad h=1.$$

The implementation in Python is relatively straight forward and we can use the same plotting function from above to take a look at the slope signal.

```python
def slope(ecg, i):
    n = len(ecg)
    if i == 0:
        return abs(ecg[i+1] - ecg[i])
    if 0 < i < n-1:
	    previous_val = abs(ecg[i+1] - ecg[i])
	    next_val = abs(ecg[i] - ecg[i-1])
        return (previous_val + next_val) / 2
    if i == n :
        return abs(ecg[i] - ecg[i-1])

slopes = [slope(ecg, i) for i in range(len(ecg))]
plot_ecg(s, title="Slope of normalized ECG Signal", y_label="Slope")
```

![[slope_norm_ecg 1.png]]

We can clearly see the two spikes of the R-waves.

To make the next steps a bit easier we will now use a Pandas dataframe to store the data.

```python
import pandas as pd

X = zip(slopes, ecg)
X = pd.DataFrame(X, columns=["slope", "ecg"]).dropna()
```


## Hierarchical Clustering to find R-Wave Clusters 

Hierarchical Clustering is a special type of clustering that generates a hierarchical decomposition of the data. There are two main approaches on how to do this. 
The first one is called **Agglomerative Nesting** which works *bottom-to-top*. This means it starts with each data point being a single cluster and then step by step merges these clusters based on their distance to each other. It stops when either a termination condition is met (e.g. cluster count) or the whole dataset is in one cluster.
The second one works the other way around and is called **Divisive Analysis** which works *top-to-bottom*. In the beginning it assumes that the whole dataset is one big cluster and then step by step splits the data into smaller clusters based on their distance to each other. This method terminates when either a certain condition is met or every datapoint is its own cluster.

In this case we use an agglomerative nesting algorithm.

The first step is to calculate the pairwise distances for the whole dataset. These distances can then be used to decide whether to merge two clusters or not.

```python
from scipy.spatial.distance import pdist
from scipy.cluster.hierarchy import linkage, fcluster, dendrogram

Y = pdist(X, metric="euclidean")
```

We then use the average-linkage hierarchical clustering algorithm to calculate the decomposition of our dataset.

```python
Z = linkage(Y, method="average")
```

We can use a so called **Dendrogram** to visualize the generated hierarchy.

```python
dendrogram(Z)
frame1 = plt.gca()
frame1.axes.xaxis.set_ticklabels([])
plt.show()
```

![[r_wave_cluster_dendrogram.png]]

As we haven't specified a termination condition yet, the agglomerative nesting method will merge clusters until the whole dataset is in one cluster. From the plot we can clearly see that this worked. It started out with lots of small clusters on the bottom and then merged them to the top.
To obtain clusters from the decomposition we can split the dendrogram at any height. The resulting tree components will then represent the clusters.

Also we can tell that there might be three distinct clusters (orange, green, red) where green is the largest of the three.

In the next step we specify the termination condition to be *maxclust* with a value of three. This way we can obtain three clusters.

```python
X["cluster"] = fcluster(Z, criterion="maxclust", t=3)
```

We can now plot the features again, colored by cluster class.

```python
fig, ax = plt.subplots()
ax.scatter(X[X["cluster"] == 1]["ecg"], X[X["cluster"] == 1]["slope"], c="blue", s=4, label="R-wave cluster")
ax.scatter(X[X["cluster"] == 2]["ecg"], X[X["cluster"] == 2]["slope"], c="red", s=4, label="Non R-wave cluster")
ax.set_title("R-wave Clusters")
ax.set_xlabel("Amplitude")
ax.set_ylabel("Slope")
ax.legend(loc="upper left", title="Classes")
plt.show()
```


![[r_wave_clusters.png]]
Of course from this plot it is hard to tell what the clusters represent in the original ECG signal.

```python
fig, ax = plt.subplots(figsize=(10, 3))
ax.plot(X["ecg"], c="gray")
ax.scatter(X[X["cluster"] == 1].index, X[X["cluster"] == 1]["ecg"], c="blue", s=10)
ax.set_xlabel("Samples")
ax.set_ylabel("Amplitude")
ax.set_title("R-Wave Clusters")
plt.show()
```


![[r_wave_clusters_ecg.png]]

Here we can now see that our three clusters represent:
- upper R-wave peak 
- lower R-wave peak 
- non R-wave 

## Hierarchical Clustering to find R-Wave Sequences 

We now have a cluster containing all R-peaks in the current ECG signal window. There can of course be multiple R-waves inside of a window which is why we want to find the individual R-wave sequences.

```python
r_cluster = X[X["cluster"] == 1]
r_cluster["x"] = r_cluster.index
r_cluster = r_cluster[["x", "ecg"]]

Y = pdist(r_cluster, metric="euclidean")
Z = linkage(Y, method="single")
clusters = fcluster(Z, criterion="maxclust", t=2)

r_cluster["seq_cluster"] = clusters
X.loc[r_cluster.index, "seq_cluster"] = r_cluster["seq_cluster"]

fig, ax = plt.subplots(figsize=(10, 3))
ax.plot(X["ecg"], c="gray")
ax.scatter(X[X["seq_cluster"] == 1].index, X[X["seq_cluster"] == 1]["ecg"], c="blue", s=10)
ax.scatter(X[X["seq_cluster"] == 2].index, X[X["seq_cluster"] == 2]["ecg"], c="red", s=10)
plt.show()
```

![[r_wave_sequences.png]]

## Estimating R-Wave Peaks 

Estimating the R-wave peaks is as simple as finding the index of the maximum value in each R-wave sequence.

```python
fig, ax = plt.subplots(figsize=(10, 3))
ax.plot(X["ecg"], c="gray")
ax.scatter(X[X["seq_cluster"] == 1]["ecg"].idxmax(), 1, c="blue", s=20, marker="s")
ax.scatter(X[X["seq_cluster"] == 2]["ecg"].idxmax(), 1, c="red", s=20, marker="s")
ax.set_ylabel("Amplitude")
ax.set_xlabel("Samples")
ax.set_title("R-Wave Peaks")
plt.show()
```

![[r_wave_peaks.png]]


## Literature 

H. Chen and K. Maharatna, "An Automatic R and T Peak Detection Method Based on the Combination of Hierarchical Clustering and Discrete Wavelet Transform," in IEEE Journal of Biomedical and Health Informatics, vol. 24, no. 10, pp. 2825-2832, Oct. 2020, doi: 10.1109/JBHI.2020.2973982.