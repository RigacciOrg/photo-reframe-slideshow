# photo-reframe Slideshow

**Slideshow with on-the-fly, user defined image cropping and resizing**

This is a stand-alone program, which requires the Python 
language (tested with version 3.5 and 2.7) with the PyQt5 and 
PIL libraries.

It requires also the external command line tool **exiv2**, to 
eventually fix a wrong Exif Orientation tag in images.

Usage:

```
usage: photo-reframe [-h] [-f] [-r WxH] [-p] [--timer TIMER]
                     [--no-usercomment] [--shuffle] [--loop LOOP]
                     [--count COUNT] [--read-only]
                     playlist

Show images listed in playlist, which contains "filename|geometry" entries.

positional arguments:
  playlist             playlist filename, or directory which contains it

optional arguments:
  -h, --help           show this help message and exit
  -f, --fullscreen     start in fullscreen mode
  -r WxH, --ratio WxH  screen ratio, width x height (default 16x9)
  -p, --play           start slideshow
  --timer TIMER        time for each slide in seconds (default 6.0)
  --no-usercomment     default to Exif UserComment disabled
  --shuffle            show images in random order
  --loop LOOP          for --play mode only: exit after LOOP slideshow repeat
                       (default 0: endless loop)
  --count COUNT        for --play mode only: show only COUNT images (default
                       0: all the images)
  --read-only          disable playlist saving/overwriting
```

## Slideshow Mode

You can start the slideshow mode in full-screen with the 
following command line:

```
photo-reframe --fullscreen --play path/to/directory/
```

The program receives the name of a **directory** (or a 
**playlist**) from the command line and it will search images 
starting from that location.

The default, preferred name for the playlist is 
"playlist_16x9.m3u" (or whatever is the screen width/height 
ratio imposed by the command line option). A fallback default is 
"playlist.m3u".

The playlist contains the filename of the images and their 
respective geometries, separated by a vertical bar, something 
like this:

```
#EXTM3U-PHOTO-REFRAME
IMG_6602.JPG|4000x2250+0+332
IMG_6605.JPG|2971x1671+796+628
IMG_6606.JPG|4000x2250+0+442
IMG_6610.JPG|3810x2143+90+387
IMG_6615.JPG|2828x1590+547+681
IMG_6617.JPG|1633x918+1229+1052
IMG_6624.JPG|2843x1599+393+585
```

Each geometry determines the portion of the image to be shown in 
the slideshow, and it is composed by four values:

* **width** of the crop region
* **height** of the crop region
* **X offset** of the top-left corner of the region
* **Y offset** of the top-left corner of the region

The cropped region is resized to occupy the entire screen. In 
slideshow mode, only images listed in the playlist will be 
automatically shown for the time specified by the **--timer** 
command line option (default is 6 seconds).

You can arrange the image order etc. editing the playlist file. 
Other images contained into the directory will not be shown.

## Playlist Editing Mode

If you stop the slideshow play mode (press the **P** key), you 
can interactively prepare the playlist. It is advised also to 
exit the full-screen mode pressing the **F** key.

Every image is displayed on the screen, you can adjust the zoom 
and pan as desired and then press the **Return** key. The border 
of the image becomes **green**, indicating that a geometry was 
selected for that image. A **red** border means that no geometry 
was defined for the current image (thus that image will not be 
displayed in slideshow). A **yellow** border means that a 
geometry was defined, but you moved the image into the frame, so 
you need to press Return again to update its value.

At the end of your work, press the **S** key to save the 
playlist with the update geometries.

This is the help for every **keyboard shortcuts**:

```
F1 or ?   Show this help
F2        Share with...
F11 or F  Toggle fullscreen mode
+         Zoom in
-         Zoom out
1         Zoom to 1:1 pixel
W         Zoom to fit image width
H         Zoom to fit image height
Z         Zoom-in to remove blank space
R         Update image by cycling Exif Orientation tag
Return    Accept current geometry for image
G         Zoom to current image geometry
Canc      Forget current geometry (remove from playlist)
I         Print image and window data
Space     Go to next image
Backspace Go to previous image
<         Shift the image backward into the playlist
>         Shift the image forward into the playlist
C         Toggle display of Exif UserComment
P         Toggle autoplay
S         Save the playlist with updated gemetries
Q         Quit program"""
```

When the program run in window mode, it will show the image into 
a rectangle with a **fixed width/height ratio** (default is 
16:9), this forces the preparation of geometries suitable for 
this screen ratio. If you intend to play the slideshow on a 
different screen, you can start the program adding a command 
line option e.g. **--ratio 4x3**.

If you run the program with the **--shuffle** option, images are 
displayed in random order. You cannot change image playlist 
position (shifting backward or forward) when you are in shuffle 
mode.

The **--loop** and **--count** options make sense only with the 
**--play** option. The first one control how many loop of the 
playlist are required (default to endless loop), the second one 
tells the program to play only some images from the playlist 
(default to all).

The **--read-only** option disables the saving (i.e. 
overwriting) of the playlist file. It may be useful when the 
program is used for playback only, to avoid accidental changes 
to the playlist. There is a _cheat mode_ to force saving even in 
read only mode, read the source code if you are interested.

The **Share With...** function, invoked with **F2** key, prompts 
you for a recipient address; then it executes the external 
program defined by the **SHARE_COMMAND** variable, passing to it 
the image filename and the recipient address as arguments. You 
can create for example an external script to send the displayed 
image as an email attachment.
