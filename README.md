
## Build image
Inside cloned repository
```
docker build -f Dockerfile.noetic -t cartographer_noetic .
```
## Create container, install cartographer
```
docker run -it --net=host --name cartographer_kitti_noetic cartographer_noetic bash
```
Inside container

```
source /ros_entrypoint.sh
mkdir -p kitti_ws/src
cd kitti_ws/src
git clone https://github.com/inkyusa/cartographer_kitti_config.git
cd ..
catkin_make
```

## Download rosbag and make sure to adapt path when executing 'rosbag play'
https://github.com/inkyusa/cartographer_kitti_config

## DEMO Cartographer noetic
Follow these steps to launch a demo in rviz with the recorded bags
```
docker start cartographer_kitti_noetic
```

in both terminals, run

```
docker exec -it cartographer_kitti_noetic bash
```

### Terminal 1
```
source /ros_entrypoint.sh 
source /kitti_ws/devel/setup.bash
roslaunch kitti_ws/src/cartographer_kitti_config/launch/kitti_3d.launch
```

### Terminal 2
```
source /ros_entrypoint.sh 
rosbag play --clock /kitti/oxts/imu:=imu /kitti/velo/pointcloud:=points2 /tf:=/tf_in rosbags/kitti_2011_09_30_drive_0027_synced_imu_velo.bag
```

## For the visualization in rviz, download the config on your host system and run it with rviz
https://github.com/inkyusa/cartographer_kitti_config/blob/master/configuration_files/kitti_3d_new.rviz

Terminal Host

```
rosrun rviz rviz -d <path_to_your_file>/kitti_3d_new.rviz 
```
