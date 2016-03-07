## XML file format ##
This documents the file format used by the standard implementation to specify individual images and animations. The XML meta-format is used to facilitate predictable parsing and compatibility. No formal DTD is available, but the important part is the semantics of the recognized elements and attributes, and those are given here.

The basic template is:
```
<flame> <xform/> <xform/> ... <color/> <color/> ... </flame>
```

The attributes of the flame element are:
| **Name** | **Type** | **Description** |
|:---------|:---------|:----------------|
| //**General Attributes**// | | |
| time     | float    | Time (frame number) when these parameters are realized. |
| name     | string   | name of flame   |
| url      | string   | Website of designer |
| nick     | string   | Name or nickname of designer |
| interpolation | string   | how flames change over time. "smooth" enables catmull-rom interpolation, "linear" for piecewise straight lines, see [[Interpolation|here]] . |
| palette\_interpolation | string   | "hsv" (default) or "sweep" methods of interpolating between colormaps |
| palette\_mode | string   | "step" (default) or "linear". how to interpolate between entries in the palette. |
| interpolation\_type | string   | method for [[Interpolation|interpolating]] xform positions: options are "log","linear","old" |
| //Camera Attributes// | | |
| size     | int int  | The width and height in pixels of the output image. |
| center   | float float | The coordinates of the center of the output image |
| scale    | float    | The number of pixels per unit in the image plane. |
| rotate   | float    | Global camera rotation angle of the image plane. |
| zoom     | float    | Scale the image (the camera, not the number of pixels) by this power of two. Also scales the quality to compensate. Together the size, center, scale, and zoom attributes specify the camera that maps the image plane to output image. |
| //Temporal and Spatial Filter Attributes// | | |
| filter\_shape | string   | Shape of redux filter to use. Defaults to "gaussian" but other options are hermite, box, triangle, bspline, mitchell, blackman, hanning, hamming, quadratic. See [[Reduction Kernels](Image.md)]. |
| filter   | non-negative float | The multiplier on the size of the kernel for the filter used to resize the oversampled image down to the output image. |
| temporal\_filter\_type | string   | Kernel used to describe the relative importance of certain time steps within a motion blur. "box" (default), "exp" or "gaussian" |
| temporal\_filter\_width | float    | length of motion blur in units of frames |
| temporal\_filter\_exp | float    | exponent controlling direction and rate of motion blur for "exp" mode temporal filter type. |
| //Color Attributes// | | |
| background | float float float | The color of the background as an RGB triple with each component in `[0, 1]`. |
| brightness | positive float | Linear scale on luminance. |
| gamma    | positive float | The inverse exponent. Values larger than one brighten the dark parts of the image. Usually between 2 and 4. |
| vibrancy | float    | Blend between applying gamma to each channel independently (zero) for pastel and ghostly white images, and applying gamma from the alpha channel to each channel (one) for saturated colors. |
| palette  | int      | One of 701 standard color-maps that are delivered with flam3. |
| hue      | float    | Added to the hue component of the colors from the palette. 0.5 means to rotate 180 degrees in color space. |
| gamma\_threshold | float default=0.01 | Linearization threshold, see [[Threshold](Gamma.md)] |
| //Rendering Attributes// | | |
| quality  | positive float | The number of samples per pixel on average. |
| supersample | positive integer | The degree of spatial oversampling on both axes. Memory use is quadratic in this parameter. Typically less than 4. |
| oversample | positive integer | Deprecated, old name for supersample. Do not use. |
| temporal\_samples | integer  | The number of time samples used for motion blur. |
| passes   | positive integer | The number of temporal sub-samples, though also useful for preventing clamping even when not doing animation. Larger values reduce the resolution of the logarithm. Unless you understand this feature very clearly, you should probably be using temporal\_samples. |
| //[[Estimation](Density.md)]// | | |
| estimator\_radius | integer default=9 | MaxKernelRadius, 0=disabled |
| estimator\_minimum | integer default=0 | MinKernelRadius |
| estimator\_curve | float default=0.4 | Alpha           |

Xform, symmetry, and color elements may appear inside the flame element. An xform element specifies a function in the function system, and a symmetry element specifies a collection of functions as described above. Color elements provide finer control over the colors than the palette attribute.

The attributes of the xform element are:

| weight | non-negative float | The relative probability that this function will be chosen by the chaos game. |
|:-------|:-------------------|:------------------------------------------------------------------------------|
| color  | float              | The color index associated with this function, normally in [0,1].             |
| symmetry | float              | <= 0 means this xform animates, also controls color influence.                |
| coefs  | 6 floats           | The coefficients for the affine part of the function, in row order (a d b e c f). |
| post   | 6 floats           | The coefficients for the affine part of the post function, in row order (a d b e c f). |
| var    | up to 14 floats    | Deprecated: The variational coefficients //vi j// for this function //i//. Any coefficients not given are assumed to be zero. |
| var1   | integer            | Deprecated: Shorthand for a var attribute with all coefficients zero except the one specified is one. So var1="2" is equivalent to var="0 0 1 0 0 ..." and var1="0" to var="1 0 0 ...". |

In addition, each of the variations names may appear as an element of an xform. Its value is a float that is the variational coefficient. For example:

```
<xform weight="0.103" color="0" symmetry="0" julia="0.6" coefs="0.531292 -0.369204 0.189104 0.990545 -1.55337 -0.507086"/>
```
and
```
<xform weight="0.132" color="0.1" symmetry="0" linear="0.13" spherical="0.03" heart="0.07" disc="1" julia="1" coefs="-0.02833 0.14165 -1.21818 -0.509937 0.790042 -1.38261"/>
```

The symmetry element has one attribute kind with a non-zero integer value that is the degree of rotational symmetry. A negative value means a dihedral symmetry. Kind one means no symmetry.

The color element has two attributes: index and rgb. The index value is an integer in [0,255] and the rgb value is three integers in [0,255]. It sets a single entry of the color map, so you need 256 of them. You cannot specify both a palette attribute and color elements.

For example:

```
<flame ...>
  <color index="0" rgb="20 12 123"/>
  <color index="1" rgb="21 13 130"/>
  ...
  <color index="255" rgb="200 42 0"/> 
<flame/>
```

Flames can also have edit histories that record their family lineage and any other changes to the genome, like this:

```
<edit date="Fri Jul 18 19:21:37 EDT 2008" id="888" action="clone brood">
  <edit date="Tue Jul 15 04:12:39 EDT 2008" id="766" action="cross alternate 1: 0 0 1 1 cmap_cross 0: 34 137 156 174 improved colors" filename="upload" index="0">
    <edit date="Tue Jul 1 11:07:02 EDT 2008" nick="m2" url="www.myspace.com/mthatsalowercasem2" id="304" action="clone upload" filename="spex" index="0">
      <edit filename="upload" index="0"/>
    </edit>
    <edit date="Wed Jun 25 00:21:20 EDT 2008" id="104" action="clone brood" filename="spex" index="0">
      <edit date="Tue Jun 24 02:42:36 EDT 2008" id="82" action="cross alternate 1: 1 0 0 1 cmap_cross 0: 7 74 179 221" filename="upload" index="0">
      <edit date="Sun Mar 26 15:48:04 PST 2006" action="genebank 198 20903" filename="genebank.flam3" index="579"/>
      <edit date="Sun Mar 26 15:46:19 PST 2006" action="genebank 191 16594" filename="genebank.flam3" index="181"/> 
    </edit>
  </edit>
</edit>
</edit>
```