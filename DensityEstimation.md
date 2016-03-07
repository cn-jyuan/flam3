The flame algorithm is like a Monte Carlo simulation, with the flame quality directly proportional to the number of iterations of the simulation. The noise that results from this stochastic sampling can be reduced by blurring the image, to get a smoother result in less time. One does not however want to lose resolution in the parts of the image that receive many samples and so have little noise.

To solve this problem, we have implemented a form of adaptive density estimation to increase image quality while keeping render times to a minimum.

Much of the approach used in the FLAM3 algorithm is based on a simplification of the methods presented in //Adaptive Filtering for Progressive Monte Carlo Image Rendering//, a paper presented at WSCG 2000 by Frank Suykens and Yves D. Willems. Their discussion of variable width kernels in section 3.5.1 of the paper is far better than any summary I can produce here. However, the approach used by the flame implementation is not as complex as the specific methods identified in the paper, and can be discussed in detail.

We can consider the intermediate product of the FLAM3 algorithm a two dimensional histogram, where each pixel is a bin. The count in each bin is incremented each time an iterated point falls within the bin's spatial boundaries. Areas of low density (small counts per bin) result in 'speckle noise', which can be seen in Figure 1. (For clarity in presentation, the flame is rendered on a white background, although most flames are rendered on a black background.)

![http://lh3.ggpht.com/_sf5_-QiEqNk/TOhCRdaFanI/AAAAAAAAJoM/Lgq3j3MuT-8/de-figure1.jpg](http://lh3.ggpht.com/_sf5_-QiEqNk/TOhCRdaFanI/AAAAAAAAJoM/Lgq3j3MuT-8/de-figure1.jpg)

While the speckle noise is accurate from a Monte Carlo standpoint, from an aesthetic point of view we would prefer the image to have a smoothly varying intensity rather than pixelization. Removing it by increasing the number of iterations is not an option due to time constraints, so instead, we make the assumption that the noise is indicative of a more widespread lower density area. By blurring the lower density areas and keeping the higher density areas intact, we can approximate higher quality (using more iterations) without the computational resources. We do this by spreading the low-density iterations with an Epanechnikov kernel, and the width of the kernel is related to the number of iterations in a bin by the relationship

KernelWidth = MaxKernelRadius / (Density^Alpha)

The MaxKernelRadius and Alpha terms are controlled by the user as rendering parameters, although default values have been chosen that tend to produce aesthetically pleasing results. Additionally, there is a MinKernelRadius that forces all bins (regardless of density) to be blurred by a small kernel as an anti-aliasing technique, although this method introduces bias into the image. The results of this new technique can be seen in Figure 2.

![http://lh6.ggpht.com/_sf5_-QiEqNk/TOhCSjbvwVI/AAAAAAAAJoU/NmbqBGmiwro/de-figure2.jpg](http://lh6.ggpht.com/_sf5_-QiEqNk/TOhCSjbvwVI/AAAAAAAAJoU/NmbqBGmiwro/de-figure2.jpg)

Not only does the density estimation algorithm give better results with fewer iterations, but it also creates a perceived ‘depth-of-field’ effect that (depending on the observer) enhances the artistic quality.

To control the density estimation code, there are three attributes defined for the `<flame>` element of a genome: estimator\_radius, estimator\_minimum, and estimator\_curve. These are equivalent to the MaxKernelRadius, MinKernelRadius, and Alpha in the equations above. To disable density estimation, simply set estimator\_radius to "0" in the flame element. The default settings are estimator\_radius="9", estimator\_minimum="0", and estimator\_curve="0.4".