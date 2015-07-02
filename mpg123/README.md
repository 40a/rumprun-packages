Overview
========

Following the instructions will build and run mpg123 for the rumprun
_hw_ platform.  You could theoretically do the same for Xen platform
via PCI passthrough, but doing so is left as an exercise for the reader.


Download
========

Fetch mpg123 sources from http://mpg123.org/
(or location of your choosing)


Patches
=======

No patches are necessary.


Instructions
============

Untar and run configure as:

```
rumprun-bmk-configure ./configure --enable-modules=no --enable-static=yes --enable-shared=no --enable-buffer=no --with-cpu=i486 --with-default-audio=sun
```

You can be more adventurous with the CPU if you like, we are just
selecting a safe default for everyone.

Run `make`.

Run `rumpbake` on `src/mpg123`.  You will most likely want to use the
`hw_generic` target, i.e. `rumpbake -T hw_generic -o mpg123.bin src/mpg123`.

Use `rumprun` as normal.  You need to specify `-a /dev/audio0` as the
audio device argument to mpg123 for now.

Examples:

```
rumprun kvm -i -b ~/tosi.iso,/mp3 -g '-soundhw es1370' mpg123.bin -a /dev/audio0 -v -z -@ /mp3/allmp3.txt
```
(assumes `tosi.iso` contains mp3's along with a list of them)

```
rumprun kvm -i -I iftag,vioif,'-net user' -W iftag,inet,dhcp -g '-soundhw es1370' mpg123.bin -a /dev/audio0 -v 'http://10.0.0.10/test.mp3'
```
(tested working, but network configuration is left as an exercise to the reader.
if you want DNS, remember to provide a stub `/etc`)
