A gstreamer wrapper for Agora Linux SDK (sink and src)


## Install gstreamer and dependencies
   sudo apt-get update     
   sudo apt-get --fix-broken --fix-missing install -y libgstreamer1.0-dev libgstreamer-plugins-base1.0-dev libgstreamer-plugins-bad1.0-dev gstreamer1.0-plugins-base gstreamer1.0-plugins-good gstreamer1.0-plugins-bad gstreamer1.0-plugins-ugly gstreamer1.0-libav gstreamer1.0-doc gstreamer1.0-tools gstreamer1.0-x gstreamer1.0-alsa gstreamer1.0-gl gstreamer1.0-gtk3 gstreamer1.0-qt5 gstreamer1.0-pulseaudio   

## Install additional libraries:

   sudo apt-get install -y meson libswscale-dev x264 libx264-dev   
   sudo apt install -y build-essential git libpcre3 libpcre3-dev zlib1g zlib1g-dev libssl-dev unzip     
   sudo apt install -y libavcodec-dev libavformat-dev libavutil-dev nasm libavfilter-dev libopus-dev   
 
## Build and install libagorac library
This assumes you have cloned this repo to your home folder ~   
   wget https://download.agora.io/sdk/release/Agora-RTC-x86_64-linux-gnu-v3.4.217.tgz   
   tar -xvzf Agora-RTC-x86_64-linux-gnu-v3.4.217.tgz   

   cd ~/agora-gstreamer/agora/libagorac  
   sudo ./install.sh ~/agora_rtc_sdk   

## Build this plugin

   cd ~/agora-gstreamer/gst-agora   
   meson build   
   ./install   

## Pipeline Configuration Properties

 appid -- sets agora app id
 
 channel  -- sets agora channel id

 userid   -- sets agora user id (optional)

 silent -- a flag to show/hide debug info

## Run and test

   export GST_PLUGIN_PATH=/usr/local/lib/x86_64-linux-gnu/gstreamer-1.0   
   
 ## agorasink
   
Using a test source:

gst-launch-1.0 -v videotestsrc pattern=ball is-live=true ! video/x-raw,format=I420,width=320,height=180,framerate=60/1   ! videoconvert ! x264enc key-int-max=60 tune=zerolatency ! agorasink appid=xxx channel=test silent=1
   

Using a webcam source:

 gst-launch-1.0 v4l2src ! jpegdec ! videoconvert ! x264enc key-int-max=60 tune=zerolatency ! agorasink appid=xxx channel=test silent=1

 ## agorasrc

   agorasrc can be used to read encoded h264 from an agora channel, here is an example pipleline:

   gst-launch-1.0 -v agorasrc appid=xxx channel=gstreamer userid=xxx ! decodebin ! autovideosink

   where appid and channel is same as agorasink. The value of userid represents which user agorasrc should subscribe to
 
 ## Developer Notes
 gst_agorasink_chain(...) in gstagorasink.c  is the main logic and entrypoint    
 meson.build specifies the files to be built    
 agorac.cpp is related to RTMPG project which we use here as a .so library  
 
