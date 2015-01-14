---
layout: post
title: Android Overdraw Optimization
---

Overdraw is painting a pixel more than once and it's very easy to overlook when building an Android application.

Overdraw can lead to poor view performance on lower end phones if not properly taken care of.  Overdraw often happens with poorly thought out styles where a background style gets applied and then has subsequent styles drawn on top, such as ListViews, TextViews, etc.  It's probably easier to see what in the world I'm talking about:

* On your Android 4.2 device, open up settings
* Go to 'Developer Options'
* Toggle 'Show GPU Overdraw'
* Now open up an app on your phone.

What we see here is how many times a pixel was painted.  From best to worst we'll see blue, green, light red and red.  If you have a lot of blues and green, with some sparse red, you're probably OK.  However if you have lots of red, you should really consider refactoring your view code.  Most instances can easily be refactored by double checking your styles and removing erroneous colors and drawing.  Some overdraw is easier to eliminate than others.  Luckily, there are actually tools that can help you out here.

For a more in depth look into overdraw, how you can fix it and some tools, check out <a title="Overdraw - case study" href="http://www.curious-creature.org/docs/android-performance-case-study-1.html" target="_blank">this case study</a> from Romain Guy.