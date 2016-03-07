You can set a parameter gamma\_threshold to control how much darkening and spackle removal to apply. The default is 0.01 but you can set it to anything from 0 to 1. Set it like this in a genome file:
```
<flame palette="15" size="640 480" scale="240" quality="10" brightness="4" gamma="4" vibrancy="1" gamma_threshold="0.05"> .... </flame>
```
The gamma correction is only applied to colors brighter than the gamma threshold. Darker pixels have a linear correction.