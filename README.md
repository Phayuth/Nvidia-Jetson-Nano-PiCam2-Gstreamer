# Nvidia-Jetson-Nano-PiCam2-Gstreamer
Jetson Nano, with PiCam2, Gstreamer, Gstreamer Pipeline
![](https://github.com/Phayuth/Nvidia-Jetson-Nano-PiCam2-Gstreamer/blob/master/PiCam2_JSNano_RTSP.jpg?raw=true)
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
#### Gstreamer Example Pipeline command
```
$ gst-launch-1.0 nvarguscamerasrc ! nvoverlaysink -e
```
Explanation :\
Gstreamer use pipeline command to construct it work, thus writing a correct pipelink command is a key to make it work.\
Gstreamer pipeline is constructed in a sequential line : {media source} ! {modification} ! {media sink} \
Where '!' is a symbol used to separate the command.\
\
In above Pipeline commad :
- nvarguscamerasrc = video stream source coming from RaspPi Camera 2
- nvoverlaysink    = sink the camera stream in disply window
#### Media Source
{media source} can be replaced with any media source such as camera stream, local media file, network media file
#### Modification
{modification} can be replaced with modification command that allow user to alter the media to their desire such as flip, rotate, filter, convert file format, encode, decode, resize, payload stream, -etc.
#### Media sink
{media sink} can be replaced with sink command to display the media or forward it to another network.
# Camera Source
Camera source can come in differents feed. There are CSI cameras, V4L2 cameras.\
To list any available cameras and supprot format
```
$ v4l2-ctl --list-devices
```
#### CSI cameras
Usually directly connect to camera header on Nvidia Jetson Devices or Raspberry Pi. For example a commonly use RaspPi CamV2.
#### V4l2 cameras
Usually directly connect to the device using USB interface such as Webcam. For example Logitech cam C series.\
Install V4l2 device
```
$ sudo apt-get install v4l-utils
$ v4l2-ctl --device=/dev/video0 --list-formats-ext
```
For more info https://github.com/dusty-nv/jetson-inference/blob/master/docs/aux-streaming.md#source-code
## Camera Stream
Stream video from camera locally\
RaspPi CamV2 source
```
$ gst-launch-1.0 nvarguscamerasrc ! 'video/x-raw(memory:NVMM), width=(int)1280, height=(int)720, format=(string)NV12, framerate=(fraction)30/1' ! nvoverlaysink -e
```
USB Camera Source
```
$ gst-launch-1.0 v4l2src device="/dev/video1" ! "video/x-raw, width=640, height=480, format=(string)YUY2" ! xvimagesink -e
```
## Camera Record
Record video from Camera and save into file
```
gst-launch-1.0 nvarguscamerasrc maxper f=1 ! 'video/x-raw(memory:NVMM), width=(int)1280, height=(int)720, format=(string)NV12, framerate=(fraction)30/1' ! nvv4l2h265enc control-rate=1 bitrate=8000000 ! 'video/x-h265, stream-format=(string)byte-stream' ! h265parse ! qtmux ! filesink location=test.mp4 -e 
```
# Streaming RaspPi CameraV2 from Jetson Nano on rtsp-server and view on VLC Player on Window PC
Make sure both Jetson Nano and PC are connected to the same network\
On Jetson Nano, Install rtsp-server library
```
$ sudo apt-get install libgstrtspserver-1.0
```
Clone gst-rtsp-server-master repository
```
$ git clone https://github.com/GStreamer/gst-rtsp-server
```
Go to gst-rtsp-server-master directory
```
$ cd ~/gst-rtsp-server-master/example
$ gcc test-launch.c -o test-launch $(pkg-config --cflags --libs gstreamer-1.0 gstreamer-rtsp-server-1.0)
```
While in that directory
```
$ sudo ./test-launch "videotestsrc ! nvvidconv ! nvv4l2h265enc ! h265parse ! video/x-h265, stream-format=byte-stream ! rtph265pay name=pay0 pt=96 "
```
View Camera Stream via VLC network rtsp://JETSON_IP:8554/test _Pipeline by matteoluci81|nvforum_
