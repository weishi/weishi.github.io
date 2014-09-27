---
layout: post
title: "Install iOS Header for Theos on OS X Mountain Lion/Mavericks"
date: 2013-09-02 11:08
comments: true
categories: 
---

The default Theos installation, as outlined in 
[this wiki](http://iphonedevwiki.net/index.php/Theos/Getting_Started),
does not include all iOS headers.
The following process installs the necessary headers and patches for OSX Mountain Lion/Mavericks.

+ Follow the [official wiki](http://iphonedevwiki.net/index.php/Theos/Getting_Started) to install theos.
+ Add iphoneheader to `theos/include`

```
git clone https://github.com/rpetrich/iphoneheaders.git
```

+ Copy `IOSurfaceAPI.h` from OSX library

```
cp /System/Library/Frameworks/IOSurface.framework/Headers/IOSurfaceAPI.h theos/include/IOSurface
```

+ Patch `IOSurfaceAPI.h` by commenting out two lines

```
/* This call lets you get an xpc_object_t that holds a reference to the IOSurface.
Note: Any live XPC objects created from an IOSurfaceRef implicity increase the IOSurface's global use
count by one until the object is destroyed. */
//xpc_object_t IOSurfaceCreateXPCObject(IOSurfaceRef aSurface)
//      IOSFC_AVAILABLE_STARTING(__MAC_10_7, __IPHONE_NA);
/* This call lets you take an xpc_object_t created via IOSurfaceCreatePort() and recreate an IOSurfaceRef from it. */
IOSurfaceRef IOSurfaceLookupFromXPCObject(xpc_object_t xobj)
IOSFC_AVAILABLE_STARTING(__MAC_10_7, __IPHONE_NA);
```
