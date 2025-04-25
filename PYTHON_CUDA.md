# Setup

## Upgrade Python

Check your existing version, we are aiming for `3.11.x`

```
$ python --version

$ pip --version
```

Upgrade, if needed.
```
$ sudo apt install -y software-properties-common

$ sudo add-apt-repository -y ppa:deadsnakes/ppa

$ sudo apt-get install python3.11 python3.11-dev python3.11-venv -y
```

Set `python` to the new version
```
$ sudo ln -s -f /usr/bin/python3.11 /usr/bin/python
```
Upgrade pip
```
```

Compile `ffmpeg` for Nvidia GPUs

https://docs.nvidia.com/video-technologies/video-codec-sdk/12.0/ffmpeg-with-nvidia-gpu/index.html#compiling-for-linux
