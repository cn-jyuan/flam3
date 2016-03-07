## Usual methods ##

Apophysis has a number of built-in methods of creating new sheep:
  * Check out the random batch that Apophysis creates on startup.
  * Use the Mutation window to generate... mutants.
  * Play around in the Editor window to tweak the transforms.
    * Play with transform weights
  * Try running some of the scripts -- you'll soon find that some are better than others. (Make sure to at least try Animate and Variation Spiral.)

## Collected wisdom ##

From the mailing list:
  * Remix! Grab some cool sheep off the server, especially edges, and play with the middle frames. (Try to understand how the cool ones work.)
  * Run the Default Animation script and stop it at various points, testing the resulting flames.
  * Prevent one of the transforms from changing during the loop by setting its symmetry to 0.00001 -- only transforms with symmetry == 0 will be rotated during the loop. This has almost no effect on the colors, but an immense impact on the loop. Create a "base" out of stopped transforms, then add to that. Especially play with symmetry 0.01 to 0.99.
  * Play with symmetry settings, both in the Editor window and by using scripts.
  * Create random batches with only one variation -- that will show you how each variation works.
  * Use scale (not zoom.)
  * Don't worry if a flame looks good but doesn't animate well -- it may not make a good sheep right away, but it might be good fodder.
  * Keep intriguing flames and sheep around as fodder -- some may have the potential to develop into many diverse sheep.
  * Modify scripts to show the frame number. For example, in the Animate script, add these lines before the "Preview;" statement:
{{msg:='Frame: '+ IntToStr(i); print(msg);}}

## Workflow ##

From Chris:

I think it's important to have your studio set up nicely and I have tried all kinds of configurations but I found [this setup](http://img343.imageshack.us/my.php?image=ursittiaposetup0bc.jpg) works best for me.
I recently stopped opening the mutation window, as, although it pretty to look at, it's a distraction and I rarely have ever used it. Plus with the new 2.04, the triangles stick to the cursor till they finish rendering...and that is very annoying.

I like to work very fast and I don't think of making "art" in the process. The first actions are very lateral with extreme movements of my forms in every which direction. I run the sheep loop and if it's interesting I save it and continue the process. The lateral part also involves the use of some scripts mixing and matching them in no particular order. The chief script I use is the default animation but I also altered the sheep loop script to update so I can have a new starting point. It's simple to do, just replace update FALSE to update TRUE at the very end of it. So in short part one is wild experimentation. Next comes the focus or fine tuning part where I will look over my saves, pick the most interesting one and work on keeping it all in the picture plane. Forms 1,2 and 3 usually do this for me. Then when it's done...render and post!

So in short, my approach involves wild experimentation/intuition with a disregard for any final outcome.
I agree that tweaking is a good way to learn but I can say much of my work grew out of the five basic parameters that were downloaded with APO2.02. I am particularly fond of the spherical variation and try not to over post my experiments with them...but I find them exciting!

Then, usually, once a week, I collect my dead sheep and their unborn siblings and work with their Flam3 transparent PNG renderings and work them into a collage. (Now if I could only get them all to move together!)
Push it to the extreme!...mistakes lead to discovery!