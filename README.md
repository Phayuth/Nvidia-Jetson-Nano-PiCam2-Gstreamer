# Nvidia-Jetson-Nano-PiCam2-Gstreamer
Jetson Nano, with PiCam2, Gstreamer, Gstreamer Pipeline
# Connection
# Installation
## Gstreamer-1.0 Installation and Setup
```
$ sudo add-apt-repository universe
$ sudo add-apt-repository multiverse
$ sudo apt-get update
$ sudo apt-get install gstreamer1.0-tools gstreamer1.0-alsa gstreamer1.0-plugins-base gstreamer1.0-plugins-good gstreamer1.0-plugins-bad gstreamer1.0-plugins-ugly gstreamer1.0-libav
$ sudo apt-get install libgstreamer1.0-dev libgstreamer-plugins-base1.0-dev libgstreamer-plugins-good1.0-dev libgstreamer-plugins-bad1.0-dev
```
#### Gstreamer Command
Check version
```
$ gst-inspect-1.0 --version
```
Gstreamer Launch
```
$ gst-launch-1.0 {Pipeline}
```
Gstreamer plug-in inspect
```
$ gst-inspect-1.0 {plug-in name}
```
#### Gstreamer simple Pipeline command

# Video Source

## Video Encode

# Camera Source

## Camera Stream

## Camera Record
