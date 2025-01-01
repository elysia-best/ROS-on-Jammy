# Build ROS noetic on Ubuntu 22.04

## Install build deps

```shell
sudo pip3 install -U rosdep rosinstall_generator vcstool
```

## Prepare `rosdep`

Here I create a rosdep file for Ubuntu 22.04 on my git repo.

```shell
# 手动模拟 rosdep init
sudo mkdir -p /etc/ros/rosdep/sources.list.d/
sudo curl -o /etc/ros/rosdep/sources.list.d/20-default.list https://gitee.com/qinyinan/rosdistro/raw/master/rosdep/sources.list.d/20-default.list
# 为 rosdep update 换源
export ROSDISTRO_INDEX_URL=https://gitee.com/qinyinan/rosdistro/raw/master/index-v4.yaml
rosdep update

# 每次 rosdep update 之前，均需要增加该环境变量
# 为了持久化该设定，可以将其写入 .bashrc 中，例如
echo 'export ROSDISTRO_INDEX_URL=https://gitee.com/qinyinan/rosdistro/raw/master/index-v4.yaml' >> ~/.bashrc
```

## Get Source Code for ROS

In this step, you should make sure that you have a good internet connetion to Github. Source codes will be downloaded from Github.

Some codes are patched so that they can be compiled on Ubunutu 22.04.

```
mkdir -p src

vcs import --input noetic-desktop.rosinstall ./src
```

## Create install dirs

```shell
sudo mkdir -p /opt/ros/noetic
sudo chmod 777 /opt/ros/noetic
```

## Install build dependencies

```
rosdep install --from-paths ./src --ignore-packages-from-source --rosdistro noetic -y
```

## Build and Install ROS

```shell
./src/catkin/bin/catkin_make_isolated --install-space /opt/ros/noetic  --install -DCMAKE_BUILD_TYPE=Release -DCATKIN_ENABLE_TESTING=OFF -DPYTHON_EXECUTABLE=/usr/bin/python3 
```

Wait patiently, there are around 234 packages to build.
