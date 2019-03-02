# Install-Tensorflow-on-Ubuntu-17.10-18.04
Quick install guide for Ubuntu 17.10/18.04LTS

After spending 3 days with 7 reinstallations of Ubuntu, I did succeed to find a right recipe to run Tensorflow on my Ubuntu 18.04 (on both desktop and laptop).

There is lots of tutorial sources out there, but many of them doesn't work on my fresh Ubuntu because they already have some preisntalled dependencies.

I can understand the frustration when searching for many different error messages in the process and many other people have the same problem. But noone can give them a right answer because this installation require a precise combination of versions of apps. Once one thing messes up, the whole thing messes up.


Let get started
## There is total 9 steps:
Overview:
- Step1: Install fresh Ubuntu
- Step2: Install Anaconda ver. 5.3
- Step3: Install GPU driver nvidia-390
- Step4: Install CUDA Toolkit (9.0)
- Step5: Install CUDNN 7.0.5
- Step6: Install Tensorflow with pip from conda
- Step7: Install dependencies Bazel
- Step8: Control environment of Anaconda guide
- Step9: Test and wish for the best


Let get started


### 1. Installing fresh Ubuntu of 17.10/18.04LTS 

You can install it here https://www.ubuntu.com/download/desktop

Normal installation and no third-party apps (not sure it will affect but you can update driver later).

### 2. Have a newest version of Anaconda [5.3 for me] 

- Install curl and use curl to download Anaconda installation script in /tmp
```
sudo apt-get install curl
cd /tmp
curl -O https://repo.anaconda.com/archive/Anaconda3-5.3.0-Linux-x86_64.sh
```
- Use the sha256sum command to verify the script checksum:
```
sha256sum Anaconda3-5.3.0-Linux-x86_64.sh
```
- You should see an output like the following:
```
cfbf5fe70dd1b797ec677e63c61f8efc92dad930fd1c94d60390bb07fdc09959  Anaconda3-5.3.0-Linux-x86_64.sh
```

- To start the Anaconda installation process run the installation script:
```
bash Anaconda3-5.3.0-Linux-x86_64.sh
```
- Follow the instruction untill Anaconda is installed [You may or maynot install VSCode]

- Next:
To activate the Anaconda installation load the new PATH environment variable which was added by the Anaconda installer into the current shell session with the following command:
```
source ~/.bashrc
```
- Verify the installation by
```
conda list
```

### 3. Install GPU drivers
#### Requirement
- An NVIDIA GPU with a compute capability of 3.0 or higher.
- Check yours here https://developer.nvidia.com/cuda-gpus for enable-CUDA GPU

- We will use driver nvidia-390 for this installation
```
sudo add-apt-repository ppa:graphics-drivers/ppa
sudo apt update
sudo apt install nvidia-390
```
- Reboot your computer. To verify the installation, type in terminal
```
nvidia-smi
```

### 4. Install the CUDA Toolkit (9.0)
- Go to https://developer.nvidia.com/cuda-90-download-archive 

- Choose Linux => x86_64 => Ubuntu => 17.04 => deb \[local\] and Download the "Base Installer"
- Once the download is complete, open a terminal in the directory the base installer is and run the follow commands
```
sudo dpkg -i cuda-repo-ubuntu1704-9-0-local_9.0.176-1_amd64.deb
sudo apt-key add /var/cuda-repo-9-0-local/7fa2af80.pub
sudo apt-get update
sudo apt-get install cuda
```
- From the same website
- Use firefox to open - Install patch 1 and 2 by click and choose option "Open with: Software install (default)"\] and install
- open your .bashrc file with nano
```
sudo nano ~/.bashrc
```
- Go to the last line and add the following lines (this will set your PATH variable)
```
export PATH=/usr/local/cuda-9.0/bin${PATH:+:$PATH}}
export LD_LIBRARY_PATH=/usr/local/cuda-9.0/lib64${LD_LIBRARY_PATH:+:${LD_LIBRARY_PATH}}
```

### 5. Install CUDNN 7.0.5 
- Go to https://developer.nvidia.com/cudnn
- You will need to create an account to Download it. 
- After sign up, find and Download CUDNN 7.0.5 for CUDA 9.0
- cd to the downloaded file foulder and Unzip the tar file using the command
```
tar -xzvf cudnn-9.0-linux-x64-v7.tgz
```
- Run the following commands to move the appropriate files to the CUDA folder
```
sudo cp cuda/include/cudnn.h /usr/local/cuda/include
sudo cp cuda/lib64/libcudnn* /usr/local/cuda/lib64
sudo chmod a+r /usr/local/cuda/include/cudnn.h /usr/local/cuda/lib64/libcudnn*
```
(almost done here)
### 6. Install Tensorflow using conda and pip
- Create a conda environment by 
```
conda create -n tf python=3.6 pip
```
- Activate tf environment using
```
conda activate tf
```
- Install tensorflow-gpu by
```
pip install tensorflow-gpu==1.5
```
- \[Optional\] Install keras 
```
git clone https://github.com/fchollet/keras
cd keras
python setup.py install
```
### 7. Install dependencies (bazel)
- Tensorflow requires bazel to run
- Install JDK 8
```
sudo apt-get install openjdk-8-jdk
```
- Add Bazel distribution URI as a package source
```
echo "deb [arch=amd64] http://storage.googleapis.com/bazel-apt stable jdk1.8" | sudo tee /etc/apt/sources.list.d/bazel.list
curl https://bazel.build/bazel-release.pub.gpg | sudo apt-key add -
```
- Install and update Bazel
```
sudo apt-get update && sudo apt-get install bazel
sudo apt-get install --only-upgrade bazel
```

### 8. Control environment
- Tensorflow will be placed inside "tf" environment of Anaconda.
- So each time before running Tensorflow, you need
```
conda activate tf
```
- When done
```
conda deactivate
```

### 9. Finally, TEST
- Get in tf environment and open python
```
conda activate tf
python
```
- Run follow python lines
```python
>>> import tensorflow as tf
>>> hello = tf.constant('hello tensorflow')
>>> with tf.Session() as sesh:
>>>     sesh.run(hello)
```
- The output should be
```
>>> 'bhello tensorflow'
```

## DONE, good luck.

## Source:
https://www.digitalocean.com/community/tutorials/how-to-install-anaconda-on-ubuntu-18-04-quickstart
https://github.com/markjay4k/Install-Tensorflow-on-Ubuntu-17.10-/blob/master/Tensorflow%20Install%20instructions.ipynb
https://docs.bazel.build/versions/master/install-ubuntu.html







