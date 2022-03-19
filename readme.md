## Guide to setup sway Window Manager for the M1 MacBook Air
*I still need to test this out so dont trust everything to work*


## 1. Requirements

* M1 Air with ArchLinux-Arm installed through [Asahi](https://asahilinux.org/)

## 2. Install sway and Terminal emulator

`sudo pacman -Syu`<br>
`sudo pacman -S sway alacritty` (Use the terminal you like)

## 3. Sway Configuration

You Should do this before lauching sway, because the default scaling is too small and you probably wont be able to lauch a terminal.<br>
You are expected to have your own configuration file.<br>
Here you will only find configuration specific to the M1 Macs.

If you dont have a config you can download the default sway config with:
`curl -o config https://raw.githubusercontent.com/swaywm/sway/master/config.in`
Move this file to ~/.config/sway/config

## 4. M1 Air specific configuration
*Most of this should work on other M1 Macs too*

### Command and Option Keys:
Command -> Mod4<br>
Option  -> Mod1

You can set variables at the beginning of the file<br>
`set $com Mod4`<br>
`set $opt Mod1`<br>
So you can use them in your configuration wherever you want:<br>
`bindsym $com+Return exec alacritty`<br>
*Make sure you setup a shortcut to a terminal or a laucher, so you can edit the config when you are in sway.*

### Display
By default everything appears very small on the M1 Screen so we need to scale it up with:
```
output "Unknown-1" {
        scale 2
        scale_filter smart
    }
```
You can check that your display is also called Unknown-1 with `swaymsg -t get_outputs`.<br>
Although it is recommended to keep the scale a natural number you can use decimals to adjust to your liking.<br>
**See more options for sway-output [here](https://man.archlinux.org/man/sway-output.5)**

### Input - Keyboard and Touchpad

Get the id's of the devices with `swaymsg -t get_inputs` and check that the ids are correct.
Everything configured here is according to my personal preference. So you need to decide if it works for you.
I tried to set sensible defaults.<br>
To view other options see [Sway Documentation for Input:](https://man.archlinux.org/man/sway-input.5)

#### Touchpad Configuration:
```
input "1452:641:Apple_Internal_Keyboard_/_Trackpad" {   #Change the id if its different
    dwt enabled                 #Disable while typing
    tap enabled                 #Tap registers as a click
    accel_profile adaptive      #adaptive/flat cursor acceleration
    pointer_accel 0             #From -1 to 1: Speed of cursor
    natural_scroll enabled      #Direction of Scroll
    scroll_factor 0.4           #Speed of Scrolling gets multiplied by this factor
    scroll_method two_finger    #none|two_finger|edge|on_button_down
    middle_emulation enabled    #Enables middle click
    tap_button_map lmr          #1Finger > Left Click , 2Finger > Right Click, 3Fingers > MiddleClick
}
```

#### Keyboard Configuration:
```
input "1452:641:Apple_Internal_Keyboard_/_Trackpad" {   #Change the id if its different
    repeat_rate 30
    repeat_delay 350
    xkb_layout us,es,de
    xkb_options caps:escape_shifted_capslock,grp:win_space_toggle
}
```
Here I set the layouts I use and I remap some keys I find useful:
 - Capslock maps to escape and works as Capslock  with shift
 - Command+Space toggles between layouts
You can see all xkb options [here](https://gist.github.com/jatcwang/ae3b7019f219b8cdc6798329108c9aee).

## 4. Firefox and other XWayland-Apps

Sadly, it seems that the apps that run through XWayland don't integrate well with the scaling and get blurry.

Firefox runs by default on XWayland, but it can run on Wayland too:)

You just need to lauch it with an enviroment variable:

`$ MOZ_ENABLE_WAYLAND=1 firefox`

I highly recommend that you do this.

More info on how to do this [here](https://wiki.archlinux.org/title/firefox#Wayland).


Keep in mind that every other app that relies on XWayland also gets blurry.



*If you find any mistakes, or have improvements for this guide, feel free to contribute:)*
