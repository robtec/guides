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

Install pip
```
$ curl -sS https://bootstrap.pypa.io/get-pip.py | python
```

Compile `ffmpeg` for Nvidia GPUs (possibly option, standard install may work)

https://docs.nvidia.com/video-technologies/video-codec-sdk/12.0/ffmpeg-with-nvidia-gpu/index.html#compiling-for-linux

Install CUDA toolkit (linux, ubuntu 20.04, x86)

https://developer.nvidia.com/cuda-downloads?target_os=Linux&target_arch=x86_64&Distribution=Ubuntu&target_version=20.04&target_type=deb_local

Install pytorch and tooling
```
# This corresponds to CUDA Toolkit version 12.8. It should be the same one
# you used when you installed PyTorch (If you installed PyTorch with pip).

$ pip install torchcodec torch torchvision torchaudio --index-url https://download.pytorch.org/whl/cu128
```
