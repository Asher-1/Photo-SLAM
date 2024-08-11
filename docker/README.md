# Docker for Deep Patch Visual Odometry

## Install Dependencies
You can install Docker [here](https://docs.docker.com/engine/install/)

Also add the [nvidia-docker](https://nvidia.github.io/nvidia-docker/) repository
```
curl -s -L https://nvidia.github.io/nvidia-docker/gpgkey | sudo apt-key add -
distribution=$(. /etc/os-release;echo $ID$VERSION_ID)
curl -s -L https://nvidia.github.io/nvidia-docker/$distribution/nvidia-docker.list | \
  sudo tee /etc/apt/sources.list.d/nvidia-docker.list
```
Install the Nvidia container/docker toolkits
```
sudo apt-get update
sudo apt-get install -y nvidia-container-toolkit nvidia-docker2
sudo systemctl restart docker
```

## Container Setup
Build the docker container

```
sudo ./build_container.sh <cuda-version>
```
_Example: If_ `nvidia-smi` _shows_ `CUDA Version: 11.8` _then run_ `./build_container.sh 11.8.0`

Start an interactive shell
```
docker exec -it photo_slam bash
```

(optional) Download the dataset.cp ./Photo-SLAM-eval/TUM/fr1/camera.yaml PATH_TO_TUM_DATASET/rgbd_dataset_freiburg1_desk
cp ./Photo-SLAM-eval/TUM/fr2/camera.yaml PATH_TO_TUM_DATASET/rgbd_dataset_freiburg2_xyz
```
cd scripts
chmod +x ./*.sh
./download_replica.sh
./download_tum.sh
./download_euroc.sh
```

For testing, you could use the below commands to run the system after specifying the `PATH_TO_Replica` and `PATH_TO_SAVE_RESULTS`. We would disable the viewer by adding `no_viewer` during the evaluation.
```
./bin/replica_rgbd \
    ./ORB-SLAM3/Vocabulary/ORBvoc.txt \
    ./cfg/ORB_SLAM3/RGB-D/Replica/office0.yaml \
    ./cfg/gaussian_mapper/RGB-D/Replica/replica_rgbd.yaml \
    ./data/Replica/office0 \
    ./results/replica_rgbd_0/office0
    # no_viewer 
```