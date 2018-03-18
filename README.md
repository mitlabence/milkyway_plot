## mw_plot

A handy python script to plot things on a face-on milkyway using pylab

Both MW.png and MW_galactocentric.png is modified from an images by **NASA/JPL-Caltech/R. Hurt (SSC/Caltech)**
Both images are 7500x7500px with resolution of 24.2 lightyears per pixel, mw_plot will fill black pixel for region
outside the pre-compiled images.

## Basic Usage

```python
from mw_plot import MWPlot
from astropy import units

# setup a MWPlot instance
plot_instance = MWPlot()

# Here are some setting you can set
plot_instance.unit = units.kpc  # units of the plot (astropy.units)
plot_instance.coord = 'galactocentric'  # can be 'galactocentric' or 'galactic'
plot_instance.center = (0, 0) * units.kpc  # Coordinates of the center of the plot
plot_instance.radius = 90000 * u.lyr  # Radius of the plot
plot_instance.figsize = (20, 20)
plot_instance.dpi = 200
plot_instance.cmap = 'viridis'  # matplotlib cmap: https://matplotlib.org/examples/color/colormaps_reference.html
plot_instance.s = 50.0  # make the scatter points bigger

# Here is the mw_plot if you have an array to color the point
# x and y must both carry astropy unit
plot_instance.mw_plot(x, y, [z, 'colorbar_title'], 'Title of the plot here')

# Here is the mw_plot if you do not have array to color the point
# x and y must both carry astropy unit
plot_instance.mw_plot(x, y, 'scatter_point_color_here', 'Title of the plot here')

# To plot
plot_instance.plot()

# To save
plot_instance.savefig('name.png')
```

## Example: plotting orbits of Sun integrated by galpy

![](example_plot_1.png)

You can plot the orbit which are some scatter points on a face-on milkyway

```python
from galpy.potential import LogarithmicHaloPotential
from galpy.orbit import Orbit
import numpy as np
from astropy import units as u
from mw_plot import MWPlot

# Orbit Integration using galpy for the Sun
# see http://galpy.readthedocs.io/en/latest/orbit.html for detail
op= Orbit(vxvv=[-8.*u.kpc,22.*u.km/u.s,242*u.km/u.s,0.*u.kpc,22.*u.km/u.s,0.*u.deg])
lp= LogarithmicHaloPotential(normalize=1.)
ts= np.linspace(0,20,10000)
op.integrate(ts,lp)
x = (op.x(ts))*u.kpc
y = op.y(ts)*u.kpc
z = op.z(ts)

# setup a MWPlot instance
plot_instance = MWPlot()
plot_instance.unit = u.kpc
plot_instance.coord = 'galactocentric'

# plot
plot_instance.mw_plot(x, y, [z, 'kpc above galactic plane'],
                      'Orbit of Sun in 20Gyr using galpy colored by potential')

# Save the figure
plot_instance.savefig(file='mw_plot.png')

# Show the figure
plot_instance.show()
```

![](example_plot_2.png)

You can set the center point and radius of the plot. In this case, we set (0, -8) in a galactocentric coordinates
such that the plot centered at the Sun, and set the plot radius as 5 kpc to close up on the Sun.

```python
from galpy.potential import LogarithmicHaloPotential
from galpy.orbit import Orbit
import numpy as np
from astropy import units as u
from mw_plot import MWPlot

# Orbit Integration using galpy for the Sun
# see http://galpy.readthedocs.io/en/latest/orbit.html for detail
op= Orbit(vxvv=[-8.*u.kpc,22.*u.km/u.s,242*u.km/u.s,0.*u.kpc,22.*u.km/u.s,0.*u.deg])
lp= LogarithmicHaloPotential(normalize=1.)
ts= np.linspace(0,20,10000)
op.integrate(ts,lp)
x = (op.x(ts))*u.kpc
y = op.y(ts)*u.kpc
z = op.z(ts)

# setup a MWPlot instance
plot_instance = MWPlot()
plot_instance.unit = u.kpc
plot_instance.coord = 'galactocentric'

# Set the center and radius of the plot
plot_instance.center = (0, -8) * u.kpc
plot_instance.radius = 5 * u.kpc

plot_instance.s = 50.0  # make the scatter points bigger

# plot
plot_instance.mw_plot(x, y, 'r', 'Orbit of Sun in 20Gyr using galpy')

# Save the figure
plot_instance.savefig(file='mw_plot_zoomed.png')

# Show the figure
plot_instance.show()
```

## Author

* **Henry Leung** - *Initial work and developer* - [henrysky](https://github.com/henrysky)\
*Astronomy Undergrad, University of Toronto*\
*Contact Henry: henrysky.leung [at] mail.utoronto.ca*

## License
This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details