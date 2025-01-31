# Stream Return View

## Setup Raspberry pi

start with RaspberryPiOS Lite. SSH must be enabled.

sudo apt update
$ sudo apt install snapd
sudo reboot

### Install dependencies

```bash
sudo apt update

sudo apt install libsdl2-dev openssl libssl-dev libgnutls28-dev libgles2-mesa libgles2-mesa-dev xorg-dev build-essential git pulseaudio
```

### Install H264

```bash
cd ~

git clone --depth 1 https://code.videolan.org/videolan/x264

cd x264

./configure --host=arm-unknown-linux-gnueabi --enable-static --disable-opencl --disable-asm

make -j4

sudo make install
```

### Install FFMPEG with options

```bash
cd ~

git clone git://source.ffmpeg.org/ffmpeg --depth=1

cd ffmpeg

./configure --arch=armel --target-os=linux --enable-gpl --enable-ffplay --enable-libx264 --enable-nonfree

make -j4

sudo make install
```

export DISPLAY=:0
export XDG_RUNTIME_DIR=/run/user/1000
export WAYLAND_DISPLAY=wayland-0


console=serial0,115200 console=tty1 root=PARTUUID=073fabce-02 rootfstype=ext4 fsck.repair=yes rootwait cfg80211_regdom=GB video=HDMI-A-1:1920x1080M@50

vlc --fullscreen --gain 6 --height 360 rtmp://capparelli.ie/live/NIAstream1


To use MMAL (Multi-Media Abstraction Layer) with Ubuntu Frame running ROSLite on your Raspberry Pi, you'll need to make sure that MMAL is available and that your environment is correctly set up, as you're running a headless setup with no GUI. Here's a general guide on how to do this:

Steps to Make MMAL Available to Ubuntu Frame with ROSLite
1. Install Required Packages
First, ensure that the necessary packages for hardware acceleration and MMAL support are installed on your Raspberry Pi. MMAL is part of the Raspberry Pi's official libraries, but we need to ensure all dependencies are met.

bash
Copy
sudo apt update
sudo apt install libraspberrypi-dev libraspberrypi0 libgles2-mesa libegl1-mesa
sudo apt install libmmal-dev libmmal-core-dev libmmal-util-dev
These packages include the libraries that allow MMAL to interface with the GPU, enabling video output on the Raspberry Pi.

2. Ensure Raspberry Pi's GPU is Available for MMAL
MMAL requires access to the GPU for video output. Since you’re running ROSLite with Ubuntu Frame and no GUI, we need to confirm that the GPU is being used properly in a headless environment.

Edit the config.txt file in the boot partition to enable hardware video decoding and other GPU features:

bash
Copy
sudo nano /boot/firmware/config.txt
Ensure that the following lines are present (uncomment them if needed):

ini
Copy
# Enable MMAL hardware acceleration
gpu_mem=128
start_x=1
disable_splash=1
gpu_mem=128 allocates memory to the GPU.
start_x=1 ensures that the video hardware is initialized, even if there is no graphical environment.
disable_splash=1 disables the splash screen (useful for headless setups).
Reboot the Raspberry Pi after saving changes:

bash
Copy
sudo reboot
3. Set Up Ubuntu Frame to Use MMAL
Since Ubuntu Frame is a display server for rendering graphical UIs (like ROS interfaces) on a Raspberry Pi, you'll need to ensure that it can interface with MMAL for rendering. To do this, you can modify the environment variables or configuration to force Ubuntu Frame to use MMAL for video output.

Use MMAL for rendering: Since you're running Ubuntu Frame on a headless system, Ubuntu Frame will typically use the framebuffer (fbdev) or a GPU-accelerated video output like MMAL. Ensure that the Wayland or Weston compositor (used by Ubuntu Frame) is set to use MMAL for video output.
You can specify the Wayland backend by setting the environment variables:

bash
Copy
export XDG_SESSION_TYPE=wayland
export WAYLAND_DISPLAY=wayland-0
These should be set in the system's environment configuration or the ~/.bashrc file (or ~/.profile) to persist across sessions.

4. Running VLC or Other Applications Using MMAL
Once the Raspberry Pi is set up to use the GPU and MMAL is available, you can attempt to run VLC or any other video-playing application that supports MMAL, while running ROSLite or Ubuntu Frame.

For VLC, you would force MMAL as the video output:

bash
Copy
vlc --vout mmal rtmp://your_stream_url
For other applications or scripts, ensure you specify the MMAL output or video rendering option. If you are using FFMPEG or GStreamer, you may also want to specify hardware-accelerated decoding.

5. Run ROSLite with Ubuntu Frame (Headless Setup)
If you're running ROSLite (or any headless ROS setup), you will likely be using roscore or similar commands without a GUI. You can still launch and use Ubuntu Frame as your compositor to manage graphical applications or UI elements that may be part of your ROS nodes.

Here's a basic example of running Ubuntu Frame with ROSLite:

bash
Copy
export WAYLAND_DISPLAY=wayland-0
export XDG_SESSION_TYPE=wayland

# Start Ubuntu Frame with your configuration
ubuntu-frame -snap ros-core --no-dbus
This way, Ubuntu Frame will be able to manage rendering for applications running on the Raspberry Pi, and you can make sure it uses the MMAL backend when displaying graphics or playing video.

Additional Tips
Hardware-accelerated video decoding: You may need to ensure that your video application (e.g., VLC, FFmpeg) is using hardware-accelerated decoding. The Raspberry Pi 3B+ supports H.264 decoding via MMAL.

Performance Tuning: Since you are running a headless environment, it's important to check performance by experimenting with buffer sizes and video output configurations in your applications. The Raspberry Pi 3B+ has limited resources, so managing CPU/GPU usage can improve overall performance.

Summary of Key Steps:
Install the necessary MMAL libraries.
Edit /boot/firmware/config.txt to enable GPU access and MMAL support.
Set environment variables for Wayland and Ubuntu Frame.
Use MMAL as the video output (--vout mmal) in VLC or other video players.
Ensure the GPU is correctly initialized by editing system configurations.
With these steps, you should be able to integrate MMAL into a headless setup running Ubuntu Frame and ROSLite on your Raspberry Pi 3B+, allowing you to use hardware-accelerated video output efficiently. Let me know if you run into any specific issues or need further assistance!