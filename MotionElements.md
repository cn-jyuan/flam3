New in the 3.0 series of flam3 is the concept of motion elements. Motion elements are a new bit of xml that accompanies an xform to describe modifications to the standard Sheep Loop (rotating non-stationary xforms about their origins); using these elements, you can periodically vary almost any parameter in an xform over time.

  * Motion elements are contained by xform elements. A single xform can contain multiple motion elements.
  * There are two new attributes available in a motion element: 'motion\_function' and 'motion\_frequency'. 'motion\_function' can take the values "sin","hill", or "triangle" (defining the shape of the periodic change), and 'motion\_frequency' is an integer defining how many cycles the parameter will make during a Sheep Loop. The "sin" motion function produces uses a sine wave, "triangle" uses a triangluar wave, and "hill" will use (1-cos(x))/2, or in other terms sin^2(x). This is also sinusoidal motion, but instead will begin from a stationary state, rather than immediate motion.
  * The value of each parameter/variation/coefficient in the motion element is the magnitude of the periodic function that is added to that parameter/variation/coefficient during the loop animation.
  * Most parameters and variables in an xform may be modified by a motion element (chaos is not supported).
  * Motion elements only have an effect in sheep loop animations and edges, not in morphs.



For example, the xforms for sheep 243.00575 look like this:
```
<xform weight="0.255" color="0" symmetry="0" linear="0.94" swirl="0.06" coefs="-0.646904 -0.617942 0.600517 -0.614845 1.72489 -2.36409"/>
<xform weight="0.95" color="1" symmetry="0" swirl="0.279721" coefs="0.771296 -0.74874 0.053767 0.048049 0.375756 -0.359987"/>
<xform weight="1" color="0" symmetry="1e-06" julia="1.24" coefs="-1.00899 0.044748 0.053403 1.01339 0 0"/>
<xform weight="0.85" color="1" symmetry="0" linear="1" coefs="0.379274 -0.19325 0.19325 0.379274 0 0"/>
<xform weight="0.425" color="0" symmetry="1" linear="1" coefs="0.444075 -0.541107 0.541107 0.444075 0 0"/>
<finalxform color="1" symmetry="1" julia="1" coefs="0.018183 2.761 -2.761 0.018183 0 0"/>
```
Using flam3-genome and sequence, with nframes=100, we get this animation:
<a href='http://www.youtube.com/watch?feature=player_embedded&v=tyBstRUjRdk' target='_blank'><img src='http://img.youtube.com/vi/tyBstRUjRdk/0.jpg' width='425' height=344 /></a>

Let's add a motion element to the final xform, like this:
```
<finalxform color="1" symmetry="1" julia="1" coefs="0.018183 2.761 -2.761 0.018183 0 0" >
    <motion motion_frequency="2" motion_function="sin" sinusoidal="0.3"/>
</finalxform>
```
...and re-render the same 100 frames. The result is radically different:

<a href='http://www.youtube.com/watch?feature=player_embedded&v=ZQcmXc9G97I' target='_blank'><img src='http://img.youtube.com/vi/ZQcmXc9G97I/0.jpg' width='425' height=344 /></a>