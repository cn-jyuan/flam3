The last major release of flam3 was in 2006, and since there has been a lot of development behind the scenes of features and improvements.  Here's a summary of what has been added over the last few years.


---


## Speed ##

All the code as been optimized, giving you results about 30% faster.

## Highlight Power ##

A long-standing bug in the way that the output color is calculated has been fixed, and a new render tweak setting is now available to adjust the behavior of flam3.  Here's a picture of the problem:

![http://dl.dropbox.com/u/108435/flam3_wiki/2229.hlm1.png](http://dl.dropbox.com/u/108435/flam3_wiki/2229.hlm1.png)

See that bright yellow spot at the top of the rounded shape?  There's a little yellow in the palette, but nowhere near the amount that is seen in this image.  It's a result of clipping each color coordinate separately in regions of high iteration density.  This is why, for a long time, RGBCMY colors have tended to dominate high-density areas of flame renders.  The solution is to keep the color vector pointing the same direction until it begins to saturate, and then trend the color towards white as the number of iterations continues to increase. The attribute 'highlight\_power' in a genome's flame element controls how fast the color converges to white.  highlight\_power interpolates just like other parameters in the flame, and defaults to -1, which is the old 2.7 behaviour.  Changing the highlight\_power to 1.0 results in the following image:

![http://dl.dropbox.com/u/108435/flam3_wiki/2229.hl10.png](http://dl.dropbox.com/u/108435/flam3_wiki/2229.hl10.png)

Yuk.  Notice that the vivid red color is now trending towards white for the entire shape, resulting in a pink hue.  Let's drop the highlight\_power to 0.4.

![http://dl.dropbox.com/u/108435/flam3_wiki/2229.hl04.png](http://dl.dropbox.com/u/108435/flam3_wiki/2229.hl04.png)

Much better.  Note that setting highlight\_power to 0 will prevent any saturated areas from trending towards white, essentially capping saturated areas to a color with saturation 255 in the HSV colorspace.

The highlight\_power setting can be edited manually, or experimented with using the fractal fr0st flame editor on the Adjust tab.


---


## Early Clip Mode ##

In the flam3 2.7 code, when applying the spatial filter to the supersampled image, the values in the supersampled image are in log-scaled units, and can be very small or very large.  Applying a filter kernel to log density data before converting it to RGB can have some potential problems, as the following image shows:

![http://dl.dropbox.com/u/108435/flam3_wiki/23652_ec_off.png](http://dl.dropbox.com/u/108435/flam3_wiki/23652_ec_off.png)

See the jaggies on the bright spokes on the left side of the image?  The spokes are very dense, and so applying the spatial filter to the log density data blurs the spokes, but the blurred areas are also bright enough to saturate, and we are left with a wider, but not smoother, swath of iterations.  However, if we clip the color information prior to the spatial filter application, we get a very different image.  This is, in essence, rendering the image very large with no supersampling, and resizing in an external program like Photoshop or Irfanview.

![http://dl.dropbox.com/u/108435/flam3_wiki/23652_ec_on.png](http://dl.dropbox.com/u/108435/flam3_wiki/23652_ec_on.png)

The lines are much smoother - but they're dimmer, too.  You can't easily have it both ways.  That said, look at the results when the image is rendered with both early clip mode AND highlight\_power = 1.5 :

![http://dl.dropbox.com/u/108435/flam3_wiki/23652_ec_hl.png](http://dl.dropbox.com/u/108435/flam3_wiki/23652_ec_hl.png)

This is a much more acceptable image, at least to me.  The cyan saturation has gone away, revealing more detail & structure.

Enabling early clip mode is done on the command-line, by setting the env var 'earlyclip' to 1.  There is also a checkbox in fr0st that allows the use of early clip mode in the render dialog.


---


## Symmetry, Animate & Color Speed ##

Up until flam3 3.0, the symmetry value of an xform was used to control the amount of color the most recent iteration contributed to the aggregate color.  Positive values of symmetry would also prevent rotation of an xform during sheep animations.  This limitation made many sheep very hard to color - so in 3.0, we have split symmetry into two new attributes, animate and color\_speed.  Setting animate > 0 indicates that an xform should rotate during sheep animations; animate = 0 prevents it from rotating.  Color\_speed is a much more intuitive value to use for specifying how much that xform's color coordinate blends into the aggregate...color\_speed = 0 means that the new color will be the old color exactly, no effect.  color\_speed = 1 means that the new color is the color of the xform, with no blending with the old color.  color\_speed = 0.5 means average them together.

Note that flames using symmetry can be read by flam3 3.0, and will be converted to equivalent animate and color\_speed settings internally.  You may not be able to convert back to 2.7, however, depending on your animate setting.  There is an animate checkbox in fr0st, as well as a slider for color\_speed.


---


## Motion Elements ##

There is a new concept in 3.0, that of motion elements.  Motion elements are sub-elements of xforms that can periodically vary most of the parameters associated with an xform during sheep animation.  For example, one could specify that the weight of the spherical variation should vary from 2 to 4, sinusoidally, 3 times during a sheep loop.  This is a very powerful feature that has yet to be explored, and will be covered in detail on a different page.


---


## Variations ##

There are now 98 variations supported by flam3!  See the complete list in the README file.  Most of the official plugin pack for Apophysis is now supported, as well as others that were requested (or easy.)


---


## Stagger Mode ##

Ever want to do a sheep-like edge, but have the xforms interpolate one at a time instead of all at once?  Setting the env var 'stagger' allows this behavior.  For example, stagger=0 means that xforms will interpolate all at once (the default.)  stagger = 0.5 means that the interpolation of each xform is staggered so that when the first xform is half done, the second one starts, and so on.  stagger = 1.0 means that each xform interpolates consecutively with no overlap.


---


## Apophysis Chaos supported ##

Apophysis-like chaos and plotmode have been implemented, with some very slight differences.  Plotmode has been promoted to 'opacity' so that it can be interpolated, and chaos can also be interpolated.