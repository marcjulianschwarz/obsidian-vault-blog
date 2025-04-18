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

The only modules needed are numpy and matplotlib. Import them like this:
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

`data` is a list of the different features that you want to plot. Every feature list contains the individual datapoints that will be plotted at frame `i`. This means all lists have to be the same length.

Now, the first feature list will need to be copied to the end of the data list. This will ensure that the radar plot will be a closed polygon. The first and last datapoint will overlap and therefore close the shape.

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

For every animation in matplotlib an initial plot (using the first datapoints) is needed. To create a radar plot it is important to set the projection method to `polar`.

```python
fig, ax = plt.subplots(figsize=(10, 10), subplot_kw={'projection': 'polar'})
```

In a radar plot the x-values will correspond to the positions on the circle (0-360 degrees) and the y-values are the distances from the middle point of the circle.

Therefore the x-values will be the label positions and the y-values will be the datapoints. For the initial plot the first datapoints will be selected using a list comprehension.

```python
radar_ln = ax.plot(label_loc, [d[0] for d in data], label="Legend")
```

The labels themselves will be placed at the previously calculated positions around the circle by converting the radians to degrees and setting the labels in the `set_thetagrids` method. 

```python
ax.set_thetagrids(np.degrees(label_loc), labels=labels)
ax.legend(loc="lower left", bbox_to_anchor=(0.95, 0.95))
```

The limits in a radar plot can be changed too:

```python
ax.set_rmax(2)
ax.set_rmin(-2)
```

## Update function

Animating a plot with matplotlib will always include an `update(i)` function which will update the plot with new data for every frame `i`. In this case the new data will again be selected with a list comprehension (the same way the plot was initialized).

```python
def update(i, radar_ln, label_loc, data):
	radar_ln[0].set_data(label_loc, [d[i] for d in data])
	return radar_ln
```


## Rendering and saving the animation 

Finally, it is only a matter of assigning the correct variables to `FuncAnimation` before running the `save()` method.

```python
ani = FuncAnimation(fig, update, frames=len(data[0]), fargs=(radar_ln, label_loc, data), interval=40)

ani.save("animation.mp4", dpi=300)
```

