This is a simple infrastructure to compile Arduino sketches on the
Intel Galileo. Compiling sketches on the Intel Galileo is particular
useful for clanton-full Linux installations based on eglibc (see e.g.
http://www.malinov.com/Home/sergey-s-blog) where one installs a rich
set of linux command on an SD card and boots the Galileo from this
card.

Background: By default, the Arduino IDE builds sketches for ulibc,
which are cross-compiled on a PC and downloaded via USB to the
board. ulibc is very compact (necessary for the default tiny SPI-flash
Linux), but it seems limited and is hard to debug (i run into problems
with dynamic loading, ldd seems not available, etc.). Therefore,
clanton-full distributions use usually eglibc. The downside of using
eglibc is that sketches compiled with Arduino IDE and ulibc can't be
executed in such an environment.

By compiling sketches directly on the Galileo under Linux, one can
compile easily sketches with eglibc in a way natural for Linux
developers while preserving the ease of sketches to interface with
sensors and hardware gadgets.

One needs actually three steps to compile and execute sketches
on the Intel Galileo:

    1) Copy sources of libraries and Template for the sketch to the SD card
    2) On Galileo, copy Template for the sketch and insert sketch
    3) Compile via make and execute the sketch

PS: I've developed this package to ease teaching IoT appliances based 
on the Intel Galileo board.


# 1) Copy Sources of Libraries and Template for the Sketch to the SD Card #

    Copy libraries from the Arduino IDE for the Intel Galileo to the SD
    card, which is used for booting the Galileo. The libraries are quite
    small (total of less the 1.5 MB) without arduino-libs (which are optional)

    Copy always the standard libraries (hardware/arduino); in case you
    have user-installed libraries, copy these as well. Optionally,
    the shipped Arduino examples for the Galileo can be copied (called
    here arduino-libs).

    Let us assume, the SD card has the label GALILEO, one can use the
    following command under Mac OS X:

       cp -r /Applications/Arduino.app/Contents/Resources/Java/hardware/arduino /Volumes/GALILEO/arduino
       cp -r ~/Documents/Arduino /Volumes/GALILEO/arduino-local
       cp -r /Applications/Arduino.app/Contents/Resources/Java/libraries /Volumes/GALILEO/arduino-libs

    Copy sketch template as well:

       cp sketch-template /Volumes/GALILEO/sketch-template

    For other operating systems you should find these libraries on the
    listed places:

    Linux:
     - standard libraries:  Arduino-1.5.3/hardware/arduino
     - user libraries:      ~/Sketchbook

    Windows:
     - standard libraries:  Arduino-1.5.3/hardware/arduino
     - user libraries:      Documents/Arduino


# 2) On Galileo, copy Template for the Sketch and insert Sketch Source #

    Start the Galileo from the prepared SD card and log in (e.g. via
    ssh). Assume, you want to compile the sketch "Blink.ino". First copy
    the sketch-template to a new folder used for your project (e.g.
    blink-sketch).

       cp -r /media/realroot/sketch-template ./blink-sketch
       cd ./blink-sketch

    Edit in Makefile and set the variable SKETCH to the name of your sketch.
    E.g.

       SKETCH=Blink

    Copy the Blink sketch "Blink.ino" to this directory. You will find
    the .ino file in the Arduino tree. Alternatively, simply edit a new
    file named "Blink.ino" and paste the source from the Arduino IDE into
    the editor.

    For larger projects, you might want to add user libraries. See
    Makefile for an example.


# 3) Compile and Run the Sketch #

    To compile the sketch, run

       make

    The Makefile will produce an executable named "sketch". One can run
    it without arguments,

       ./sketch

    Optionally one can provide ttys for "Serial", "Serial1" and
    "Serial2" if these are connected:

       ./sketch tty0 tty1

    For debugging during development it is the easiest to use
    "fprintf(stderr, ....)" in your sketches since debugging
    goes directly to the Linux console.

