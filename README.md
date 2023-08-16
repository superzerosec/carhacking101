# Car Hacking 101
This guide is for researcher to start a car hacking journey

Here, we will build a project, configure it and run few basic toolset

Workflow as below:

1. Creating a Virtual CAN interface, vcan0
2. Building SocketCAN userspace utilities and tools
3. Running cansniffer
4. Sending a customise CAN message
5. Sending a random generated CAN message
6. Bulding canTot - is a python-based cli framework based on sploitkit
7. Running a Unified Diagnostic Services (UDS) Server - is a ECU simulator that provides UDS support

We will using 2 terminal during this setup

## Creating a Virtual CAN interface, vcan0

**On Terminal 1**

Install dependecies on Ubuntu or Debian
```shell
sudo apt-get install libsdl2-dev libsdl2-image-dev can-utils  
```

Create vcan0 interface

```shell
sudo modprobe can
sudo modprobe vcan
sudo ip link add dev vcan0 type vcan
sudo ip link set up vcan0
```

Verifing vcan0

```shell
ifconfig vcan0 
```

if the vcan0 interface created properly, we should be able see the output with no error

## Building SocketCAN userspace utilities and tools

**On Terminal 1**

Clone and build project

```shell
cd
git clone https://github.com/linux-can/can-utils.git
cd can-utils
make
```

## Running cansniffer

**On Terminal 1**

From `can-utils` project folder, execute `cansniffer` on `vcan0`
```shell
./cansniffer vcan0
```

## Sending a customise CAN message

**On Terminal 2**

From `can-utils` project folder, execute `cansend` on `vcan0`
```shell
./cansend vcan0 5A1#11.2233.44556677.88
```

Observe stdout on terminal 1

## Sending a random generated CAN message

**On Terminal 2**

From `can-utils` project folder, execute `cangen` on `vcan0`
```shell
./cangen vcan0
```

Observe stdout on terminal 1

## Bulding canTot

**On Terminal 2**

Cloning project repo
```shell
cd 
git clone https://github.com/shipcod3/canTot.git
cd canTot
```

Intalling python virtual env and create `venv_cantot`
```shell
pip3 install virtualenv
python3 -m venv venv_cantot
source venv_cantot/bin/activate
```

Installing dependencies
```shell
pip3 install -r requirements.txt
python3 main.py 
```

Explore build in modules and use `mazda_ic_mover`

```shell
show modules
use mazda_ic_mover
show options
run
```

Observe stdout on terminal 1

## Running a Unified Diagnostic Services (UDS) Server

**On Terminal 2**

```shell
cd
git clone https://github.com/zombieCraig/uds-server.git
cd uds-server
make
```

Using provided `vintest.sh`,

```shell
./uds-server -v -V "PWN3D OP3N G4R4G3" vcan0
```

# Credit
* https://github.com/shipcod3
* https://github.com/zombieCraig
