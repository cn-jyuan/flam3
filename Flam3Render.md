## Using flam3-render ##

Flam3-render reads an input file and renders each flame in it to an image file on disk.

The default output format for all of the flam3 render tools is PNG,
without transparency. All of of the environment variables that
apply to the flam3-render program are listed below, with more frequently
used options near the top of the list.

## Parameters ##

Setting the parameters is explained here: CommandLineOverview.

| **name** | **default** | **meaning** |
|:---------|:------------|:------------|
| in       | stdin       | name of input file |
| out      | NA          | name of output file (use prefix if rendering more than one image) |
| prefix   | (empty)     | prefix names of output files with this string. |
| nstrips  | 1           | number of strips, ie render fractions of a frame at once |
| qs       | 1           | quality scale, multiply quality of all frames by this |
| ss       | 1           | size scale, multiply size (in pixels) of all frames by this |
| format   | png         | "jpg" or "ppm" or "png" |
| jpeg     | NA          | jpeg quality for compression, default is native jpeg default |
| transparency | 0           | make bknd transparent, if format supports it |
| verbose  | 0           | if non-zero then print progress meter on stderr |
| fields   | 0           | if 1 then render fields, ie odd scanlines at time+0.5 |
| pixel\_aspect | 1.0         | aspect ratio of pixels (width over height), eg 0.90909 for NTSC |
| isaac\_seed | random      | string to be used in generating random seed. defaults to time(0) |
| seed     | random      | integer seed for random numbers, defaults to time+pid. |
| bits     | 33          | also 16, 32, or 64: sets bit-width of internal buffers |
| image    | filename    | replace palette with png, jpg, or ppm image |
| name\_enable | 0           | use 'name' attr in `<flame>` to name image output if present |
| nick     | (empty)     | nickname to use in `<edit>` tags / img comments |
| url      | (empty)     | url to use in `<edit>` tags /img comments |
| id       | (empty)     | ID to use in `<edit>` tags / img comments |
| use\_mem | auto        | floating point number of bytes of memory to use (render only) |
| nthreads | auto        | number of threads to use |
| insert\_palette | unset       | insert the palette into the image. |
| enable\_jpeg\_comments | 1           | enables comments in the jpeg header |
| enable\_png\_comments | 1           | enables comments in the png header |
| earlyclip | 0           | if true then clamp luminance values earlier to avoid aliasing, also increases fuse to 100 from 15 (2.8 only) |