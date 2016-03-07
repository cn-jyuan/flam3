The default spatial filter is a Gaussian, but there are 10 options to choose from.

To use these kernels, a new XML attribute has been added to the `<flame>` element, called //filter\_shape//. This attribute takes a string argument, which can be bell, blackman, box, bspline, gaussian, hamming, hanning, hermite, mitchell, quadratic, or triangle. If not specified, the shape defaults to gaussian (to preserve backwards compatibility). Note that the //filter// attribute is a multiplier for the standard 'support' of the shape, so larger values lead to a stronger filter application. (Aside: The original code's Gaussian filter was slightly larger than the default size - about 20% - and that behaviour has been preserved for backwards compatibility. This means that the Gaussian filter is slightly more agressive at similar //filter// settings than other kernels.)

In response to the likely comment that some very important kernels are not present, like Lanczos, Catrom, etc - due to the nature of the data that is being convolved with the kernel, strongly negative values introducing ugly artifacts were observed when using kernels with negative values. For this reason these generally more accepted kernels are not yet included in flam3 - once the artifacts can be dealt with appropriately, they may be added in.

Below are examples of one of the sample images that ships with the flam3 code with different //filter\_shape// and //filter// settings. All renders were performed with oversample=3 and quality=500.


![http://lh3.ggpht.com/_sf5_-QiEqNk/TOhD7KIeFYI/AAAAAAAAJok/dpcop05-aQQ/s912/filter-grid.jpg](http://lh3.ggpht.com/_sf5_-QiEqNk/TOhD7KIeFYI/AAAAAAAAJok/dpcop05-aQQ/s912/filter-grid.jpg)

(note these images have been jpg compressed which interferes with some of the fine distinctions between these kernels)

![http://lh3.ggpht.com/_sf5_-QiEqNk/TOhEuIAu7aI/AAAAAAAAJow/X92JVzfsRmk/s912/filter-grid2.jpg](http://lh3.ggpht.com/_sf5_-QiEqNk/TOhEuIAu7aI/AAAAAAAAJow/X92JVzfsRmk/s912/filter-grid2.jpg)