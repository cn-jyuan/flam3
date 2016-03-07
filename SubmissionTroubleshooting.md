## I can't upload ##
You have hit the posts-per-day limit. Wait until tomorrow.

## The sheep died before it could be born ##
Usually indicates a formatting error in your uploaded flame.
  * The option 'Save gradient in old file format' MUST be checked on the 'General' tab of Apophysis' Options. If it is not, the submission will fail. (Note - this is fixed in the next version of the software.)
  * The 'name' of the flame in the file may contain spaces or start with a number. This is not valid XML, and will be rejected by the server.
  * If you saved the genome with File->Save Parameters..., or Ctrl-S, then Apophysis saves the flame with an additional set of enclosing tags around it, called `<Flames>`. Remove the `<Flames name="...">` line (the first line of the file) and the `</Flames>` line (the last line) before submission. Do not remove the `<flame ...>` and `</flame>` lines.
  * You uploaded something other than the flame file -- perhaps a rendered jpg.

## The sheep looks different from how it was designed ##
  * Check the number of variations per xform. The server limits it to 5.
  * The twintrian variation is banned from the server because its definition has changed over time.
## The sheep looks fine but doesn't move ##
> For Apopysis users, try looking at the "Colors" tab in the Transform Editor window (where you move triangles). Check each triangle's Symmetry. Try setting the Symmetry to zero for one or more triangles. This WILL change the colors of your sheep but it might also successfully animate your "lazy" sheep.