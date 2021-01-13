# deej Microbit

You can find the original deej github by clicking [here](https://github.com/omriharel/deej)

deej is an **open-source hardware volume mixer** for Windows and Linux PCs. It lets you **seamlessly control the volumes of different apps** (such as your music player, the game you're playing and your voice chat session) without having to stop what you're doing.

![deejdemo](images/deej-demo-lowrez.gif)


## How to install 
1. Make sure to have your Microbit UNPLUGGED from your Windows PC
2. Download and run [deej-setup.exe](https://github.com/dakota-mewt/mewt/blob/main/deej/deej-setup.exe)
3. Follow the onscreen prompt

## How to run
- run C:\deej\deej.exe

**Join the [deej Discord server](https://discord.gg/nf88NJu) if you need help or have any questions!**

[![Discord](https://img.shields.io/discord/702940502038937667?logo=discord)](https://discord.gg/nf88NJu)



## Table of contents

- [Features](#features)
- [How it works](#how-it-works)
  - [Software](#software)
- [Slider mapping (configuration)](#slider-mapping-configuration)
- [Community](#community)
- [Long-ish term roadmap](#long-ish-term-roadmap)
- [License](#license)

## Features

deej is written in Go and [distributed](https://github.com/omriharel/deej/releases/latest) as a portable (no installer needed) executable.

- Bind apps to different sliders
  - Bind multiple apps per slider (i.e. one slider for all your games)
  - Bind the master channel
  - Bind "system sounds" (on Windows)
  - Bind specific audio devices by name (on Windows)
  - **_New:_** Bind currently active app (on Windows, _experimental_)
  - **_New:_** Bind all other unassigned apps (_experimental_)
- Control your microphone's input level
- Lightweight desktop client, consuming around 10MB of memory
- Runs from your system tray
- Helpful notifications to let you know if something isn't working

## How it works

### Software

- The PC runs a lightweight Go client [`cmd/main.go`](./cmd/main.go) in the background. This client reads the serial stream and adjusts app volumes according to the given configuration file

## Slider mapping (configuration)

`deej` uses a simple YAML-formatted configuration file named [`config.yaml`](./config.yaml), placed alongside the deej executable.

The config file determines which applications (and devices) are mapped to which sliders, and which parameters to use for the connection to the Arduino board, as well as other user preferences.

**This file auto-reloads when its contents are changed, so you can change application mappings on-the-fly without restarting `deej`.**

It looks like this:

```yaml
slider_mapping:
  0: master
  1: chrome.exe
  2: spotify.exe
  3:
    - pathofexile_x64.exe
    - rocketleague.exe
  4: discord.exe

# set this to true if you want the controls inverted (i.e. top is 0%, bottom is 100%)
invert_sliders: false

# settings for connecting to the arduino board
com_port: COM4
baud_rate: 9600

# adjust the amount of signal noise reduction depending on your hardware quality
# supported values are "low" (excellent hardware), "default" (regular hardware) or "high" (bad, noisy hardware)
noise_reduction: default
```

- `master` is a special option to control the master volume of the system _(uses the default playback device)_
- `mic` is a special option to control your microphone's input level _(uses the default recording device)_
- **_New:_** `deej.unmapped` is a special option to control all apps that aren't bound to any slider ("everything else") (_experimental_)
- **_New:_** On Windows, `deej.current` is a special option to control whichever app is currently in focus (_experimental_)
- On Windows, you can specify a device's full name, i.e. `Speakers (Realtek High Definition Audio)`, to bind that device's level to a slider. This doesn't conflict with the default `master` and `mic` options, and works for both input and output devices.
  - Be sure to use the full device name, as seen in the menu that comes up when left-clicking the speaker icon in the tray menu
- `system` is a special option on Windows to control the "System sounds" volume in the Windows mixer
- All names are case-**in**sensitive, meaning both `chrome.exe` and `CHROME.exe` will work
- You can create groups of process names (using a list) to either:
    - control more than one app with a single slider
    - choose whichever process in the group that's currently running (i.e. to have one slider control any game you're playing)






## Community

[![Discord](https://img.shields.io/discord/702940502038937667?logo=discord)](https://discord.gg/nf88NJu)

While `deej` is still a very new project, a vibrant community has already started to grow around it. Come hang out with us in the [deej Discord server](https://discord.gg/nf88NJu), or check out awesome builds made by our members in the [community showcase](./community.md).

The server is also a great place to ask questions, suggest features or report bugs (but of course, feel free to use GitHub if you prefer).

### Donations

If you love deej and want to show your support for the original developer of this project (I just ported it from Arduino to the Microbit), you can do so using the link below. Please don't feel obligated to donate - building the project and telling your friends about it goes a very long way! Thank you very much.

[![ko-fi](https://www.ko-fi.com/img/githubbutton_sm.svg)](https://ko-fi.com/omriharel)

## Long-ish term roadmap

- Serial communications rework to support two-way data flows for better extensibility
- Basic GUI to replace manual configuration editing
- Feel free to open an issue if you feel like something else is missing

## License

`deej` is released under the [MIT license](./LICENSE).
