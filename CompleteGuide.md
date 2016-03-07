A guide for designing and posting sheep to the server:

## Getting Started: ##
  * **Step 1:** The best first step is to join the [Genetic Design mailing list](http://lists.electricsheep.org/cgi-bin/mailman/listinfo/genetic-design). You will then get all emails that are sent to the List by people around the world. If you read the email from this list for a while you will see important information, such as the URL and password for posting new sheep, cross the list, as well as a lot of information about things to do with sheep.
  * **Step 2: Get a Flame editing program.** This is the program you will probably use to do most of your viewing and rendering. On Windows, we recommend [Fr0st](http://fr0st.wordpress.com/) but you can also use Apophysis. First download and install the [last stable release](http://www.apophysis.org/downloads.html)  and then download and install the [Sheep Edition](http://electricsheep.org/Apophysis207SE.exe). If you're on a Mac you can use [Oxidizer](http://oxidizer.sourceforge.net/Site/Oxidizer.html), for Linux you can use [Qosmic](http://code.google.com/p/qosmic/).
  * **Step 3: Make some fractals.** When you start up Apophysis it will give you a bunch of random fractal flames that you can play with, but you'll probably have more luck and more fun if you start with some existing sheep and try to modify them first. There are several places to get sheep genes:
    * From the [sheep server](http://v2d7c.sheepserver.net/cgi/status.cgi). Click on any image that you like. Click on the genome link. Right-click on the file link and choose "save as." Pick a name that ends with ".flame" or ".flam3". If you prefer you can click on the file link instead, then select all of the text and save it as a plain text file with the extension ".flame" or ".flam3" . Be sure to use an editor that does not add line breaks, like [The Programmer's File Editor](http://www.lancs.ac.uk/staff/steveb/cpaap/pfe/) or Unix's pico. You can then open this file in Apophysis.
    * From the [electric sheep archives](http://www.electricsheep.org/archive/) - the repository of deceased sheep.
    * From the parameters of a frame that is in the process of rendering. See the "Your Sheep's Page" section below for details.
  * If you have trouble opening the file in Apophysis, it is likely that one of two things has happened:
    * You accidentally clipped off some part of the file. Make sure the file begins with `<flame` and ends with `</flame>`.
    * You are using the release (non-beta) version of Apophysis and your parameter file has some `<edit>` tags at the bottom. You can just cut these out in your text editor, but it is probably better to download and install the new Apophysis beta.

## Modifying Flames ##
This process will be different depending on if you are using Apophysis, Oxidizer, Qosmic, Fr0st, or Flam3.

When you have a still image that you are happy with, you are ready for Step 4:

## Uploading Sheep ##
  * So, now you have the parameters for your shiny new little lamb saved into a file. If you haven't gotten the information for posting your sheep yet from reading the Genetic Design email list, now is the time to post to the list asking for this information. Follow the instructions they give you.
  * If your sheep has posted successfully, you should see a (low-res) image of your sheep and see that it has been assigned a number. Bookmark this page or keep the link open - this is where you will find information on your sheep's life on the server. If something was wrong with the parameter file you will not see its picture and it will list the sheep as dead if you check its stats page (see below for more information about things you can do with your sheep's page). You may watch its progress in the [this queue](http://v2d7c.sheepserver.net/cgi/status.cgi?&detail=queue). There is a limit of 1 upload per 24 hours.
  * The sheep server does the rest! The frames of the sheep animation will be rendered by users around the world. You can watch its progress under the Frames link. When it is done you can download it.
  * You can watch your sheep's popularity on the stats link. Be forewarned - life on the sheep server can be quite harsh.
  * Old sheep are deleted from the folder on your hard drive to make room for new ones! If you want to preserve particular sheep, you need to save a copy in a different folder.

## Your sheep's page ##
  * Stats: This is the main page you'll be watching. The sheep's state should normally progress from post\_queue to busy to done to dead. This is also where you see your sheep's rating (see Voting, below). You can also see links to its Edges here, but that's easier to keep track of in the Motion link.
  * Motion: Shows a few example frames of your sheep loop as well as pictures of the sheep that it has made edges with. Each sheep will be a part of a minimum of 2 edges as well as its own loop. If an edge is just showing a question mark, it means that the edge is in the process of rendering. Click on the question mark and then click on the frames link for another source of sheep genes!
  * Frames: This link does two different things. After the sheep (or edge) is rendered, it shows the names of the people who generously donated their CPU time to render this sheep. While the sheep is rendering, though, it displays each frame as it is completed - which is a great source of unique parameters. If you see an individual frame that you like, click on it, click on the genes link, and save the text that it gives you (the `<get>` to `</get>` tags) in a text file with the extension .flame . It will have 3 slightly different still images in it (3 frames of the animation). You can open this .flame file in Apophysis. You can only get the parameters for these "middle frames" off the server while the sheep (or edge) is in the process of rendering - once its done, the parameters aren't saved anywhere.
  * Family/Lineage: This usually only gets interesting after the sheep is dead. Check in here from time to time to see if your sheep has produced any offspring. Only the server's sheep have their lineage traced. The server's sheep say they were designed by brood.

## Genetic Engineering ##
  * You may find yourself wanting to do some more advanced genetic engineering, such as crossing two interesting sheep or randomly mutating a sheep. The best way to do that is to use yet another program, flam3. Instructions for doing so are found on the Electric Sheep Wiki but they are probably a little arcane if you're not already comfortable using command-line utilities. Flam3 is also good for rendering sheep at higher resolution than Apophysis will handle, conveniently doing batch renderings, etc. The following commands will cross two sheep, if used in a Cygwin or Linux command line terminal:
```
env cross0=flame1.flame cross1=flame2.flame repeat=20 flam3-genome > newfile.flame
```
Instead of flame1.flame and flame2.flame, put in the names of the .flame files for the two sheep you want to cross. The results of the cross will be saved in the file newfile.flame, or whatever name you specify. This will generate 20 new sheep, you can change the number to get however many you want. To do this in Windows without Cygwin, you should be able to use [this file](http://www.tiedyedfreaks.org/ace/doscross.bat). Open it in a text editor and change the names of the sheep (A.flame is one parent. B.flame is the other parent, AB\_cross.flame is the new file with children).
  * If you wish to render sheep using flam3, you can use the line:
```
env in=newfile.flame prefix="newfile" flam3-render
```
This will generate images named newfile0000.jpg, newfile0001.jpg, etc. You can tell which sheep is which in your newfile.flame by looking at the time= tags.
  * Note the number in front of the quality tag. This determines the resolution of the //rendering//, which is independent of the resolution of the //jpg//. For some reason Apophysis likes to set quality to more or less random numbers. If you are particular about the quality of your renderings, you'll have to go through in a text editor and check the quality setting of every individual sheep before you let flam3 render it. Sheep server default is 500, you can use lower quality settings to get it to render faster.

## Voting and Herd Health ##
  * Life or death on the sheep server is determined by the number of sheep on the server, the age of the sheep, and its //current// rating.
  * Reproduction is also determined by current rating, although dead sheep can reproduce too.
  * Voting has no impact on the sheep on your computer.
  * You can delete sheep from your computer. If you want to see your own sheep more often on your screensaver, you may want to "depopulate" your sheep folder so it contains a higher percentage of your own sheep. Remember that the oldest sheep will still be deleted first as the screensaver downloads new sheep.
  * If you don't want your old sheep to be crowded out of your sheep folder, you may turn off both bittorrent and http from the screensaver options window to get it to stop downloading new sheep. Then, the only way to get new sheep is download them by hand. (Note that you can't yet turn off automatic downloading for the Linux client).
  * Votes only count for live sheep. Most sheep only live about 7 days. Votes for edges don't count.