//(Note: This guide is written for the options available to version 2.7 of the flam3 software.)//

While many folks are content to create new flames from scratch, it is not only possible but very simple to create new flames from old flames by applying some genetic manipulation. flam3-genome allows you to not only mutate existing genomes, but create genetic crosses of two genomes to create something very different from the parents.


---


**Informational Env Vars**

When using flam3-genome, there are new env vars that allow the history of how a particular genome was created to be tracked. These env vars are as follows:

'nick' : Your nickname or handle that you use for posting sheep, etc.
'url' : The address for your website, gallery, blog, etc.
'comment' : A comment string describing your methodology.

These env vars are optional, but will personalize an `<edit>` element within the genome with your information should you provide it. The structure will look something like this in the genome:

```
<edit date="28 Nov 2005 17:07" nick="Erik" action="cross interpolate 0.168697 1">
   <edit filename="cog.flame" index="16"/>
   <edit filename="cog.flame" index="23"/>
</edit>
```

The 'action' means that these two flames were crossed with the interpolate method, using a control point about 17% from the first flame listed to the second flame, and the final 1 means that it used the palette from the second flame to color the result (if it used the first flame's palette, there would be a 0.)


---


Mutate Mode

In 'mutate' mode, a random genome is selected from the file specified by the 'mutate' environment variable, and one of the following mutations (also randomly selected) will be applied:

  * Mutate Variations (10% of the time) : A random flame is generated, and the variations of the random flame are applied to the original flame's xforms.
  * Mutate Xform (20% of the time) : A random xform is picked from the original flame and the coefs randomized.
  * Mutate Symmetry (20% of the time) : The symmetry of the original flame is randomized.
  * Mutate Post (10% of the time) : The post xforms of the original flame are randomized.
  * Mutate Colors (10% of the time) : The palette of the original flame is rotated or randomized (with some quality checking).
  * Mutate Delete (10% of the time) : An xform is randomly deleted from the original flame.
  * Mutate Coefs (20% of the time) : A random flame is generated, and the coefs of the original flame are slowly mutated towards these new coefs.

Additionally, half of the time the image will be reframed to more appropriately fill the image.

As an example of this method, consider the file seeds.flame, which has 10 'nice' flames in it that you would like to mutate. The following command will randomly select and mutate a flame from this file 20 times, and direct the output to the file seeds\_mutated.flame:

```
env mutate=seeds.flame repeat=20 nick="Erik" flam3-genome > seeds_mutated.flame
```


---


**Cross Mode**
In 'cross' mode, elements from one parent file are genetically crossed with elements from a second parent file to create a new flame. There are three methods used to create children from parents:

  * 'alternate' (80% if random) : Xforms are alternately copied from each parent into the new flame.
  * 'interpolate' (10% if random) : a control point is randomly selected between the two parent flames and the new flame is interpolated at this point.
  * 'union' (10% if random) : All xforms are copied from the parents into the new flame (up to the software's limit).

These three methods can be specified on the command line as an environment variable, or you can let the system randomly choose one (with the percentages listed above.) For example, if I wanted to cross genomes from A.flame with genomes in B.flame, 20 times, using interpolation, I would issue the following command:

```
env cross0=A.flame cross1=B.flame method=interpolate repeat=20 nick="Erik" flam3-genome > AB_cross.flame
```

I should note here that a template may be applied to the output of the genetic mutation methods by specifying the 'template' env var.

Each result of the 'cross' or 'mutate' methods is run through a test for image content. The frame is rendered at a very low quality, and the percentage of values in the image that are white (255) and black (0) are calculated. The average value is also calculated. In order to pass this test, the average value must be greater than 20, the fraction of black values must be less than 1%, and the fraction of white values must be less than 5% (by default). These defaults can be changed by setting the 'avg', 'black', and 'white' env vars on the command line. If the genome fails the test, it is discarded and a new mutation/cross is generated.

Additionally, the symmetry of the randomly generated genomes in the 'mutate' method can be manually set with the 'symmetry' env var (if you prefer all mutations to have a particular symmetry setting.)


---


The remaining methods available to flam3-genome are for the purposes of animation:

**Sequence Mode**

A discussion on how to use 'sequence' to generate animation time steps can be found at AnimatingFlames.

Rotate / Interp Mode

The 'rotate' and 'interp' modes of operation for flam3-genome are used to generate genome files that can be used to generate single frames of animation. The 'rotate' env var takes a genome file argument; the first genome in this file will be used to generate the animation genome as follows (using the template file from the AnimatingFlames example):

```
env rotate=A.flame template=anim_template.flame nframes=100 frame=5 flam3-genome > A_005.flame
```

This will generate a file with three genomes, one at time=4, time=5, and time=6, under the assumption that the full rotation will take a total of 100 frames. This file can then be rendered with flam3-animate as follows:

```
env in=A_005.flame frame=5 flam3-animate
```

For 'interp' mode, a combination of a rotation and an interpolation between two genomes is generated. An example (where AB.flame has two genomes):

```
env interp=AB.flame template=anim_template.flame nframes=100 frame=5 flam3-genome > AB_005.flame
```

Again, this will generate a file with three genomes, at t=4, t=5, and t=6, where the rotation and transition takes a total of 100 frames. The render can be done as before.