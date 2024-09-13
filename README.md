Like [Figure 3](https://www.annualreviews.org/docserver/fulltext/marine/9/1/ma90083.f3.gif), in 
[Lynch-Stieglitz, Jean. 2017. Annual review of marine science](https://doi.org/10.1146/annurev-marine-010816-060415),
stacked line plots are common in the field of Paleoclimate research.
They are usually done in GUI plot APP. But in Python, some specific 
settings are needed. Here is a sample of how to make it using Python.

There is also another realization of stacked plot, see in Pkg [Pyleoclim](https://pyleoclim-util.readthedocs.io/en/latest/_modules/pyleoclim/core/multipleseries.html#MultipleSeries.stackplot) document.

# Stacked Line Plot

## Dependency

python pkgs:

- numpy
- xarray
- matplotlib


## Demo of Processes

it follows these steps

![202409_Python_LinePlot](https://github.com/user-attachments/assets/c016e261-aa65-4a06-8ad7-0b25988656c0)

# Sample Code

```Python
##----------------------------------------
# Define the compression factor for the y-axis.
# range (0, 0.5), could be adjusted according to the need.
# here, just a demo
y_stack_factor = (1-0.618)/2

##----------------------------------------
# Example usage, x-axis can be different for each subplot
x = np.linspace(-np.pi*2, np.pi*2, 100)
data1 = xr.DataArray(np.sin(x)*4, coords=[('x0', x)], dims=['some_x'], attrs={'long_name': '4 Sine'})
data2 = xr.DataArray(np.cos(x)*2, coords=[('x2', x)], dims=['x_some'], attrs={'long_name': '2 Cosine'})
data3 = xr.DataArray(np.sin(x),   coords=[('x3',x-np.pi/4)], dims=['x'], attrs={'long_name': 'Shifted Sine'})
data4 = xr.DataArray(np.cos(x/2),   coords=[('x4',x)], dims=['xn'], attrs={'long_name': '0.5x freq Cosine'})

##----------------------------------------
## list of data, to be plotted
datas = [data1, data2, data3, data4]

##----------------------------------------
## other parameters
cols  = ['tab:red', 'tab:blue', 'tab:orange', 'tab:green']
indexs = ['a', 'b', 'c', 'd']
title = 'Title'
x_label = 'Bottom X-Axis Label'


##----------------------------------------
# start to create the plot
fig = plt.figure(figsize=(4, 1.1 * len(datas)))
##----------------------------------------
axes = create_stacked_plot(fig, datas, y_stack_factor, 
                           color_list=cols, x_axis_label=x_label, 
                           figure_index=indexs, title=title)

##----------------------------------------
# set x-axis limit
# axes[0].set_xlim(-np.pi*1.7, np.pi*1.7)

##----------------------------------------
## add span
span_x = [-np.pi/2, np.pi/2]
mark_axes_span(axes, y_stack_factor, span_x, fcolor='gray')
# for ax in axes:
#     ax.axvspan(xmin=span_x[0], xmax=span_x[1], 
#                ymin=0, ymax=1, 
#                facecolor='gray', alpha=0.2, zorder=0)

##----------------------------------------
# show plot
# plt.savefig('sample_stacked_plot_adjusted.svg')
# plt.savefig('sample_stacked_plot_adjusted.png', dpi=200)
plt.show()
```

the output should be like this

![2209e916-7796-4d85-bef4-201136dccb2a](https://github.com/user-attachments/assets/06915d6c-2ebb-4952-b15a-2d8cbb2108e3)

