Motion blur for animation is controlled with the temporal\_samples parameter. The default is off, which is 1 sample and hence no blur. Setting temporal\_samples="10" makes the flame be rendered 10 times, each at a slightly different time, and averages the results together. Any parts of the flame that are in motion will be blurred. If they are moving fast enough, then the blur breaks apart and you can see a stroboscopic effect. In this case, increase the number of temporal samples.

You can also create motion blur by increasing the number of batches. The [[Estimation|Density Estimator](Density.md)] runs for each batch, so this is slower than using temporal samples, but the results are more realistic. Batches are also useful when using smaller buffers (16 and 32 bits) to avoid saturation. You probably don't want to use them.

If you only have a single genome not an animation, you have to put it in motion before you can see any blur.
You can make an animation from a normal input flame and then render a blurred image like this:

```
env rotate=input.flam3 frame=0 nframes=50 flam3-genome > anim.flam3
env frame=0 flam3-animate < anim.flam3
```

Reduce the number of frames to get more blur (Or increase it to get less).