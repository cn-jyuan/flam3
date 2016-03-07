The interpolation and interpolation\_type attributes control how flames animate from one key-frame to the next.

### Interpolation ###

You may compare [smooth](http://draves.org/pix/clip/smooth-smooth.mov) and [linear](http://draves.org/pix/clip/smooth-linear.mov) interpolation. Both of these animations use
the same keyframes/control points. In the linear, one there are two angles
or jolts in the motion, one at 2 seconds in and the other at 4, but in the
smooth one there are none. They were rendered with frames 30 to 120 of [this](http://draves.org/pix/clip/cr3.flam3)
sequence with this command:
```
env begin=30 end=120 qs=0.1 ss=0.25 flam3-animate < cr3.flam3
```
You can set the interpolation from linear and angular to smooth Catmull-Rom
splines on any keyframe. It then applies to the interpolation from that flame
to the next one. You cannot apply smooth interpolation to the first or last
keyframes of a sequence.

### Interpolation\_type ###

While the interpolation attribute changes what could be thought of as the path of the animation, the interpolation\_type attribute alters the way the xform triangles are moved through space and how interpolations between keyframes with a different order to/different number of xforms. The linear function uses a linear interpolation for each coefficient of a triangle where log converts to polar measurements.

### Special Sauce ###

The following "shortcuts" or improvements are made in interpolation, collectively they are called "special sauce":

First:
For log interpolation only, when padding an xform that uses spherical, ngon, julian, juliascope, polar, wedge\_sph or wedge\_julia, instead of a standard rest position (linear=1, 1 0 0 1 0 0), use a special one (linear=-1, -1 0 0 -1 0 0).

Next, try:
For xforms using rect, rings2, fan2, blob, supershape, curl, or perspective, pad with an identity using that variation (or those variations) with rest position for the coefs (1 0 0 1 0 0) but the parameters are set to make it look like linear=1 (curl with c1=0 and c2=0, for example).

Finally:
If an xform has fan or rings, then use those in the output (the rest position for those variations results in no effect.)