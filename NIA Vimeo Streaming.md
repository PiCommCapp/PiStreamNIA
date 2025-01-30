# NIA Vimeo Streaming Information

## Player Installation

### Install dependencies

```bash
sudo apt update

sudo apt install libsdl2-dev openssl libssl-dev libgnutls28-dev libgles2-mesa libgles2-mesa-dev xorg-dev build-essential
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

./configure --extra-ldflags="-latomic" --arch=armel --target-os=linux --enable-openssl --enable-mmal --enable-gpl --enable-ffplay --enable-omx --enable-omx-rpi --enable-libx264 --enable-nonfree

make -j4

sudo make install
```
