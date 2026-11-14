---
layout: post
title: "DIY Drone Update #2 - Actually Building It!"
date: 2022-11-23 07:03:00 +0000
tags:
  - "DIY Drone"
  - "Harrison Asmar"
  - "Projects"
redirect_from:
  - "/2022/11/diy-drone-update-2.html"
---

This project has been a long time in the making. My [first post about this drone](https://blog.crankshafttech.com/2019/11/diy-drone-update-1.html) was all the way back in November 2019, where I sourced all the parts but didn't actually build it. But at the end of 2021 (I know, also a long time ago), I actually built and successfully flew it!

Stepping back to 2021, I still had not replaced the broken motor, so I ordered one of eBay, which this time came with all the accessories. I guess you get what you pay for. I also ordered a proper 12V power supply for the battery charger, not because the DIY version was dangerous or failed, but because it used more space than necessary, and was probably not that efficient. Maybe I'll build one of those custom desktop power supplies out of it one day.

![](https://blogger.googleusercontent.com/img/a/AVvXsEhLvxBTcI7YEqGk5ztiLkQncO7ElQLZ_yP1OW8ARG3Wl3z-OVE_pzAcZAFhLurke0LM4YQ8fJYHbEhCcgq4KCT84RowpBKUamWVQUsCwlGVAMtEjqyRQMLZSIoAu-2AC4jsXvcVs8VBXpklRj6om0AgW7ZY7CGIrlm2duqNHg58kk0mBLQ5wxhP9Nyr=w300-h400)

_The First-Person View (FPV) module connected to the Flight Controller_

I started by connecting up the FPV module to the Flight Controller,
since it was the easiest component to start with and allowed me to see
if the camera and On-Screen Display (OSD) on the Flight Controller
worked correctly. Thankfully, when I powered the board via the USB port,
 I was successfully able to connect to the video feed from my headset
(which after purchasing, I have now realised is

[illegal to use](https://www.casa.gov.au/drones/drone-rules/flying-first-person-view)

in Australia without authorisation that I don't have).

At this point I met up with some friends who had slightly more experience with RC aircraft than me, and we got going on connecting the transceiver to ensure that reliable control could be made when the motors are connected. To do this, the PPM pads were shorted to specify which protocol was to be used. After configuring the FC within Betaflight Configurator, the switches, dials and sticks all could be seen to be working. Mainly, I was able to configure the throttle to be the correct stick and in the correct direction, and that a safety switch would be able to instantly disable the motors when activated.

Now that all the simple (and in the case of the transceiver, essential) components were connected, I began to connect all the motors. Firstly, the motors were connected to each of the 4 Electronic Speed Controllers (ESCs), which control the amount of current going to the motors, and allow for the drone to know what speed the motors are rotating at. These ESCs also have extra connectivity, allowing for their [BLHeli](https://github.com/bitdump/BLHeli) firmware to be updated, but also configured beyond that of pure Betaflight.

![](https://blogger.googleusercontent.com/img/a/AVvXsEiIvp3h1nRppI-FMuz4l0kAt9bn12_D9yGYWulWPtZHRny4YcC80tb68t7elKPTnPbizBd8MoC9vy8W4Ul_HDqHcX-oIuSn30HMcgePd5DPSlGaFF3ONPP5s0iaP6Gb5Fa3MfA2ytRMYKVV12icXNeIoyMImvlLaGYpniGYwn_0Co8MExlYOcayMWh1=w300-h400)

_The (amazing) workspace much of the drone was built in_

The flight controller I used only had one pad for soldering all the live and ground wires from the motors to, so I purchased a distribution board which had pads for each of the 4 ESC's power cables, as well as the battery and the power for the Flight Controller. The rest of the wires were soldered directly to the FC, which facilitate the control of each of the individual motors.

While soldering it together, I had to make sure the wires were long or short enough to get to where they want to go, but also make sure that nothing would interfere with the props on each of the motors. I prevented wires from getting snagged using electrical tape, which I also used underneath live components such as the ESC's and Flight Controller to ensure that nothing shorted on the carbon fibre frame.

Once it was all within the frame, I did a quick test of each motor through Betaflight (**with the props removed**), to check that all the motors worked, were in the correct layout, and that they were all spinning the correct direction. I quickly discovered one of the motors spinning the wrong direction, which was easily fixed by unsoldering and swapping the wires around.

Eventually, once everything seemed like it was working correctly, including the safety switch on the controller, I attached the props and took it outside to test if it would fly. And it did! Although I am not very good at flying it, especially landing... It's still in one piece, but I have a lot of practice to do in the simulator before flying it properly.

![](https://blogger.googleusercontent.com/img/a/AVvXsEjIfdP-g_ngIowsljaNRKJ-DWid9248cgAEoOXQTRVngLuRkv5JYr3VG1BePoMJb97oXjZu33tv_n3kq_ZST5WY3p2TLvySEQy4aWAjfSoi1sKxMBVjqFQSBxYcVDeWvJS0ZVoh2fpmRctsM8ZjRtEuxTxyhkbkPtuc5N5_uLg334lwBQhdlH3hdV4-=w400-h300)

_The completed drone (minus battery)_

So now it is complete and works, I hope to fly it soon once I've spent some more time in the simulator. I also have an RC plane now, which I also cannot fly well, so there may be more aerospace related content in the future if either of those get off the ground (pun intended).

more

As always, if you liked this project update, you can find more [here](https://blog.crankshafttech.com/search/label/Projects), and also through the tags below or in the sidebar.

Also, please consider donating via [GitHub Sponsors](https://github.com/sponsors/hasmar04) or [PayPal](https://paypal.me/harryasmar) so I can continue making content into the future.
Finally, to get notified when new posts drop, sign up for email notifications in
the sidebar, as well as follow @crankshafttech on [Facebook](https://www.facebook.com/crankshafttech/) and [Instagram](https://www.instagram.com/crankshafttech/).

If
 you have any feedback or thoughts, please leave it in the comments below.

Thanks again for reading!
