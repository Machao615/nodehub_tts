English| [简体中文](./README_cn.md)

# Hobot TTS

**hobot_tts** provides the function to convert text into speech for audio playback. It subscribes to text messages, then calls the TTS software interface to convert the text into PCM data, and finally uses the ALSA interface for playback.

## Important Notice

The CPP compilation source code required for this project comes from: https://github.com/D-Robotics/hobot_tts.git

⚠️ **Note: This repository is only used as documentation. Everything is based on the source address.**

For detailed technical documentation and latest updates, please refer to:
- Source code repository: https://github.com/D-Robotics/hobot_tts.git
- Official RDK documentation: https://developer.d-robotics.cc/rdk_doc/Robot_development/quick_demo/hobot_tts

## Compilation Tutorial

### Prerequisites
Ensure that your RDK system has TROS (TogetherROS) environment installed.

> **📌 Note:** After installing TROS, the system already has the hobot_tts package built-in and can be used directly. The following compilation steps are only needed if you require custom development or want to use the latest version.

### Get Source Code
```bash
# Create workspace
mkdir -p ~/tros_ws/src
cd ~/tros_ws/src

# Clone source code
git clone https://github.com/D-Robotics/hobot_tts.git
```

### Compile Package
```bash
# Enter workspace root directory
cd ~/tros_ws

# Set TROS environment
source /opt/tros/humble/setup.bash

# Compile package
colcon build --packages-select hobot_tts

# If you need to compile all packages
# colcon build
```

### Set Environment Variables
```bash
# Set compiled environment
source ~/tros_ws/install/setup.bash

# Recommend adding this command to ~/.bashrc for automatic loading
echo "source ~/tros_ws/install/setup.bash" >> ~/.bashrc
```

### Verify Compilation
```bash
# Check if package compiled successfully
ros2 pkg list | grep hobot_tts
```

## Environment Setup

To run hobot_tts, it is necessary to confirm that the audio device is set up correctly. Refer to the RDK User Manual [Audio Adapter](https://developer.horizon.cc/documents_rdk/hardware_development/rdk_x3/audio_board) section for specific setup methods.

You can use the following command to check if the settings are correct:

```bash
root@ubuntu:~# ls /dev/snd/
by-path  controlC0  pcmC0D0c  pcmC0D1p  timer
```

If an audio device such as `pcmC0D1p` appears, it means the settings are correct.

## How to Run

To run for the first time, download and extract the model file. The detailed commands are as follows:

```bash
wget http://archive.d-robotics.cc/tts-model/tts_model.tar.gz
sudo tar -xf tts_model.tar.gz -C /opt/tros/${TROS_DISTRO}/lib/hobot_tts/
```

Start the program:

```bash
source /opt/tros/setup.bash

# Suppress debug printing information
export GLOG_minloglevel=1

ros2 run hobot_tts hobot_tts
```

After successful execution, the program subscribes to the topic "/tts_text" (message type is std_msgs/msg/String) and converts it into speech signal for playback.

Note: When loading the audio driver, if the new audio device is not `pcmC0D1p`, for example `pcmC1D1p`, you need to use the `playback_device` parameter to specify the playback audio device.

## Parameter List

| Parameter Name  | Explanation              | Type         | Required | Default Value |
| --------------- | ------------------------ | ------------ | -------- | ------------- |
| topic_sub       | Subscribed text topic    | std::string  | No       | "/tts_text"   |
| playback_device | Audio playback device     | std::string  | No       | "hw:0,1"      |