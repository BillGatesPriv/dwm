dwm - suckless window manager

My build of dwm, the minimalist, fast and flexible window manager with focus on productivity.

Most of the code originates from various patches from the [suckless website](https://dwm.suckless.org)  
and additionaly from [Randoragon](https://github.com/Randoragon)'s build of dwm.

## Main features

- Running custom scripts at startup
- New layouts
- Move and resize floating windows with keyboard shortcuts
- Inner and outer gaps resizeable during runtime (compatible with all layouts)
- Execute arbitrary commands by sending signals
- dwmblocks integration - read the [Status Bar Rewrite section](https://github.com/coalbl4ck/dwm#status-bar-rewrite)
- color fonts support (will crash if you don't have [libxft-bgra](https://aur.archlinux.org/packages/libxft-bgra) installed!)
- replaced dmenu binding with rofi


## Bar features

- draw rectangles and customize colors
- transparency
- custom foreground and background colors
- customizable bar text horizontal padding
- custom bar height
- toggle dwmblocks without toggling the entire bar
- dwmblocks won't consume CPU power when bar is toggled off (Currently not implemented for CPU utilization option)

## Applied Patches

- [alpha](https://dwm.suckless.org/patches/alpha/)
- [autostart](https://dwm.suckless.org/patches/autostart/)
- [fancybar](https://dwm.suckless.org/patches/fancybar/)
- [fsignal](https://dwm.suckless.org/patches/fsignal/)
- [moveresize](https://dwm.suckless.org/patches/moveresize/)
- [selfrestart](https://dwm.suckless.org/patches/selfrestart/)
- [status2d](https://dwm.suckless.org/patches/status2d/)
- [vanitygaps](https://dwm.suckless.org/patches/vanitygaps/)

## Status Bar Rewrite

-- Disclaimer -- 

Credit goes entirely to [Randoragon](https://github.com/Randoragon) for this feature, which i copied in part to use in my build of dwm and dwmblocks  


Apparently [XStoreName cannot handle all character encodings well](https://linux.die.net/man/3/xstorename) (see second last paragraph in Description), which
actually caused me many instant crashes that were pretty hard to trace when I was trying to get the bar to display
icons from FontAwesome or Unicode characters. Since that's an inherent weakness of the WM\_NAME property, I thought
of an alternative solution:

Both dwm and dwmblocks initialize a block of shared memory. dwmblocks writes status strings to the shared memory,
then uses [fsignal](https://dwm.suckless.org/patches/fsignal/) to send SIGUSR1 to dwm. dwm updates the status
text from shared memory every time it receives the SIGUSR1 fake signal.

To allow dwm to send signals directly to dwmblocks, by convention dwmblocks will set the first 4 bytes of
shared memory to its PID. Additionally, the 5th byte denotes bar visibility, and the 6th status text visibility.
The purpose is for dwmblocks to only run scripts when necessary, so whenever the 5th byte is 0, it will skip
calling the scripts entirely, and when the 6th byte is 0, it will only call "persistent" scripts.
Note that this only applies to interval-based scripts, signals will be handled as always.

## Installation

In order to build dwm you need the Xlib header files.

Edit config.mk to match your local setup (dwm is installed into
the /usr/local namespace by default).

Afterwards enter the following command to build and install dwm (if
necessary as root):

    make clean install

## References

- dwm is actively developed by the suckless community
    - [suckless.org](https://suckless.org)  
- scripts and shared memory  
	- [Randoragon](https://github.com/Randoragon)  
- some snippets of code (mostly vanitygaps for some layouts) are reimplemented copies originating from Luke Smith's dwm repo
    - [github.com/lukesmithxyz/dwm](https://github.com/lukesmithxyz/dwm)


