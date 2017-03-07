---
Title: Build Chromium-Embeded-Framework-in-Manjaro-Linux
date: 2017-03-02 00:12:21
tags: [c++]
toc: true
---

### Different Approaches to Integrate a WebBrowser/WebView into Your Desktop Applications.  

* Easy 3D Render To Texture Approach  
[Awesomium](http://www.awesomium.com)(base on Webkit) - this project started from Ogre3d showcase forum back in 2009 when I was still in school. Then it said goodbye to Ogre community and went commercial. It still reminds me the happy time I was building [games and editors](http://youchaosdevelopment.blogspot.com/2009/02/world-of-champloo.html) using a C++ game engine(No offense, it feels better to write C++ than drag and drop Unity3D script even that's a much easier way.). Ogre3D - a great game engine that teaches me Design Pattern and many things for software architecture. I would definitely recommend ["Game Engine Architecture"](https://www.amazon.com/Engine-Architecture-Second-Jason-Gregory/dp/1466560010) book to anyone who is interested on understanding Ogre3D and a proper game engine architecture.  
I gave a try today on Awesomium, it still works but the problem I found is that for some HTML5 featured site, it doesn't work and I got a message asking me to upgrade my browser. I guess it's because the project hasn't been updated for 2 years and the underlying webkit support maybe outdated.  

* In visual studio , drag and drop a .net webview UI component on your c# project and you have it. But who wants a webview with IE core?  

* Integrate one of the major browser core into your desktop application. (Firefox Gecko, Chromium Blink, Safari Webkit or IE Trident etc.) Among those using Google Chromium based browser became a very popular solution. You can find successful example such as [Crosswalk](https://crosswalk-project.org/documentation/about/demos.html) which not only brings the chromium browser/webview to desktop but also mobile platform.
Another popular solution came out for a few years is CEF(chromium embeded framework). I have seen many html5 UI style desktop applicaiton which uses this framework such as [HEX](https://github.com/netease-youdao/hex).

### Building CEF on Manjaro Linux  
Since my purpose is to integrate CEF into my application, I choose to use the CEF binaries directly. Make sure to download the latest binaries from [here](http://opensource.spotify.com/cefbuilds/index.html). (I used cef_binary_3.2924.1575.g97389a9, an early version will give you a annoying run time error about libgcrypt11.so not found.)

The building steps are cleared in the CMakelists.txt. On linux, you can choose ninja or Gnu Make to build the examples.  

* Make sure to install CMake 2.8.12.1 or newer version; install ninja build tools.  
  On ArchLinux, run  *sudo pacman -S ninja* and *sudo pacman -S cmake*  
  
* Run the following commands  
  > cd path/to/cef_binary  
  > mkdir build && cd build  
  > cmake -G "Ninja" -DCMAKE_BUILD_TYPE=Debug ..  
  > ninja cefclient cefsimple  

* Then I get the following error:
  ../cefclient/browser/client_types.h:12:21: fatal error gtk/gtk.h: No such file or directory  
![](/images/gtkerror.png)

What is going on? If you read the comments in CmakeLists.txt, ArchLinux based family was not in the supported distribution list and there are three required packages: build-essential, libgtk2.0-dev, libgtkglext1-dev. I can't find those packages in Pacman repos, do I really need to install those? 
Lets's check gtk/gtk.h path by running *sudo pacman -Ql gtk2* and *pkg-config --cflags --libs gtk+-2.0*. It's in my /usr/include folder. It seems GTK2.0+- and GTK3.0+- comes with manjaro distro.(Manjaro is an Arch Linux with customized desktop.) So it seems cmake can not find the GTK path. Rgrep(emacs command) "gtk+-2.0" and I found this line in CEF CmakeLists.txt file:  

```
FIND_LINUX_LIBRARIES("gmodule-2.0 gtk+-2.0 gthread-2.0 gtk+-unix-print-2.0 gtkglext-1.0")
```  

This macro is defined in cef_macros.cmake:142 for reading pkg-config into cmake. Change this one line in CMakeLists.txt into multiple lines, passing libraries' names one by one.
```
  FIND_LINUX_LIBRARIES("gmodule-2.0")
  FIND_LINUX_LIBRARIES("gtk+-2.0")
  FIND_LINUX_LIBRARIES("gthread-2.0")
  FIND_LINUX_LIBRARIES("gtk+-unix-print-2.0")
  FIND_LINUX_LIBRARIES("gtkglext-1.0")
```
Compile it again, get error <gtk/gtkgl.h> not found. ```sudo pacman -S gtkglext```
Done. So during the compiling step the only package you need to install in Manjaro Linux is gtkglext.  

* Run the command following the build instruction.
EXE="/path/to/cef_binary/build/cefclient/Debug/chrome-sandbox" && sudo -- chown root:root $EXE && sudo -- chmod 7455 $EXE
cd /path/to/cefclient
./cefclient
You will see a simple chrome embeded desktop application runs:  
![](/images/cef.png)





  
  
  


