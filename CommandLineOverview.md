The flam3-render program reads flame elements from its input and renders each one as an output image. The flam3-animate program reads flame elements and interprets them as key-frames for an animation. It renders one output image per integer frame number, starting at time 0 and ending at the time of the last key-frame minus one. It interpolates values linearly in-between key-frames.

Depending on whether you're using a DOS-like interface on a Windows box, or a shell on a Unix/Linux/Mac machine, there are two different, but very similar, methods. These are likely to change in the future to true command-line argument functionality, but for now, the arguments are passed in to the programs with environment variables.

In a Unix/Linux/Mac shell, you can pass these arguments in using the 'env' command. 'env' allows you to set environment variables and then execute a command without permanently exporting these values - although you'll probably want to do this with some of the more commonly set values (like your nickname), using the 'export' command. So, for example, to render a flame whose genome was called 'test.flame' to a JPG file called 'test.jpg', use
```
env in=test.flame out=test.jpg format=jpg flam3-render 
```
You can also read a flame file in from stdin, like this:
```
env out=test.jpg format=jpg flam3-render < test.flame
```
For Windows, unless you have the Cygwin package installed (which provides Linux-shell-like functionality, and is highly recommended) you will have to set all of these variables separately:
```
set out=test.jpg 
set format=jpg 
flam3-render < test.flame
```
Note that these variables will stay defined for future calls to flam3 executables, so make sure you either clear the values or ensure that they are set properly before running another tool.