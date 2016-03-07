Highlight power is a new feature in flam3, and it is a remedy to a longstanding bug that affects the colors of flames in high density areas, where colors up to this point are almost always RGBCMY or white. Here's why.

Consider the color orange. In RGB space, a common orange is represented by (1.0, 0.5, 0.0). Although it's more complex than this in the flam3 algorithm, assume that to calculate the color of a given point, we add up the colors of all of the iterations and divide by the render quality. A saturated area is one where, when you divide by the render quality, some of the color values are still greater than 1.0, the max color value permittable. In these circumstances, we clip the color values to 1.0 - but this is where the problem lies. In all versions of flam3 up through 2.7, we clip each color coordinate INDIVIDUALLY. If there are enough iterations of orange on a single point, then both the red AND green values saturate, are clipped individually, and the final color ends up being (1.0, 1.0, 0.0) - not orange at all, but yellow! This is extremely hard to predict and can result in colors appearing in your images that are not in the flame's palette.

The solution that I devised clips the color coordinates not individually, but as a whole; we clip the color in HSV (Hue Saturation Value) space, at the max Value of the color. Once the color saturates past max Value, we use the new 'highlight\_power' attribute of the `<flame>` element to control how quickly these saturated pixels become white. Setting highlight\_power to -1 turns off the new behaviour and clips colors the old fashioned way. Some examples:

![http://lh4.ggpht.com/_sf5_-QiEqNk/TOhFxdeiY-I/AAAAAAAAJpA/SCed5zQbYqg/hp-00000.orig.jpg](http://lh4.ggpht.com/_sf5_-QiEqNk/TOhFxdeiY-I/AAAAAAAAJpA/SCed5zQbYqg/hp-00000.orig.jpg)

This is a still frame from sheep 244.00977. There are plenty of saturated areas in this image, notably in the upper-right portion of the large circle feature. Now, compare this image with the next one; I have set highlight\_power to 0 so it is easy to see the hue bug.

![http://lh5.ggpht.com/_sf5_-QiEqNk/TOhFzF1029I/AAAAAAAAJpI/xSFiydq1Fdk/hp-00000.0.jpg](http://lh5.ggpht.com/_sf5_-QiEqNk/TOhFzF1029I/AAAAAAAAJpI/xSFiydq1Fdk/hp-00000.0.jpg)

Where did the yellow go? This is a case where the orange color was saturated and became yellow, exactly as I described at the beginning of this article. Fixing the bug and turning off highlights shows the colors that are actually in the palette. Now, increasing the value of highlight\_power will make the saturated parts of the image start to trend towards white. Here are three different highlight\_power settings, 0.5, 1.0 and 2.0.

![http://lh5.ggpht.com/_sf5_-QiEqNk/TOhF1IJIvRI/AAAAAAAAJpQ/y5GczDoeFq4/hp-00000.5.jpg](http://lh5.ggpht.com/_sf5_-QiEqNk/TOhF1IJIvRI/AAAAAAAAJpQ/y5GczDoeFq4/hp-00000.5.jpg)

![http://lh4.ggpht.com/_sf5_-QiEqNk/TOhF2HCjTSI/AAAAAAAAJpY/jCQ5OadaGdo/hp-00000.1.0.jpg](http://lh4.ggpht.com/_sf5_-QiEqNk/TOhF2HCjTSI/AAAAAAAAJpY/jCQ5OadaGdo/hp-00000.1.0.jpg)

![http://lh4.ggpht.com/_sf5_-QiEqNk/TOhF4O-b6CI/AAAAAAAAJpg/zjZarkoTVP8/hp-00000.2.0.jpg](http://lh4.ggpht.com/_sf5_-QiEqNk/TOhF4O-b6CI/AAAAAAAAJpg/zjZarkoTVP8/hp-00000.2.0.jpg)

Set it to -1 to turn it off. There is no upper limit to its value. There is no value (except turning it off) that corresponds to current behavior.