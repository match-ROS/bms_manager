# bms_manager

## Installation
first clone the package into the raspberry pi's workspace. 

make sure that the parameters `LED_PIN` and the `BATTERY_NODE_ID` in `bms_manager_node.py` are set correctly.  

`config.txt` has to be set up like it has been explained in the CAN HAT documentation (see sources of this document).   
to bring up the CAN interface at startup create a text file at `/etc/systemd/network/80-can.network`
that has the following content inside it.


	[Match]
	Name=can0
	[CAN]
	BitRate=250K
	RestartSec=100ms


## Testing: 
To test the node:
for testing the functionality of the node the script can be executed independantly: 

	~> cd bms_manager
	~> sudo bms_manager/scripts/bms_manager.sh

## start bms_manager on start up: 
we create a systemd service to start the node on start up. 

make the bms_manager.sh file exacutable: 

	~> sudo chmod 744 scripts/bms_manager.sh

## to bring up the bms_manager at startup: 
create a text file at `/etc/systemd/system/bms_manager.service` with the following content: 


	[Unit]
	Description=BMS-manager-node manages the led strip and published bms data in ROS  
	Wants=network.target
	After=syslog.target network-online.target

	[Service]
	Type=simple
	ExecStart=/home/rosmatch/catkin_ws/src/bms_manager/scripts/bms_manager.sh

	[Install]
	WantedBy=multi-user.target


please be aware that the `ExecStart` parameter has to be set correctly to point to the `bms_manager.sh` script.  
Also make sure that the `ROS_MASTER_URI` is set correctly inside the `bms_manager.sh` and the roscore is running. 

and to start the node: 

	~> sudo systemctl daemon-reload 
	~> sudo systemctl enable bms_manager.service
	~> sudo systemctl start bms_manager.service

now the state of charge should get published on SOC ROS-topic and the led strip should light up.


## sources
For reference see: 
the CAN HAT:

	[RS485_CAN_HAT](https://www.waveshare.com/wiki/RS485_CAN_HAT)
	[RS485_CAN_HAT_(B)](https://www.waveshare.com/wiki/RS485_CAN_HAT_(B))

 

