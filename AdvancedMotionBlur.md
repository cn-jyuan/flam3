Here are examples of what you can do with temporal\_filter\_type, temporal\_filter\_width, and temporal\_filter\_exponent.

For some time, flame animations have been rendered by slowly changing xforms and parameters and iterating for a fraction of the total //quality// at each time step. The iterations are uniformly distributed across all of the //temporal\_samples// specified in the `<flame>` tag. This results in images that look like the following sample (sheep 118163, flock 202, frame 10 of 80):

![http://lh4.ggpht.com/_sf5_-QiEqNk/TOgyaB1P5PI/AAAAAAAAJm4/-KVwpdTDfWY/mb-00010-std.jpg](http://lh4.ggpht.com/_sf5_-QiEqNk/TOgyaB1P5PI/AAAAAAAAJm4/-KVwpdTDfWY/mb-00010-std.jpg)

One alternative to the uniform distribution of points is to set the //temporal\_filter\_type// attribute to "gaussian". The iterations will appear clustered near the middle of the motion rather than uniformly spanning all of the temporal samples.

//temporal\_filter\_type//="gaussian":

![http://lh5.ggpht.com/_sf5_-QiEqNk/TOgyalnaiAI/AAAAAAAAJm4/RsmgvyqQw84/mb-00010_g1.jpg](http://lh5.ggpht.com/_sf5_-QiEqNk/TOgyalnaiAI/AAAAAAAAJm4/RsmgvyqQw84/mb-00010_g1.jpg)

We also now can expand the motion blur past -0.5/+0.5 time steps using the //temporal\_filter\_width// attribute.

//temporal\_filter\_type//="gaussian" //temporal\_filter\_width//="2":

![http://lh6.ggpht.com/_sf5_-QiEqNk/TOgybK5BCZI/AAAAAAAAJm4/VnWrLP7mDpQ/mb-00010_g2.jpg](http://lh6.ggpht.com/_sf5_-QiEqNk/TOgybK5BCZI/AAAAAAAAJm4/VnWrLP7mDpQ/mb-00010_g2.jpg)

To get directional motion rather than blurred motion, set //temporal\_filter\_type// to "exp", and use the //temporal\_filter\_exponent// attribute to control the speed and direction of the motion. //temporal\_filter\_exponent// defaults to 0.0, which is omnidirectional (for backwards compatibility). If //temporal\_filter\_exponent// is > 0, then later time steps are emphasized, and if < 0, earlier time steps are emphasized. Using the same flame:

![http://lh4.ggpht.com/_sf5_-QiEqNk/TOgyb1ckWHI/AAAAAAAAJm4/Mb3G6-n4bkc/mb-00010_lp2.jpg](http://lh4.ggpht.com/_sf5_-QiEqNk/TOgyb1ckWHI/AAAAAAAAJm4/Mb3G6-n4bkc/mb-00010_lp2.jpg)

//temporal\_filter\_type//="exp" //temporal\_filter\_exponent//="-2.0":

![http://lh3.ggpht.com/_sf5_-QiEqNk/TOgycRaRq7I/AAAAAAAAJm4/gsEQ9naSU1k/mb-00010_ln2.jpg](http://lh3.ggpht.com/_sf5_-QiEqNk/TOgycRaRq7I/AAAAAAAAJm4/gsEQ9naSU1k/mb-00010_ln2.jpg)