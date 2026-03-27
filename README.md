# IMU频率的GPS-IMU 融合算法

本软件包提供了一种基于ESKF的GPS-IMU生成IMU频率里程计的融合算法，使用本软件包时，请注明软件包出处。

详细参考知乎：https://zhuanlan.zhihu.com/p/627857398

## 1. 安装:

### 1.1 安装依赖

```bash
# 必须安装以下依赖
#1. pcl 
#2. GeographicLib
sudo apt-get install libpcl-dev libgeographic-dev
```

### 1.2 编译package

```bash
mkdir -p ~/catkin_ws/src
cd ~/catkin_ws/src
git clone https://gitee.com/dawanzi98/gnss_imu_odom_at_imu_rate.git
cd ../..
catkin_make
```



## 2 运行

包内提供了三个`launch`文件：
kitti的数据集可以从https://github.com/leo6862/ins_eskf_kitti 下载

```
1. kitti.launch: 适用kitti数据集
2. mulran.launch: 适用mulran数据集
```

## 3 配置

位于包内`./config`文件夹下

```yaml
imu_sub_topic: /kitti/oxts/imu/extract # imu话题
gnss_sub_topic: /kitti/oxts/gps/fix # GNSS 话题
cloud_topic: /rslidar_points #点云话题，不开启点云显示节点，该配置无效
# 0 输出gnss频率的里程计，1 输出 imu频率的里程计
hz_mode: 1 #0 for gps rate ; 1 for imu rate
```

