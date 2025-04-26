# Setup

## Upgrade Python

Check your existing version, we are aiming for python `3.11.x` and pip `25.x`

```
$ python --version

$ pip --version
```

Install/upgrade, if needed.
```
$ sudo apt install -y software-properties-common

$ sudo add-apt-repository -y ppa:deadsnakes/ppa

$ sudo apt-get install python3.11 python3.11-dev python3.11-venv -y
```

Link `python` to the new version
```
$ sudo ln -s -f /usr/bin/python3.11 /usr/bin/python
```

Install pip
```
$ curl -sS https://bootstrap.pypa.io/get-pip.py | python
```

Make a directory for building FFmpeg for NVIDIA
```
$ mkdir development && cd development/
```

Install the CUDA toolkit
```
wget https://developer.download.nvidia.com/compute/cuda/repos/ubuntu2004/x86_64/cuda-keyring_1.1-1_all.deb

sudo dpkg -i cuda-keyring_1.1-1_all.deb

sudo apt-get update

sudo apt-get -y install cuda-toolkit-12-8

sudo apt-get install -y nvidia-open
```
Guide - https://developer.nvidia.com/cuda-downloads?target_os=Linux&target_arch=x86_64&Distribution=Ubuntu&target_version=20.04&target_type=deb_network

Follow the steps here to build FFmpeg - https://docs.nvidia.com/video-technologies/video-codec-sdk/12.0/ffmpeg-with-nvidia-gpu/index.html

Verify FFmpeg for NVIDIA
```
$ ffmpeg -decoders | grep -i nvidia
```

Install pytorch and tooling
```
# This corresponds to CUDA Toolkit version 12.8. It should be the same one
# you used when you installed PyTorch (If you installed PyTorch with pip).

$ pip install torchcodec torch torchvision torchaudio --index-url https://download.pytorch.org/whl/cu128
```
