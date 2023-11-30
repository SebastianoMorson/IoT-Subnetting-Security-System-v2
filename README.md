# IoT-Subnetting-Security-System

## Tool Description
ISSS is a cybersecurity tool designed for IoT devices. The tool utilizes a machine learning model to analyze generated network traffic. Upon detecting malicious traffic, the tool identifies the actors involved in the communication and adds a firewall rule for these devices to isolate them from the network.

## Compatible Architectures
The tool is configurable on Raspberry Pi 3b+ and 4 boards.

## Necessary Dependencies
The libraries and programs required by the tool are listed in the files requirements.txt and pip_requirements.txt and are automatically installed by running the Initializer_Component.sh script. The tool also requires Python version >3.9.2, which is not automatically installed during the execution of the Initializer Component.

## Installation
To install the tool, follow these steps:
1. Download the folder using the command

```
sudo git clone https://github.com/Sebastiano-Morson/IoT-Subnetting-Security-System.git
```

2. Access the ISSS folder
3. Execute the command to install the tool

```
sudo ./Initializer_Component.sh
```

The installation phase may take several minutes, during which you will be prompted for information such as SSID name, password, and IP associated with the subnet. It also involves the installation of all dependencies required for the proper functioning of the tool, as listed in the pip_requirements.txt and requirements.txt files. Once installed, the tool will reboot. If everything has proceeded correctly, the new AP with the specified SSID set during the installation phase will be visible. There will also be a new service that will be automatically executed at each boot of the device.

By default, during installation, files are copied to the /usr/local/ directory. You can modify the installation folder by accessing the Initializer_Component.sh file and modifying the line

```
cp -r ../ISSS/ /usr/local/ISSS/
```

## Configuration

### Network Configuration
To change the SSID name and network password, it is sufficient to access the configuration file /etc/hostapd/hostapd.conf and modify lines 3 and 10.

### Capture Parameters Configuration
The Evaluation_Engine uses the NFStream library to capture packets circulating on the network. To modify capture parameters, refer to the NFStream tool documentation.

### Machine Learning Model Configuration
To change the machine learning algorithm used, search within the Evaluation_Engine.py script for the line
```
model = joblib.load("./random_forest.joblib")
```
and replace the path "./random_forest.joblib" with the path where the new model is located.

## Ban_Notifier_Engine
The Ban_Notifier_Engine represents the interface between the user and the ISSS and can be invoked through the command
```
Ban_Notifier_Engine.py [-h] [-l] [-s] [-a] [--show-granted-devices] [-d ALLERTS_DELAY] [-m MAX_ALLERTS] [-o]
                              [--lock-device LOCK_DEVICE | --unlock-device UNLOCK_DEVICE | --grant-device GRANT_DEVICE | --remove-grant REMOVE_GRANT]
```

Through the Ban_Notifier_Engine, you can view the current status of the Evaluation_Engine and Isulator_Engine.

The Ban_Notifier_Engine.py script provides various options to control and modify the tool's operation.
| Option | Description |
| :---: | :---: |
| -l, --list | show the list of devices in isolation |
| -s, --show-logs | show the list of locking and unlocking operations |
| -a, --show-allerts | show the list of devices flagged as suspicious by the Evaluation Engine |
| --show-granted-devices | show the list of authorized devices that will not be subjected to isolation measures |
| -d ALLERTS_DELAY, --allerts-delay ALLERTS_DELAY | allows modifying the time interval in which two or more negative evaluations are considered related |
| -m MAX_ALLERTS, --max-allerts MAX_ALLERTS | allows modifying the number of alerts required to ban the connection |
| -o, --show-options | show the current settings used by the Evaluation Engine |
| --lock-device LOCK_DEVICE | allows locking a device |
| --unlock-device UNLOCK_DEVICE | allows unlocking a device |
| --grant-device GRANT_DEVICE | allows authorizing a device |
| --remove-grant REMOVE_GRANT | allows removing authorization for a device |


## Note
If you want to verify that the tool works correctly, you can test each module individually.

First, stop the ISSS_RunTimeComponent.service.
```
sudo systemctl stop ISSS_RunTimeComponent.service
```
Then, open 3 shells and for each one, navigate to the installation folder (by default /usr/local/ISSS/).

In the first shell, run the Evaluation_Engine.py file.

In the second shell, run the Isulator_Engine.py file.

In the third shell, run the Ban_Notifier_Engine.py file passing the desired arguments.

If the tool works correctly, you should get a situation like the one shown in the gif.
![](https://github.com/Sebastiano-Morson/IoT-Subnetting-Security-System/blob/main/readme_folder/ezgif.com-gif-maker.gif)
