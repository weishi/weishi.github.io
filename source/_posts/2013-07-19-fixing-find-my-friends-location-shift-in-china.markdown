---
layout: post
title: "Fixing Find My Friends Location Shift in China"
date: 2013-07-19 14:45
comments: true
categories: [iPhone, Jailbreak]
---

Since China uses a different coordinate system,
the common latitude/longitude coordinates obtained from GPS or iOS `CoreLocation` service
will not map to the correct location in China.
The shift is usually 500+ meters, which renders the FindMyFriends app useless.
In order to let your (unjailbroken) friends see your true location,
your reported location need to be shifted.
When they receive your (shifted) location and apply it to a shifted map,
the shift effect cancels out and they see your true location in MapView.

`FindMyFriends.app` reports user location from two places.

+ When the app is in **foreground**, the app itself reports the location.

+ When the app is in **background**, `aosnotifyd` daemon will respond to the location request.
(This daemon is also responsible for `FindMyiPhone.app` location tracking.)

Therefore to fix the location shift, those two places need to be patched.


## com.apple.mobileme.fmf1

In `FindMyFriends.app`, the following function does the reporting:

``` objc ServerInteractionController.h:
-(void)sendMyLocation:(id)location;
```

Using `theos`, we can hook into the method and modify the `(id)location`.

However, we can fix friends location altogether by hooking the location data
structure directly.

``` objc
%hook FMFLocation
- (void)updateLatitude:(id)lat longitude:(id)lng altitude:(id)alt
horizontalAccuracy:(id)acc verticalAccuracy:(id)acc5 course:(id)c speed:(id)s timestamp:(id)ts {
    double nlat,nlng;
    transform([lat doubleValue],[lng doubleValue], &nlat, &nlng);
    NSNumber *olat=[NSNumber numberWithDouble:nlat];
    NSNumber *olng=[NSNumber numberWithDouble:nlng];
    %orig(olat,olng,alt,acc,acc5,c,s,ts);
}
%end
```


## com.apple.aosnotifyd

In `aosnotifyd`, the following function does the reporting:

``` objc AOSFindBaseServiceProvider.h
-(void)sendCurrentLocation:(id)fp8 isFinished:(BOOL)fp1 forCmd:(id)fp2 withReason:(int)fp3 andAccuracyChange:(double)fp4;
```

This can be patched similarly.

## Location shift function

For some reason, I will not discuss the detail of the shift function `transform()`.
Basically it does the conversion from WGS-84(GPS) to GCJ-02 coordinates.
Google `WGS2GCJ` for more information.

## Source code

You can download and build the cydia tweak from my repository:
[github.com/weishi/FMFChinaLocationPatch](https://github.com/weishi/FMFChinaLocationPatch)

