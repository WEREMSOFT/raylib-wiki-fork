Based on this answer: http://forum.raylib.com/index.php?p=/discussion/comment/207/#Comment_207

> Ray, I have been playing with Touch Input of a TFT connected to a Raspberry Pi. 

> I am working outside of X, using RayLib directly on the FrameBuffer

> The main problem is that if you process Touch events as a Mouse, then movements are relative when you need absolute, and if you process the raw events, then you need to handle the scaling, de-jitter etc.

> The best approach I have found so far is to use TSLIB to calibrate the TFT Screen, and then to run the stand-alone utility ts_uinput as a daemon. This generates two additional input devices beneath /dev/input/ - a mouse device and event stream (/mouse2 and /event4 in my particular system).

> The /dev/input/mouse2 device is no use, because the movements are relative, but /dev/input/event4 is good because the events are passed through TSLIB and so are scaled and de-jittered.

> It is a very simple event stream that just contains absolute x & y values plus pressure.

> I modified core.c in RayLib and added a touchThread similar to the existing mouseThread, and used that to read from/dev/input/event4 stream and convert the x and y values from the event stream into the existing absolute mousePosition.x and mousePosition.y values, and used pressure > 0 to simulate clicking the left mouse button by setting currentMouseState[0] = 1 or 0.

> This works fairly well, and means I don't need to worry about scaling or de-jittering the event stream as it is passed through TSLIB first, it also means my application doesn't need to be linked to the TSLIB library, and neither does RayLib.

> If I linked my application directly to TSLIB then I would need to process the touch signals outside of RayLib, whereas it is much easier to use existing RayLib calls for collision detection etc, and it doesn't seem to make sense to link RayLib to TSLIB for the rare occasion that somebody wants to use a TFT screen.

> I would submit a pull request via Github, but I am a little concerned that what I have developed is not a general-purpose solution: It involves the user in some work (installing TSLIB, calibrating the TFT screen, running ts_uinput as a daemon), and I also cannot guarantee what the new event stream under /dev/input/ will be called.

> I would be interested to know whether you think I should submit a pull request so at least you can see what I have done?

> Richard (Morphology)