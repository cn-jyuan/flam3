## Using flam3-animate ##

flam3-animate treats the flames in its input as a time-varying animation.
For each frame of output, it [interpolates](Interpolation.md) the inputs to the requested time,
and then renders the result with motion blur.

### Parameters ###

Setting the parameters is explained [above](CommandLineOverview.md).

| **name** | **default** | **meaning** |
|:---------|:------------|:------------|
| prefix   | (empty)     | prefix names of output files with this string. |
| begin    | j           | time of first frame to render (j=first time in input file) |
| end      | n-1         | time of last frame to render (n=last time specified in the input file) |
| time     | NA          | time of first and last frame (ie do one frame) |
| frame    | NA          | synonym for "time" |
| in       | stdin       | name of input file |
| dtime    | 1           | time between frames |
| fields   | 0           | if 1 then render fields, ie odd scanlines at time+0.5 |
| qs       | 1           | quality scale, multiply quality of all frames by this |
| ss       | 1           | size scale, multiply size (in pixels) of all frames by this |
| jpeg     | NA          | jpeg quality for compression, default is native jpeg default |
| format   | png         | "jpg" or "ppm" or "png" |
| pixel\_aspect | 1.0         | aspect ratio of pixels (width over height), eg 0.90909 for NTSC |
| isaac\_seed | random      | string to be used in generating random seed. defaults to time(0) |
| seed     | random      | integer seed for random numbers, defaults to time+pid |
| verbose  | 0           | if non-zero then print progress meter on stderr |
| bits     | 33          | also 16, 32, or 64: sets bit-width of internal buffers |
| transparency | 0           | make bknd transparent, if format supports it |
| nick     | (empty)     | nickname to use in `<edit>` tags / img comments |
| url      | (empty)     | url to use in `<edit>` tags /img comments |
| id       | (empty)     | ID to use in `<edit>` tags / img comments |
| nthreads | auto        | number of threads to use |
| insert\_palette | unset       | insert the palette into the image. |
| enable\_jpeg\_comments | 1           | enables comments in the jpeg header |
| enable\_png\_comments | 1           | enables comments in the png header |
| write\_genome | 0           | write out genome for center of motion blur for each rendered frame |
| earlyclip | 0           | if true then clamp luminance values earlier to avoid aliasing, also increases fuse to 100 from 15 (2.8 only) |

for example:
```
env dtime=5 prefix=foo. in=test.flam3 flam3-animate
```
means to render every 5th frame of parameter file foo.flam3, and store
the results in files named foo.XXXXX.png.

You can put a single static input flame into motion and render a single frame with motion-blur like this:
```
env rotate=input.flam3 frame=0 nframes=50 flam3-genome > anim.flam3
env frame=0 flam3-animate < anim.flam3
```