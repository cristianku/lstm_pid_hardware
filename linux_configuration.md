# LSTM HARDWARE PID
Hardware controlled PID using LSTM neural network

# UBUNTU 24.04 configurations for Deep learning
These are general configurations for Fedora (41) that can be applied to any Deep Learning project

### UBUNTU is installed on a Raid 1 SSD ( two SSD, 1 in mirroring)
[Here you can find instructions how to install in RAID Ubuntu](https://www.youtube.com/watch?v=rJzHpc1kQW4)

#### CUDA TOOLKIT INSTALLATION 
```bash
wget https://developer.download.nvidia.com/compute/cuda/repos/ubuntu2404/x86_64/cuda-keyring_1.1-1_all.deb
sudo dpkg -i cuda-keyring_1.1-1_all.deb
sudo apt-get update
sudo apt-get -y install cuda-toolkit-12-6
````
#### NVIDIA DRIVER INSTALLER
To install the open kernel module flavor:
```commandline
sudo apt-get install -y nvidia-open
```
To install the legacy kernel module flavor:
```commandline
sudo apt-get install -y cuda-drivers
```
#### mounting NVME

##### Creating a partition
```bash
sudo parted /dev/nvme0n1 --script mklabel gpt
sudo parted /dev/nvme0n1 --script mkpart primary ext4 0% 100%
sudo mkfs.ext4 /dev/nvme0n1p1

sudo parted /dev/nvme1n1 --script mklabel gpt
sudo parted /dev/nvme1n1 --script mkpart primary ext4 0% 100%
sudo mkfs.ext4 /dev/nvme1n1p1

cristianku@serverone-deep:~$ lsblk |grep nvme
nvme0n1     259:0    0   1.8T  0 disk  
└─nvme0n1p1 259:2    0   1.8T  0 part  
nvme1n1     259:1    0 931.5G  0 disk  
└─nvme1n1p1 259:4    0 931.5G  0 part 
```

##### Getting the BLKID
```bash
cristianku@serverone-deep:~$ sudo blkid /dev/nvme0n1p1
/dev/nvme0n1p1: PARTLABEL="primary" PARTUUID="86f602aa-ae12-4cd9-aa3c-9ca74816e3c1"

cristianku@serverone-deep:~$ sudo blkid /dev/nvme1n1p1
/dev/nvme1n1p1: PARTLABEL="primary" PARTUUID="ca63c21a-f5ff-46a0-a2e9-e961a89ec0a2"
```

##### Mounting NVME
```bash
sudo mkdir -p /mnt/nvme0
sudo mkdir -p /mnt/nvme1

sudo vi /etc/fstab
UUID=<UUID-for-nvme0n1p1> /mnt/nvme0 ext4 defaults 0 2
UUID=<UUID-for-nvme1n1p1> /mnt/nvme1 ext4 defaults 0 2

sudo mount -a

```