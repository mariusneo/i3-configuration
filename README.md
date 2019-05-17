Configuration setup for Linux i3 Window Tiling Manager
======================================================


## Introduction


[i3](https://i3wm.org) is a tiling window manager that I came acquainted with a few days ago.

The basic idea of this window manager is that it provides the user with ability of spliting his
screen in multiple tiles (which is very useful when working with multiple terminals or there's a
need to resize just by using the keyboard the windows to fit completely the surface of the screen).

So far it seems pretty fun to play with it on top of Ubuntu.
In order to avoid forgetting what I learned so far, I've decided to write it down here and
use it on other computers I use.


## i3 Cheat Sheet

[i3 reference card](https://i3wm.org/docs/refcard.html) contains the most usefuls shortcuts
that are needed to get started with i3.


## Installation


```
sudo apt install i3
```

The next time when logging into Ubuntu, there will appear at the login also the option of using
i3 as a window manager.


All my customizations perfomed for i3's config file can be found in [~/.config/i3/config](config) file.



The first unexpected surpise I had on my Dell XPS 15 was that I had to change the resolution to
1920x1080. I could do this with the help of xrandr tool.


Adding to `~/.config/i3/config` file the following line

```
# set the screen resolution
exec xrandr --output eDP-1 --mode 1920x1080
```
saving and restarting i3 inplace (`$mod` + `shift` + `r` shortcut) solved th problem for me.

### Background image

```
sudo apt-get install nitrogen
nitrogen /usr/share/backgrounds
```


### Resizing windows

```
# Resizing windows in i3 using keyboard only
# https://unix.stackexchange.com/q/255344/150597

# Resizing by 1
bindsym $mod+Ctrl+Right resize shrink width 1 px or 1 ppt
bindsym $mod+Ctrl+Up resize grow height 1 px or 1 ppt
bindsym $mod+Ctrl+Down resize shrink height 1 px or 1 ppt
bindsym $mod+Ctrl+Left resize grow width 1 px or 1 ppt

# Resizing by 10
bindsym $mod+Ctrl+Shift+Right resize shrink width 10 px or 10 ppt
bindsym $mod+Ctrl+Shift+Up resize grow height 10 px or 10 ppt
bindsym $mod+Ctrl+Shift+Down resize shrink height 10 px or 10 ppt
bindsym $mod+Ctrl+Shift+Left resize grow width 10 px or 10 ppt
```

### Multimedia keys

Here there are a few extra controls (wi-fi on/off, screen brightness), but I don't have them yet.

```
# Pulse Audio controls
bindsym XF86AudioRaiseVolume exec --no-startup-id pactl set-sink-volume 0 +5% #increase sound volume
bindsym XF86AudioLowerVolume exec --no-startup-id pactl set-sink-volume 0 -5% #decrease sound volume
bindsym XF86AudioMute exec --no-startup-id pactl set-sink-mute 0 toggle # mute sound

# Screen brightness controls
bindsym XF86MonBrightnessUp exec xbacklight -inc 20 # increase screen brightness
bindsym XF86MonBrightnessDown exec xbacklight -dec 20 # decrease screen brightness
```


### Work on multiple monitors

```
# https://faq.i3wm.org/question/5312/how-to-toggle-onoff-external-and-internal-monitors.1.html
# Setup external monitor
bindsym $mod+Shift+s exec --no-startup-id /home/marius/bin/monitor 
```

The referenced file [/home/marius/bin/monitor](monitor) is also available in this repository. 



# Status bar customizations

I've used i3status for customizing the status bar displayed on the bottom of i3 window manager.
It has a small drawback because it refreshes by default only once every 5 seconds - which is 
not quite ideal when switching between keyboard layouts ( I use `en`, `de`, `ro` for my keyboard),
but I can live with how it is.


![i3 status bar](i3-status-bar.png)



See [/etc/i3status.conf](i3status.conf) file content for the customizations made in order to achieve
the bar layout presented above.

In order to benefit of the fancy fonts, the font awesome should be installed.

```
sudo apt-get install fonts-font-awesome
```



# Changing between keyboard layouts

It was rather tricky to achieve changing the keyboard layout and seeing the newly chosen locale
on the status bar (in fact now I see it only with up to 5 seconds delay).

Install [xkblayout-state](https://github.com/nonpop/xkblayout-state) for retrieving the current
keyboard layout.

Afterwards I had to adapt [i3-keyboard-layout](https://github.com/porras/i3-keyboard-layout) to
use `xkblayout-state` in order to display the current keyboard layout.

Files needed:

- [/home/marius/bin/i3-keyboard-layout](i3-keyboard-layout)
- [/home/marius/bin/xkblayout-state](xkblayout-state) - already compiled on my Ubuntu machine



# Rofi window switcher

i3 comes equiped with a rather basic launcher for applications (`$mod` + `d`) shortcut, reason
why I opted to use `rofi`.

```
sudo apt install rofi
```

One of the main advantages of `rofi` is that this is not only an `i3` application launcher, but
also an application switcher. By default via `i3` there's no possibility that I know of for
switching between the opened applications (which may be found in some obscure workspace that
is not currently visible to the user), this is possible via `rofi`.

Switch between the modes (drun/window/ssh) by using `shift` + `left` or `right` keyboard shortcut.


