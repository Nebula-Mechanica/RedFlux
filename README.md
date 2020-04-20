# RedFlux
CLI and GUI frontend for Redshift, inspired by f.lux.

Main priority is to give user full control on color temperature, not in the geographically automated way. This is for people like me that use blue light filter to reduce load on eyes whole day long, not just to fall asleep faster.

![Screenshot](images/screenshot.png)

This repo consists of two scrpits:

* RedFlux (redflux) - CLI wrapper around Redshift, meant to be used by itself, ot with hotkey managers or in other scripts
* RedFlux GUI (redflux-gui) - GUI (Tk) wrapper around RedFlux, meant to be controlled by the user in graphical environment

## Dependencies

To use RedFlux CLI following things are required:

* Redshift
* Bash (*not just a POSIX shell*)

Optional dependencies (used only if component exists in system):

* libnotify (notify-send and some notification daemon)
* Redshift GUI

To use RedFlux GUI following things are required:

* RedFlux (**and its dependencies**)
* Tcl/Tk/wish (*available in repositories of most Linux distributions*)
* procps-ng/psmisc (*very common components, most probably already installed on your system*)
* tkbash (*bundled with RedFlux as redflux-tkbash in **3rd-party** folder*)

Optional dependencies (used only if component exists in system):

* yad (*for tray icon*)

## Installation

Installation is described in INSTALL.md file.

## How to use

### RedFlux

You could use RedFlux standalone, or with help of your favorite window manager's hotkeys. To use standalone, just run `redflux enable`. That will enable redflux for you with default temperature of 4200K (you could change default right into redflux file).

You may notice that there is no background process - that is, Redshift works like this by setting temperature directly into X Server, without any kind of overlay.

To disable redflux just run `redflux disable`. That will restore your screen temperature to normal 6500K (the **natural** temperature).

To alter temperature you may run something like `redflux 200` or `redflux -200` and that will add or subtract from current temperature. Remember that higher means colder. Or you could run something like `redflux set 3200` to set temperature to 3200 directly.

This changes will survive disable/enable process, so if you just want things back to default, run `redflux reset`.

All this actions will by default trigger two things: display notification about the performed action and notify RedFlux GUI about change of current state. If you have no notification daemon or RedFlux GUI related commands will be skipped, but you will see warnings. To disable them, launch RedFlux with options `--no-notifications` to suppress notification warnings and/or `--no-send-updates` to suppress missing RedFlux GUI warnings.

All this options could also be called from your hotkey manager, and there is one extra command - `redflux init`. It's basically the same as `redflux enabled`, but doesn't trigger notification - useful for autostart with user session.

### RedFlux GUI

Using Redflux GUI is quite simple. Just launch the command `redflux-gui` and (if dependencies are OK) you will see the window like on screenshot at this page.

**You may notice elements appearing on screen sequentially, it is not a bug, but a limitation of tkbash. Fortunately this happens only on start of a program, but not when you minimize/unminimize it or hide/unhide from the tray.**

In window there are main control buttons. Left four buttons are quite self-describing and looks just like options to RedFlux CLI. On right row you may see presets where you can choose temperature of your screen. Most names and values are intentionally borrowed from f.lux. The square near preset represent how the white color will look after transformation.

Color distortion meter on left bottom is just a static image, but when you will change temperature, color distortion will be visible by eating blue then red to black color. That's why you shouldn't draw or design something with heavy presets. The warmest preset is 1000K and, unlike f.lux it still keeps some green light, so image is not *redscale*. It's a limitation of Redshift, while f.lux can go down to 800K. 6500K is the natural color. On bottom right you can enter exact temperature you want.

Actually, Redshift allows you to set even colder temperatures, then natural, but it's completely useless in our case so disallowed. If you are curious, you can modify MAXTEMP value in RedFlux.

By the way, four left control buttons could be used with hotkeys displayed. No global hotkeys, though, use your window manager with redflux to achieve them.

There also is a tray support in RedFlux GUI. To use it, make sure that you have **yad** software.

To control features of RedFlux GUI you can use options described. Order of options is not important, if there are conflicting options, the last one is taken into account. RedFlux GUI detects absence of optional components and disables its support automatically, ignoring corresponding options.

Options:

* --tray - enable tray support (default)
* --no-tray - disable tray support
* --hotkeys - enable hotkeys (default)
* --no-hotkeys - disable hotkeys
* --start-hidden - start hidden in tray if supported, otherwise start minimized
* --no-start-hidden - do not start hidden (default)
