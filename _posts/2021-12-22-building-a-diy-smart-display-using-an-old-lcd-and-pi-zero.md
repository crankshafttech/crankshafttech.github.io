---
layout: post
title: "Building a DIY Smart Display using an Old LCD & Pi Zero!"
date: 2021-12-22 04:32:00 +0000
tags:
  - "Raspberry Pi"
  - "DIY"
  - "Harrison Asmar"
  - "Projects"
redirect_from:
  - "/2021/12/diy-smart-display-old-LCD-Pi-Zero.html"
---

It's been a while since I last wrote for this blog, let alone about a project, but over the past few months I have worked on multiple different projects that I hope to share on here over the next few weeks and months.

![](assets/images/20210808_122429.jpg){: width="400" height="300" }
_The finished project displaying the Raspberry Pi boot screen_

I first embarked on this particular project after I had setup [Home Assistant](https://www.home-assistant.io/) on my Home Server and wanted a way to display the information on the dashboard in my workshop.

I was also inspired by the blank whiteboard I had hanging on my wall already and the smart home dashboards built by the members or [r/homelab](https://www.reddit.com/r/homelab/).

![](assets/images/20210807_110641.jpg){: width="400" height="300" }
_The destroyed pin-board and whiteboard_

The first piece I needed was a whiteboard, with the one I used pictured above. As it was already partly destroyed by someone else, it was perfect for this project as I was throwing out the cork half anyway.

The second piece I needed was a display. I sourced this from an old Dell All-In-One that had been stored under my desk for multiple years. I had turned it on in the past and knew that the panel worked.

To get the panel to work in the configuration, I needed to source a controller board to power the display as well as allow for a HDMI connection to it, without the rest of the computer being required.
As the display utilised the common LDVS display connection, which meant that I could order a compatible board for my specific display for US$33 on eBay. This also controls the backlight to allow for brightness control as well as turning the backlight on and off when needed.

Finally, the display requires a way to load and display the Home Assistant dashboard via a browser. When I started this project the Raspberry Pi Zero 2 was not yet available, so the original Pi Zero W was used. This caused a few problems which will be detailed later on.

## Assembling the Display

First, the pin-board needed to be disassembled so that the cork half could be removed so that only the whiteboard remained. Luckily, the frame was only held together with staples and the panels were not attached to the frame, rather just sitting within grooves, so i was able to pull it apart nice and cleanly.

![](assets/images/20210807_111445.jpg){: width="400" height="300" }
_The board completely disassembled_

The next order of business was to modify the frame so that only the whiteboard half was framed.

This was achieved by using a handsaw to cut additional corners into the wide grooved-edges of the frame so the mid bar could be replaced with the stronger side that was from the pin-board, which also makes the frame symmetrical.

This frame was then held together using nails and glue, with the whiteboard section slid inside, which created a tight fit that held the board firmly in place.

After this, the display panel was measured and a hole of the same size was cut out of the whiteboard. This was not perfectly straight and centred, but it was close enough for me to be pleased with the result.

![](assets/images/20210807_132636.jpg){: width="400" height="300" }
_The screen inserted into the whiteboard_

As the whiteboard portion was only made from cardboard with a whiteboard sticker, the cuts were not very neat, but they did the job.

![](assets/images/20210807_135148.jpg){: width="400" height="300" }
_The messy cutting as seen from the back_

To secure the screen to the whiteboard, electrical tape was used to join the screen bezel to the whiteboard as well as cover any imperfections in the cuts.

![](assets/images/20210807_135135.jpg){: width="400" height="300" }
_The tape applied to the front of the screen_

This tape was applied to both the front and back, excluding where the display control board was, to make sure the display was secured in both directions.

Now that the screen was attached to the whiteboard, the frame was reinstalled with it's modifications using nails and glue.

![](assets/images/20210808_103218.jpg){: width="400" height="300" }
_The whiteboard with the frame attached_

Now that the frame was attached, I needed a way to attach the electronics like the controller board without a risk of them shorting against the metal backing of the display. To solve this, a piece of wood was cut to fit inside the back of the frame so that it was flush, and it was then attached to the screen and whiteboard using double-sided tape. This also provides more contact between the screen and the frame, reducing the risk of it falling out.

![](assets/images/20210808_111539.jpg){: width="400" height="300" }
_The backing wood with all the required holes and tape_

Holes were required for the controller board as well as the backlight port, so that nothing was broken and everything could be accessed easily.

All that was left to do was install the electronics!

![](assets/images/20210808_113727.jpg){: width="400" height="300" }
_All the electronics laid out on the back_

All the components were attached using double-sided foam tape, as screws would interfere with the screen.
Absent from the above photo is the Raspberry Pi Zero W, which was attached via HDMI to the display controller.

## Setting up the Software

The first task was flashing an SD Card with an up-to date version of Raspberry Pi OS (previously Raspbian). I chose the Lite version as the Pi Zero W is not very powerful and needs as much processing power as possible dedicated to the browser window.

The first boot proved successful, as after the rainbow boot screen, the boot debugging appeared, followed by the initial setup.

![](assets/images/20210808_122431.jpg){: width="400" height="300" }
_The boot debugging from the Pi Zero W_

As this is not a tutorial, I will not be detailing every step to setting up the software. This is also because it will vary from setup to setup.

The main things I configured were:

- Setting the rotation to vertical as that is how I will mount it
- Installing the web browser and it's related packages
- Making a startup script that loads the webpage at each boot in kiosk mode

Sadly due to the poor performance, I have not yet been able to deploy this setup, but have some possible remedies which I discuss below.

## Future Improvements

The main issue currently is the performance of the Pi Zero W due to it's single core processor. At the time of building this, the Pi Zero 2, which is an upgraded Pi Zero W with 4 cores, was not available, but now that it is and I have acquired one, I hope to see if it rectifies my issues in the future so that I can finally deploy it.

Another issue that I have a possible solution for is that the components currently protrude out the back of the frame, meaning that it would be resting against them if it was wall-mounted. The solution I hope to try is attaching the offcuts of wood from the other half of the frame to the back as spacers. This would also hide the components, making the setup look cleaner.

Finally, something I have not found a solution for yet is powering both the Pi Zero 2 and the display with one power cable. As the Pi Zero 2 requires 5V over Micro USB and the Display 12V over a barrel, I currently have to run two power cables, but possibly with a step-down converter to 5V both could be powered off the 5V in the future.

## Final Thoughts

Now that this project is mostly complete, I am overall satisfied with the outcome, and the amount of room for improvement in the future.

As always, if you liked this project, you can find all my other projects [here](/tags/projects/), and also through the tags below or in the sidebar.

If you have any feedback, please leave it in the comments below,  and thanks again for reading!
