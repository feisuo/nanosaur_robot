# :sauropod: nanosaur

NanoSaur is a little tracked robot ROS2 enabled, made for an NVIDIA Jetson Nano

* Website: [nanosaur.ai](https://nanosaur.ai)
* For technical details follow [wiki](https://github.com/rnanosaur/nanosaur/wiki)
* If you need an help open an [issue](https://github.com/rnanosaur/nanosaur/issues)

## Install nanosaur system
```
git clone https://github.com/rnanosaur/nanosaur.git
sudo bash nanosaur/nanosaur_bringup/scripts/install.sh
```

After the installation please **reboot** the jetson board

## Run nanosaur

This script start the docker-compose

```
cd nanosaur/
docker-compose up -d
```

# Develop

This following part is to develop with nanosaur

##  Build Docker

```
docker build -t nanosaur .
```

## Run docker container

https://answers.ros.org/question/358453/ros2-docker-multiple-hosts/

```
docker run --net=host -v $HOME/dev_ws/src:/opt/ros_ws/src -v "/run/jtop.sock:/run/jtop.sock" --device /dev/i2c-1 -it --rm nanosaur bash
```

Debug camera

```
cd src/nanosaur
docker run --runtime nvidia -it --rm  --network host -v /tmp/argus_socket:/tmp/argus_socket -v $(pwd):/opt/ros_ws/src/nanosaur nanosaur:test bash
```

## Detect I2C devices

Install I2C tools and dectect all devices

```
sudo apt-get install -y python-smbus
sudo apt-get install -y i2c-tools
```

```
sudo i2cdetect -y -r 1
```

Devices:
* **3C** left Display
* **3D** right display
* **60** motor driver

## Camera fail

If the camera fail do:

```
sudo systemctl restart nvargus-daemon
```

And rerun the docker container