### Start yolov5/scaled-yolov4 on jetson nano
**Install all necessary packages**
1. Install pytorch 
* install the dependencies (if not already onboard)
```
$ sudo apt-get install python3-pip libopenblas-dev libopenmpi-dev libomp-dev
$ sudo -H pip3 install future
```
* upgrade setuptools 47.1.1 -> 54.0.0
```
$ sudo -H pip3 install --upgrade setuptools
$ sudo -H pip3 install Cython
```
* install gdown to download from Google drive
`$ sudo -H pip3 install gdown`
* copy binairy
`$ sudo cp ~/.local/bin/gdown /usr/local/bin/gdown`
* download the wheel
`$ gdown https://drive.google.com/uc?id=1aWuKu8eqkZwVzFFvguVuwkj0zdCir9qX`
* install PyTorch 1.7.0
`$ sudo -H pip3 install torch-1.7.0a0-cp36-cp36m-linux_aarch64.whl`
* clean up
`$ rm torch-1.7.0a0-cp36-cp36m-linux_aarch64.whl`

2. Install Torchvision
# the dependencies
```
$ sudo apt-get install libjpeg-dev zlib1g-dev libpython3-dev
$ sudo apt-get install libavcodec-dev libavformat-dev libswscale-dev
```
# install gdown to download from Google drive, if not done yet
`$ sudo -H pip3 install gdown`
# copy binairy
`$ sudo cp ~/.local/bin/gdown /usr/local/bin/gdown`
# download TorchVision 0.8.1
`$ gdown https://drive.google.com/uc?id=1WhplBjODLjNmYWEvQliCdkt3CqQTsClm`
# install TorchVision 0.8.1
`$ sudo -H pip3 install torchvision-0.8.1a0+45f960c-cp36-cp36m-linux_aarch64.whl`
# clean up
`$ rm torchvision-0.8.1a0+45f960c-cp36-cp36m-linux_aarch64.whl`

