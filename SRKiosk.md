# Stream Return View

## Setup Raspberry pi

start with RaspberryPiOS Lite. SSH must be enabled.

sudo apt update
$ sudo apt install snapd
sudo reboot

### Install dependencies

```bash
sudo apt update

sudo apt install libsdl2-dev openssl libssl-dev libgnutls28-dev libgles2-mesa libgles2-mesa-dev xorg-dev build-essential git
```

### Install H264

```bash
cd ~

git clone --depth 1 https://code.videolan.org/videolan/x264

cd x264

./configure --host=arm-unknown-linux-gnueabi --enable-static --disable-opencl

make -j4

make install
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
vlc --fullscreen --gain 6 --height 360 rtmp://86.20.185.133/live/teststream