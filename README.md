# acfm_video_3d_reconstruction installation instructions
NYCU undergraduate students' project notes for building env of acfm_video_3d_reconstruction on conda


Original project page: [Learning monocular 3D reconstruction of articulated categories from motion](https://fkokkinos.github.io/video_3d_reconstruction/)

## Requirements
* Python 3.6+
* PyTorch 1.7.0
* PyTorch3D 0.3.0, there are some breaking changes with more recent versions.
* cuda 11.0

## Installation
### download source code
```console
$ git clone https://github.com/fkokkinos/acfm_video_3d_reconstruction
$ cd acfm_video_3d_reconstruction/
```

### install default env 
```console
$ conda create -n acfm
$ conda activate acfm
(acfm)$ conda env update --file environment.yml
(acfm)$ conda activate acfm
```

### download pytorch
```console
(acfm)$ conda install -c pytorch pytorch=1.7.0 torchvision cudatoolkit=11.0
```

### install pytorch3d prerequisites
Reference: [facebookresearch/pytorch3d](https://github.com/facebookresearch/pytorch3d/blob/main/INSTALL.md)
```console
(acfm)$ conda install -c fvcore -c iopath -c conda-forge fvcore iopath
(acfm)$ curl -LO https://github.com/NVIDIA/cub/archive/1.10.0.tar.gz
(acfm)$ tar xzf 1.10.0.tar.gz
(acfm)$ export CUB_HOME=$PWD/cub-1.10.0
(acfm)$ conda install jupyter
(acfm)$ pip install scikit-image matplotlib imageio plotly opencv-python
(acfm)$ pip install black 'isort<5' flake8 flake8-bugbear flake8-comprehensions
```

### install pytorch3d
#### Method 1: from prebuilt binaries
This method may not warn you of GPU incompatibility.
```console
(acfm)$ conda install pytorch3d -c pytorch3d
```
In training section, if system output error messages like
```console
ImportError: ~/.conda/envs/pytorch3d/lib/python3.8/site-packages/pytorch3d/_C.cpython-38-x86_64-linux-gnu.so: undefined symbol: _ZN3c104cuda12device_countEv
```
rebuild the pytorch3d by method 2.

#### Method 2: from a local clone
Recommend!
```console
(acfm)$ git clone https://github.com/facebookresearch/pytorch3d.git
(acfm)$ cd pytorch3d && pip install -e .
```
To rebuild it after installing from a local clone run.
```console
(acfm)$ rm -rf build/ **/*.so
(acfm)$ pip install -e .
```

If error messages show that

```console
fatal error: cuda_runtime_api.h: No such file or directory
5 | <cuda_runtime_api.h>
  | ^~~~~~~~~~~~~~~~~~~~
```
solve by exporting CUDA_HOME
```console
(acfm)$ export CUDA_HOME=/usr/local/cuda-11
```
In this project, if you run on RTX 30 series, you may get errors

```console
GeForce RTX 3080 with CUDA capability sm_86 is not compatible with the current PyTorch installation. The current PyTorch install supports CUDA capabilities sm_37 sm_50 sm_60 sm_61 sm_70 sm_75 .
```

I recommend you get an suppored GPU, e.g. GTX 1080 Ti.

### install optical flow package

```console
(acfm)$ cd multiframe/data/optical_flow/model/correlation_package/
(acfm)$ python setup.py install
```
If system warns that *RuntimeError: Error compiling object for extension*, then
```console
# before python setup.py install
(acfm)$ export CUDA_HOME=/usr/local/cuda-11
```

## Train
```console
(acfm)$ CUDA_VISIBLE_DEVICES=0 python3 main.py --name=bird_net --num_lbs 32 --symmetric_texture=False --nz_feat 256 --cam_loss_wt 2. --mask_loss_wt 2. --symmetric=False --print_freq 100 --display_freq 100 --boundaries_reg_wt 1. --bdt_reg_wt 0.1 --edt_reg_wt 0.1  --tex_size 6 --save_epoch_freq 10 --kp_loss_wt 50. --tex_loss_wt 1.
```

If you save data on other directory, set --cub_dir. For example
```console 
# my direction
(acfm)$ --cub_dir ../../../../../data/dd/CUB_200_2011/
```

### other problems
If terminal outputs

ValueError: numpy.ndarray size changed, may indicate binary incompatibility. Expected 88 from C header, got 80 from PyObject

update pip
```console
pip install --upgrade numpy
```

Training on a server leads to over-requesting
```console
ConnectionRefusedError: [Errno 111] Connection refused
```
We can solve it by
```console
# terminal 1 
$ python -m visdom.server

# terminal 2
CUDA_VISIBLE_DEVICES=0 python3 main.py --name=bird_net --num_lbs 32 --symmetric_texture=False --nz_feat 256 --cam_loss_wt 2. --mask_loss_wt 2. --symmetric=False --print_freq 100 --display_freq 100 --boundaries_reg_wt 1. --bdt_reg_wt 0.1 --edt_reg_wt 0.1  --tex_size 6 --save_epoch_freq 10 --kp_loss_wt 50. --tex_loss_wt 1.
```

## Resource monitoring for monocular
### Hardware: 
CPU: i7-8700K
* RAM: ~9G

GPU: 1080 Ti
* temperature: 60
* Volatile GPU-util: 90~100%
* RAM: ~5G

### Other:
* Time: ~9 hr(50 epoches)

## Result
[acfm models](https://drive.google.com/drive/folders/1Ds4FzAG1DL31mm0-DRtpAbMLTd_bK47l?usp=sharing)
## terminal cheatsheet
### run 
```console
$ source activate_conda.sh
```

### exit
```console
(acfm)$ source deactivate_conda.sh
```
Hint: in activate_conda.sh and deactivate_conda.sh 
```sh
# in activate_conda.sh
conda activate acfm

# in deactivate_conda.sh
conda deactivate
```
You can modify ~/.bashrc

```sh
alias acfm='conda activate acfm'
alias deacfm='conda deactivate'
```

### build 
```
$ conda create -n acfm
```

### remove
```console
$ conda env remove -n acfm
```
