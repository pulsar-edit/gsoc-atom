# International keyboard issues in Atom

<p align="justify">
&nbsp; Being able to type in and use an application correctly in the keyboard layout of your native language is a simple but obvious need. If this kind of issues can be worked around/overcomed in a browser, when it comes to <strike>a text editor</strike> <a href=https://atom.io> the text editor</a> it's priority dramatically increases. As we will see further on, the mentioned issues are not caused, but rather inherited.
</p>

## Keyboard event in Atom
<p align="justify">
So, what happens when we press a key in Atom? At a simplified level, the (physical) keyboard generates an event. The event gets to the operating system that further assigns it to your focused application. A focused application is your main tab, "in which you currently are". We use <a href=http://www.chromium.org/blink> Blink </a>, the layout engine. This layout engine "feeds" Atom an object called <b> KeyboardEvent </b>. KeyboardEvent has fields(.key, .keydown, .keyup) that contain information about what key was pressed, wether it's a modifier or not etc.
</p>

## What goes wrong

<p align="justify">
There's a saying about what hapens when you chase two rabbits, so we decided to single out a keyboard and focus on that to get a firm grip of problem's nature.Therefore, I focused solely on <b> German </b> keyboard. The following three issues were identifiable and reproductible:
</p>

- alt gr + q should be "@" but no character is printed.
- \# is correctly written in the editor window, but read as \
- ctrl-shift-7 is read as ctrl-? instead of ctrl-/

<p align="justify"> From left to right, we have : alt pressed on a US keyboard layout, alt gr pressed on a German keyboard and (alt gr + q) on a German keyboard</p>
<img src=http://i.imgur.com/jentXet.png>



<p align="justify"> Based on the images above we can notice a few issues </p>

- alt gr is not recognised and has a wrong keycode (225 is the keycode for sharp s - ß)
- the .key member for alt gr is empty.

<p align="justify"> Similarly, the # gets printed correctly but is recognised as a \ . </p>

## Why it goes wrong

### Wrong keycode/charcode on some events (#, alt gr)

<p align="justify"> Still under investigation, to be updated on further notice. </p>

### Why .key is empty

<p align="justify"> Looking <a href=https://chromium.googlesource.com/chromium/blink/+/master/Source/core/events/KeyboardEvent.cpp>here</a> we can see that alt gr .key is not yet implemented/supported in Chrome/Blink, and that's the cause for which the field is empty</p>

### Current status

``` this will be continuously updated ```
<p align="justify">
Investigating chrome source, aiming to decide if we have enough correct information to properly determine all keys or if we should look for alternatives.
</p>


## Background details & relevant information
<p align="justify">
Tests were performed on the following setup: <br><br>
Operating System: Ubuntu 14.04<br>
Platform: x64<br>
Chrome version: Version 45.0.2441.0 (64-bit) built from source <br>
Atom version: 0.208.0-cecd233 built from source. <br> <br>
</p>
<p align="justify"><b> Note:</b>  All aforementioned key combinations (on their specific layouts) were tested in the URL bar(of Chrome) as well as in a terminal. The keyboard events issue is not caused by the operating system. Further proof is that the issues manifest differently on different operating systems. </p>
