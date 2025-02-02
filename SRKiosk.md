# Stream Return View

## Setup Raspberry pi

start with RaspberryPiOS Lite. SSH must be enabled.

```bash
sudo apt update
$ sudo apt install snapd
sudo reboot
```

### Install dependencies

```bash
sudo apt update

sudo apt install libsdl2-dev nasm openssl libssl-dev libgnutls28-dev libgles2-mesa-dev xorg-dev build-essential git pulseaudio vlc libavcodec-extra
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

./configure --target-os=linux --enable-gpl --enable-libx264 --enable-nonfree --enable-ffplay --arch=armel

make -j4

sudo make install
```

export DISPLAY=:0
export XDG_RUNTIME_DIR=/run/user/1000
export WAYLAND_DISPLAY=wayland-0


cvlc --gain 6 --height 360 --network-caching=3000 --avcodec-hw=mmal --file-caching=3000 --live-caching=3000 --fullscreen rtmp://capparelli.ie/live/testcard {videourl}

git clone https://github.com/videolan/vlc.git
cd vlc
./bootstrap
./configure --enable-v4l2 --disable-mmal --disable-omxil --disable-lua --disable-qt
make -j$(nproc)
sudo make install

