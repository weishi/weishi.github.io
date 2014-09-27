---
layout: post
title: "Change Tab Color on Terminator and Ubuntu Terminal"
date: 2014-09-27 10:37
comments: true
categories: ubuntu, gtk, terminal
---

## The Problem

The default color for active tab on Terminator and Ubuntu terminal
is almost indistinguishable.
This can be fixed by using another theme,
but if you want to use your current theme and only tweak the tab color,
read on.

Because `Terminator 0.97` still uses GTK2; 
whereas Ubuntu terminal uses GTK3,
they each require a different method to tweak.

## The Tweak for GTK2

For `Terminator` and any other GTK2 applications,
you can directly edit the `gtkrc` file your theme uses.

```
sudo vim /usr/share/themes/<YOUR_THEME>/gtk-2.0/gtkrc
``` 

Find the following block:

```
style "notebook_bg" {
	bg[NORMAL] = shade (1.02, @bg_color)
	bg[ACTIVE] = shade (0.97, @bg_color)
	fg[ACTIVE] = mix (0.8, @fg_color, shade (0.97, @bg_color))
}
```

Replace `@bg_color` in `bg[NORMAL]` to a color you like,
e.g. `#FCBC8D`.

## The Tweak for GTK3

For `terminal` and any other GTK3 applications,
you can edit the `gtk.css` in your home directory.
It is located in `#HOME/.config/gtk-3.0/gtk.css`.
Create one if it's not there.

Add the following lines to `gtk.css`:

```
TerminalWindow .notebook tab:active{
	background-color: #FCBC8D;
}
```

For the change to take effect, you need to open up a new application.

