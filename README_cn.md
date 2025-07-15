[English](./README.md) | 简体中文

# Hobot TTS

**hobot_tts**提供将文本转化为语音播放功能，订阅文本消息，然后调用TTS软件接口，将文本转化为PCM数据，最后调用ALSA接口播放。

## 重要说明

本项目所需的CPP编译源码来自于：https://github.com/D-Robotics/hobot_tts.git

⚠️ **注意：本仓库仅作为说明文档使用，一切以源地址为准。**

详细的技术文档和最新更新请参考：
- 源代码仓库：https://github.com/D-Robotics/hobot_tts.git
- RDK官方文档：https://developer.d-robotics.cc/rdk_doc/Robot_development/quick_demo/hobot_tts

## 编译教程

### 准备工作
确保您的RDK系统已经安装了TROS(TogetherROS)环境。

> **📌 注意：** 安装TROS后，若系统已经内置hobot_tts功能包，可以直接使用。如果需要自定义开发或使用最新版本，才需要进行以下编译步骤。

### 获取源码
```bash
# 创建工作空间
mkdir -p ~/tros_ws/src
cd ~/tros_ws/src

# 克隆源码
git clone https://github.com/D-Robotics/hobot_tts.git
```

### 编译功能包
```bash
# 进入工作空间根目录
cd ~/tros_ws

# 设置TROS环境
source /opt/tros/humble/setup.bash

# 编译功能包
colcon build --packages-select hobot_tts

# 如果需要编译所有功能包
# colcon build
```

### 设置环境变量
```bash
# 设置编译后的环境
source ~/tros_ws/install/setup.bash

# 建议将此命令添加到 ~/.bashrc 中以便自动加载
echo "source ~/tros_ws/install/setup.bash" >> ~/.bashrc
```

### 验证编译
```bash
# 检查功能包是否编译成功
ros2 pkg list | grep hobot_tts
```

## 环境搭建

运行hobot_tts，需要确认音频设备设置正确，具体设置方法参考RDK用户手册[音频转接板](https://developer.horizon.cc/documents_rdk/hardware_development/rdk_x3/audio_board)章节。

可使用如下命令检查是否设置正确：

```bash
root@ubuntu:~# ls /dev/snd/
by-path  controlC0  pcmC0D0c  pcmC0D1p  timer
```

如果出现例如`pcmC0D1p`音频设备则表示设置正确。

## 运行方式

首次运行需要下载模型文件解压，详细命令如下：

```bash
wget http://archive.d-robotics.cc/tts-model/tts_model.tar.gz
sudo tar -xf tts_model.tar.gz -C /opt/tros/${TROS_DISTRO}/lib/hobot_tts/
```

启动程序：

```bash
source /opt/tros/setup.bash

# 屏蔽调式打印信息
export GLOG_minloglevel=1

ros2 run hobot_tts hobot_tts
```

运行成功后，程序订阅topic "/tts_text"（消息类型为std_msgs/msg/String），然后转化为语音信号播放。

注意：加载音频驱动时，若新增音频设备不是`pcmC0D1p`，例如`pcmC1D1p`，则需要使用参数`playback_device`指定播放音频设备。

## 参数列表

| 参数名          | 解释             | 类型        | 是否必须 | 默认值      |
| --------------- | ---------------- | ----------- | -------- | ----------- |
| topic_sub       | 订阅的文本topic  | std::string | 否       | "/tts_text" |
| playback_device | 语音播放音频设备 | std::string | 否       | "hw:0,1"    |