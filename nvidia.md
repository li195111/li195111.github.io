# Ubuntu 18.04 + Nvidia CUDA 10.2 + CUDnn 8

### Nvidia Device
    sudo apt-get update
    sudo ubuntu-drivers devices

### 安裝缺失的套件 --- 重複檢查直到沒有缺失
    sudo ubuntu-drivers autoinstall

    The following packages have unmet dependencies:
    nvidia-driver-xxx : Depends: xserver-xorg-video-nvidia-xxx but it is not going to be installed

    sudo apt-get install nvidia-driver-xxx

### 刪除多餘的套件
    sudo apt autoremove

### 重新開機
    sudo reboot

### 檢查
    nvidia-smi

    正常會看到這個畫面
    Thu Nov 26 21:42:12 2020       
    +-----------------------------------------------------------------------------+
    | NVIDIA-SMI 455.38       Driver Version: 455.38       CUDA Version: 11.1     |
    |-------------------------------+----------------------+----------------------+
    | GPU  Name        Persistence-M| Bus-Id        Disp.A | Volatile Uncorr. ECC |
    | Fan  Temp  Perf  Pwr:Usage/Cap|         Memory-Usage | GPU-Util  Compute M. |
    |                               |                      |               MIG M. |
    |===============================+======================+======================|
    |   0  GeForce RTX 207...  Off  | 00000000:01:00.0  On |                  N/A |
    | 26%   31C    P8     2W / 215W |    287MiB /  7979MiB |      2%      Default |
    |                               |                      |                  N/A |
    +-------------------------------+----------------------+----------------------+
                                                                                
    +-----------------------------------------------------------------------------+
    | Processes:                                                                  |
    |  GPU   GI   CI        PID   Type   Process name                  GPU Memory |
    |        ID   ID                                                   Usage      |
    |=============================================================================|
    |    0   N/A  N/A      1042      G   /usr/lib/xorg/Xorg                168MiB |
    |    0   N/A  N/A      1199      G   /usr/bin/gnome-shell               33MiB |
    |    0   N/A  N/A      3680      G   ...AAAAAAAA== --shared-files       20MiB |
    |    0   N/A  N/A      9758      G   ...AAAAAAAAA= --shared-files       60MiB |
    +-----------------------------------------------------------------------------+

### 安裝 CUDA 10.2
    wget https://developer.download.nvidia.com/compute/cuda/10.2/Prod/local_installers/cuda_10.2.89_440.33.01_linux.run
    sudo sh cuda_10.2.89_440.33.01_linux.run

    # accept -> unselect nvidia drivers -> Install
    rm -rf cuda_10.2.89_440.33.01_linux.run

    # Patch 1
    wget https://developer.download.nvidia.com/compute/cuda/10.2/Prod/patches/1/cuda_10.2.1_linux.run
    sudo sh cuda_10.2.1_linux.run

    # accept -> Install -> upgrade all
    rm -rf cuda_10.2.1_linux.run

    # Patch 2
    wget https://developer.download.nvidia.com/compute/cuda/10.2/Prod/patches/2/cuda_10.2.2_linux.run
    sudo sh cuda_10.2.2_linux.run

    # accept -> Install -> upgrade all
    rm -rf cuda_10.2.2_linux.run

### 修改終端機 modify bash
    sudo gedit ~/.bashrc

### 將下列兩行加到最後
    export PATH=/usr/local/cuda-10.2/bin:$PATH
    export LD_LIBRARY_PATH=/usr/local/cuda-10.2/lib64:$LD_LIBRARY_PATH

### Ctrl + S 存檔 然後回到終端機重新載入
    source /.bashrc

### 檢查 CUDA 是否安裝成功
    nvcc -V

### 成功回傳
    nvcc: NVIDIA (R) Cuda compiler driver
    Copyright (c) 2005-2019 NVIDIA Corporation
    Built on Wed_Oct_23_19:24:38_PDT_2019
    Cuda compilation tools, release 10.2, V10.2.89

### 安裝 CUDnn 8
    # Download from CUDnn Home Page --- need to login nvidia
    https://developer.nvidia.com/cudnn

    cd ~/Downloads

    # Runtime Lib
    sudo dpkg -i libcudnn8_8.0.5.39-1+cuda10.2_amd64.deb
    rm -rf libcudnn8_8.0.5.39-1+cuda10.2_amd64.deb

    # Developer Lib
    sudo dpkg -i libcudnn8-dev_8.0.5.39-1+cuda10.2_amd64.deb
    rm -rf libcudnn8-dev_8.0.5.39-1+cuda10.2_amd64.deb

    # code sample
    sudo dpkg -i libcudnn8-samples_8.0.5.39-1+cuda10.2_amd64.deb
    rm -rf libcudnn8-samples_8.0.5.39-1+cuda10.2_amd64.deb
