---
blog-title: How to animate a Radar Plot
blog-published: 2022-02-21
blog-tags:
  - EN
  - Python
  - Data Science
  - Data Visualization
blog-archived: true
---

In this short guide I will show you how to animate a *Radar Plot* with [matplotlib](https://matplotlib.org) in Python.

## Example 

At the end of this guide you will be able to create radar plot animations like this one:


<iframe width="560" height="315" src="https://www.youtube.com/embed/9Uk6DoPJqjs" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>


## Import modules

The modules needed are numpy and matplotlib. Import them like this:
```python
import numpy as np
import matplotlib.pyplot as plt
from matplotlib.animation import FuncAnimation
```

## Data 

For this guide, I am creating some artificial data to make the data structure a bit more visible: 

```python
length = 250

data = [
	[np.sin(i*0.06) for i in range(0, length)],
	[np.cos(i*0.06) for i in range(0, length)],
	[np.sin(i*0.06) for i in range(0, length)],
	[np.cos(i*0.06) for i in range(0, length)],
	[np.sin(i*0.06) for i in range(0, length)]
]
```

`data` is a list of the different features that you want to plot. Every feature list contains the individual datapoints that the animation plots at frame ⁠`i`. This means all lists have to be the same length.

Now, you copy the first feature list to the end of the data list. This will ensure that the radar plot will be a closed polygon. The first and last datapoint will overlap and close the shape.

```python
data = [*data, data[0]]

# This works too 
data.append(data[0])
```

To achieve this, we can use the syntax `*expression` to spread out the list and append the first entry.  

From the Python documentation:
> If the syntax `*expression` appears in the function call, `expression` must evaluate to an [iterable](https://docs.python.org/3/glossary.html#term-iterable). Elements from these iterables are treated as if they were additional positional arguments. For the call`f(x1, x2, *y, x3, x4)`, if _y_ evaluates to a sequence _y1_, …, _yM_, this is equivalent to a call with M+4 positional arguments _x1_, _x2_, _y1_, …, _yM_, _x3_, _x4_.


## Labels

To create labels for every feature in the data list, create a new list and apply the same procedure with the spread operator.

```python
labels = ["Label 1", "Label 2", "Label 3", "Label 4", "Label 5"]
labels = [*labels, labels[0]]

# This works too 
labels.append(labels[0])
```

The last step, before plotting begins, will be to calculate evenly distributed positions for the labels like this:

```python
# Get len(labels) equal distance points on a circle
label_loc = np.linspace(0, 2*np.pi, num=len(labels))
```

## Initialize plot 

Matplotlib animations require an initial plot using the first datapoints. To create a radar plot, set the projection method to `polar`.

```python
fig, ax = plt.subplots(figsize=(10, 10), subplot_kw={'projection': 'polar'})
```

In a radar plot the x-values will correspond to the positions on the circle (0-360 degrees) and the y-values are the distances from the middle point of the circle.

The x-values will be the label positions and the y-values will be the datapoints. A list comprehension will select the first datapoints for the initial plot.

```python
radar_ln = ax.plot(label_loc, [d[0] for d in data], label="Legend")
```

The code places the labels at the calculated positions around the circle by converting radians to degrees and setting them in the `⁠set_thetagrids` method.

```python
ax.set_thetagrids(np.degrees(label_loc), labels=labels)
ax.legend(loc="lower left", bbox_to_anchor=(0.95, 0.95))
```

You can change the limits in a radar plot too:

```python
ax.set_rmax(2)
ax.set_rmin(-2)
```

## Update function

Animating a plot with matplotlib will always include an `update(i)` function which will update the plot with new data for every frame `i`. In this case, a list comprehension selects the new data (the same way you initialized the plot).

```python
def update(i, radar_ln, label_loc, data):
	radar_ln[0].set_data(label_loc, [d[i] for d in data])
	return radar_ln
```


## Rendering and saving the animation 

The last step is assigning the correct variables to `FuncAnimation` before running the `save()` method.

```python
ani = FuncAnimation(fig, update, frames=len(data[0]), fargs=(radar_ln, label_loc, data), interval=40)

ani.save("animation.mp4", dpi=300)
```

