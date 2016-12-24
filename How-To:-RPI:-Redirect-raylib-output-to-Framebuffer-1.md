**Based on answer by `Morphology` user on [raylib forum](http://forum.raylib.com/index.php?p=/discussion/87/can-i-direct-the-output-to-framebuffer-1#latest):**

There is no particular magic to what I am doing. The first stage is to get the TFT screen working with the Raspberry Pi. In my case I am using one based on the `ili9340 TFT controller`, with an `ads7846 touchscreen controller`. These use SPI Bus 0 and 1, so SPI has to be enabled.

`Nortro's TFT drivers` are now built into the latest Raspberry Pi core, so to get this screen working on the Pi, you just need to edit `/etc/rc.local` and add the following just above the existing script that prints the IP address:
```
modprobe fbtft_device custom name=fb_ili9340 buswidth=8 speed=16000000 bgr=1 gpios=reset:27,dc:22 rotate=270
```
NOTE: bgr=1 is necessary on my TFT as red & blue were reversed. rotate=270 because of the orientation of the screen. If not needed, leave them off.

Edit `/boot/config.txt` and do the following:

Take the comment off the line `dtparam=spi=on`

Add the following in the sections entitled #additional overlays and parameters are....
```
dtoverlay=ads7846,penirq=17,speed=16000000,xohms=100,swapxy=1
```
again, swapxy was because of my screen orientation.

This gets my TFT working, and I can test it by typing the following at the command line, which directs the console output to fb1:
```
con2fbmap 1 1
```
and back again with 
```
con2fbmap 1 0
```
Then, to mirror fb0 to fb1 continuously I use [raspi2fb](https://github.com/AndrewFromMelbourne/raspi2fb) available from github, along with build and install instructions.

Finally, I added the following lines to `/boot/config.txt` so that the aspect ratio of the HDMI screen exactly matches that of the TFT (320 x 240) - otherwise you'll get dead bands down the side of the TFT. This sets the HDMI output to 1280x960 which is the same aspect ratio as 320x240:
```
hdmi_group=2
hdmi_mode=32
hdmi_cvt 1280 960 60 1 0 0 0
```
This is when developing, so that with X running I still get a high resolution display for writing code etc., however, if all I am doing is running the Pi with JUST the TFT attached, then I use the following settings in /boot/config.txt which sets the default HDMI resolution to 320 x 240 BUT NOTE: my main HDMI monitor cannot display that resolution, so if I need to change it back, I have to edit /boot/confix.txt using the TFT display, which can be quite a challenge!
```
#hdmi_group=2
#hdmi_mode=32
hdmi_cvt 320 240 60 1 0 0 0
```
Once that is all done, and you've re-booted, start raspi2fb as a daemon by typing the following:
``` 
raspi2fb --daemon
```
And the HDMI should be mirrored to the TFT. The default framerate for raspi2fb is 10fps, so there is a bit of a lag between what happens on fb0 (HDMI output) and the mirrored version on fb1, but it is fine for my purposes. The important point is that I can then run RayLib as usual, and whatever I output to fb0 is automatically mirrored to fb1.

As I mentioned above, this actually gives me better performance than my raw graphics routines that wrote directly to fb1. I cannot really explain this, though I suspect that the Linux Copy On Write may have something to do with it??
