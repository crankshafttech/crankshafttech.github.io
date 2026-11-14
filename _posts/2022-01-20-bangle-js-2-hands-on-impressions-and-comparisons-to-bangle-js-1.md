---
layout: post
title: "Bangle.js 2 - Hands-on, Impressions and Comparisons to Bangle.js 1"
date: 2022-01-20 02:59:00 +0000
tags:
  - "Bangle.js"
  - "Reviews"
  - "Hands-on"
  - "Harrison Asmar"
  - "JavaScript"
redirect_from:
  - "/2022/01/banglejs2-hands-on-impressions-comparisons-banglsjs1.html"
---

The Bangle.js series of watches instantly caught my attention back in 2019 for their customisability and freedom to install and create whatever I wanted, while still supporting common smart-watch features like notifications and health tracking.

While I had a few small things I disliked about the first iteration, I was overall very pleased and excited for the future of the ecosystem. So when the second version launched in the second half of 2021, I was extremely excited to see what had been changed and improved.

Now that I have owned and worn both the Bangle.js 1 and 2 for a reasonable amount of time, I'm sharing my thoughts on what they do well, as well as what room there is for improvement.

![](/assets/images/20211231_134657.heic)

_Bangle.js 1 (Left) and Bangle.js 2 (Right)_

## Software

The awesome part of both of these watches is the underlying firmware, [Espruino](https://github.com/espruino/Espruino). This firmware works on many different microcontrollers, and allows for the device to be programmed using JavaScript, a language traditionally reserved for websites. This allows for a lower initial learning curve, as JavaScript is a relatively easy language to learn compared to the likes of C.

Also, it allows for easy coding and testing via a web browser, as all the code can run natively, with a few obvious emulations of certain hardware sensors and components.

With both watches using the same firmware, it means that most apps are cross-compatible, although some that rely on certain hardware features, such as the 3 buttons on the Bangle.js 1, may not work on both. An example of this is [BangleRun](https://github.com/espruino/BangleApps/tree/master/apps/banglerun), which only works with the Bangle.js 1.
This means that over 2 years after the original kickstarter launch, the ecosystem is filled with different watchfaces, apps and widgets to play around with.

Another cool aspect of the Bangle.js ecosystem is the [Widgets](https://www.espruino.com/Bangle.js+Widgets). These are little icons that can easily be added to any app to show things such as time, battery, bluetooth, steps and more. These widgets can even be companions to full apps, such as the notification widget that tells the user when there are notifications in the Messages app.

One other aspect of the Bangle.js software is that every component can be modified or outright replaced, including the firmware. This means that if there is an aspect of the overall system you don't like, such as the menus, settings or behaviour, all the code is open and freely available for you to modify.

In addition to this, all aspects of the code accept community input through their respective Github repositories, so whatever improvements or changes you make can potentially help improve the experience for other users around the world.

To see the list of available apps for these watches, visit the [App Loader](https://banglejs.com/apps/), and to start developing code yourself, checkout the [numerous tutorials on the Espruino website](https://www.espruino.com/Bangle.js2#tutorials).

## Hardware

The physical aspects of the Bangle.js 1 and 2 are really what set them apart. The most notable difference is the size and shape of the watches. While the Bangle.js 1 was round and quite large, the Bangle.js 2 is significantly smaller and also rocking a "squircle" face. I much prefer this new design, as it fits on my small wrist much better and isn't as heavy meaning I can actually forget the watch is there. 

![](/assets/images/20211231_134739.heic)

_Side Profile of Bangle.js 1 (Left) vs Bangle.js 2 (Right)_

Another notable change, which I have already mentioned above, is the removal of the 3-button navigation in exchange for a touchscreen and a single side button design, comparable to the Amazfit Bip line of watches.

![](/assets/images/20211231_134832.heic)

_The Side Buttons on the Bangle.js 1 (Top) vs Bangle.js 2 (Bottom)_

This change is a bit more controversial as a touchscreen is much less accurate and reliable than physical buttons, while on the other hand the touchscreen allows for more interaction styles not possible with buttons, such as interacting with a specific icon on the screen without "scrolling" through them to select the correct one.

Sadly, one thing that did not change was the water-resistance rating. Both watches are rated for IP68, which means they can get wet and submerged, but swimming is not recommended. As a swimmer myself, this is not ideal, but knowing that it at least has a chance of surviving if I forget to take it off, I can live with it.
Another change with the screen is that the Bangle.js 1 had a beautiful, bright and clear OLED display, while the Bangle.js 2 has an always-on LCD that is a lower resolution. Personally I prefer this display as it allows for much longer battery life as well as an always on display, which are not both possible at the same time with an OLED. Speaking of battery life, while the Bangle.js 2 is advertised as having up to 30 days, in my experience it's closer to 1 week.
Although my experience may differ from the advertised time, this is due to how many apps I have installed as well as the amount of notifications I receive and how I have 24/7 heart rate enabled along with steps, all of which can significantly affect battery life. I have no doubt it is possible to reach the advertised time, it would just come with sacrifices.
More information on battery life and power draw can be found [here](https://www.espruino.com/Bangle.js2#power-consumption).
Other changes to the hardware include an improved heart rate sensor, a better charger with less chance of a short-circuit, a barometer under the strap mount and standard 20mm interchangeable bands. To see all of the specifications, check out the product page [here](https://www.espruino.com/Bangle.js2#features). 

## Conclusion

Overall I think the Bangle.js 2 is a major upgrade over the Bangle.js 1 even with a few of the "downgrades", especially in regards to the screen, but with these being in exchange for a lighter, smaller design it's not really a downgrade, more just a design shift. For people with large wrists that want the brighter and higher resolution OLED as well as physical navigation buttons, the Bangle.js 1 is for them, while the Bangle.js 2 is for people who grieve their Pebble of yesteryear and don't want to sacrifice battery life and weight. For me personally, this will be staying on my wrist for the foreseeable future.

If you are interested in getting one for yourself, sadly only the Bangle.js 1 is available to buy at the time of writing, with the Bangle.js only sent out to Kickstarter backers. To buy the original and eventually the Bangle.js 2, checkout the [Espruino Shop](https://shop.espruino.com) (not affiliated). 

As always, if you liked this review, you can find more [here](/tags/reviews/), and also through the tags below or in the sidebar. If you have any feedback or things that I should include or remove in future reviews and showcases, please leave it in the comments below, and thanks again for reading!
