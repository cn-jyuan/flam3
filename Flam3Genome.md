## Using flam3-genome ##
You can use flam3-genome to clone, mutate, and cross flame genomes, and
it can also be used to generate animation sequences and apply templates
to existing flames. Flam3-genome creates and manipulates genomes (parameter sets).

For more info on how to use flam3-genome, see GeneticAlgorithm

the flam3-genome program creates random parameter files. it also mutates,
rotates, and interpolates existing parameter files. for example to
create 10 wholly new control points and render them at normal quality:
```
env template=vidres.flam3 repeat=10 flam3-genome > new.flam3
flam3-render < new.flam3
```
note that, by default, if a random flame is requested and neither
'use\_vars' or 'dont\_use\_vars' are specified, the following variations are
not used for random flame generation: noise, blur, gaussian\_blur,
radial\_blur, ngon, square, rays, and cross.

if you left out the "template=vidres.flam3" part then the size,
quality, etc parameters would be their default (small) values. you
can set the symmetry group:
```
env template=vidres.flam3 symmetry=3 flam3-genome > new3.flam3
env template=vidres.flam3 symmetry=-2 flam3-genome > new-2.flam3
flam3-render < new3.flam3
flam3-render < new-2.flam3
```
Mutation is done by giving an input flame file to alter:
```
env template=vidres.flam3 flam3-genome > parent.flam3
env prefix=parent. flam3-render < parent.flam3
env template=vidres.flam3 mutate=parent.flam3 repeat=10 flam3-genome > mutation.flam3
flam3-render < mutation.flam3
```
Crossover is handled similarly:
```
env template=vidres.flam3 flam3-genome > parent0.flam3
env prefix=parent0. flam3-render < parent0.flam3
env template=vidres.flam3 flam3-genome > parent1.flam3
env prefix=parent1. flam3-render < parent1.flam3
env template=vidres.flam3 cross0=parent0.flam3 cross1=parent1.flam3 flam3-genome > crossover.flam3
flam3-render < crossover.flam3
```
### Parameter files for animation ###
flam3-genome has 3 ways to produce parameter files for animation in
the style of electric sheep. The highest level and most useful from
the command line is the sequence method. It takes a collection of
control points and makes an animation that has each flame do fractal
rotation for 360 degrees, then make a smooth transition to the next.
for example:
```
env sequence=test.flam3 nframes=20 flam3-genome > seq.flam3
flam3-animate < seq.flam3
```
creates and renders a 60 frame animation. There are two flames in
test.flam3, so the animation consists three stages: the first one
rotating, then a transition, then the second one rotating. Each stage
has 20 frames as specified on the command line. If you want to
render only some fraction of a whole animation file, specify the begin
and end times:
```
env begin=20 end=40 flam3-animate < seq.flam3
```
The other two methods, rotate and inter, are harder to use because
they produce files that are only good for one frame of animation. The
output consists of 3 control points, one for the time requested, one
before and one after. That allows proper motion blur. for example:
```
env template=vidres.flam3 flam3-genome > rotme.flam3
env rotate=rotme.flam3 frame=10 nframes=20 flam3-genome > rot10.flam3
env frame=10 flam3-animate < rot10.flam3
```
The file rot10.flam3 specifies the animation for just one frame, in
this case 10 out of 20 frames in the complete animation. C1
continuous electric sheep genetic crossfades are created like this:
```
env inter=test.flam3 frame=10 nframes=20 flam3-genome > inter10.flam3
env frame=10 flam3-animate < inter10.flam3
```
### Parameters ###
See above for how to set them.
| name | default | meaning |
|:-----|:--------|:--------|
| in   | stdin   | name of input file |
| out  | NA      | name of output file (bad idea if rending more than one, use prefix instead) |
| template | NA      | apply defaults based on this genome (genome only) |
| isaac\_seed | random  | string to be used in generating random seed. defaults to time(0) |
| seed | random  | integer seed for random numbers, defaults to time+pid |
| verbose | 0       | if non-zero then print progress meter on stderr |
| use\_vars | -1      | comma separated list of variation #'s to use when generating a random flame |
| dont\_use\_vars | unset   | comma separated list of variation #'s to NOT use when generating a random flame |
| tries | 50      | number of tries to make to find a good genome. |
| cross0, cross1 | NA      | select one flame from each file randomly to genetically cross |
| method | NA      | method for cross: alternate, interpolate, or union. |
| mutate | NA      | select random flame from this file and mutate it |
| symmetry | NA      | set symmetry of result. |
| clone | NA      | clone random flame from input |
| clone\_all | NA      | clone all flames in file - useful to apply a template to all flames |
| animate | NA      | interpolates between all flames in a file, using times specified in file |
| sequence | NA      | 360 degree rotation 'loops' times of each control point plus rotating transitions |
| loops | NA      | number of times to rotate each control point in sequence |
| strip | NA      | strip to render, frame and nframes control which one |
| nick | (empty) | nickname to use in `<edit>` tags / img comments |
| url  | (empty) | url to use in `<edit>` tags /img comments |
| id   | (empty) | ID to use in `<edit>` tags / img comments |
| comment | (empty) | comment string for `<edit>` tags |
| noedits | unset   | omit edit tags from output |
| print\_edit\_depth | 0       | depth to truncate `<edit>` tag structure. 0 prints all `<edit>` tags |
| intpalette | unset   | round palette entries for importing into older Apophysis versions |