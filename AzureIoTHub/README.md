# Azure IoT Setup Example

## Prerequisites
* Azure Account
* Raspberry Pi
* Linux or Putty
* Visual Studio or equivalent
* Sql Server or equivalent
## 1. 
### 1.1 Setup Raspberry PI (Producer)
#### Guides:
* [Setup Raspberry Pi](https://www.raspberrypi.org/help/quick-start-guide/2/)
* [Setup Raspbian Stretch Lite](https://www.raspberrypi.org/documentation/installation/installing-images/)
* [Security Raspbian](https://www.raspberrypi.org/documentation/configuration/security.md)
* [Setup WLAN Raspbian](https://www.raspberrypi.org/documentation/configuration/wireless/wireless-cli.md)

### 1.2 Install Azure IoT SDK (Collection)
The Azure IoT SDK is developed to use MQTT protocol.
On Raspberry Pi open remote console (via SSH or Putty for windows) and install pip
```
sudo apt-get install python3-pip
```
or
```
sudo apt-get install python-pip
```
Depending on the Python version ```python --version```.

Install Azure IoT device Client
```
pip install azure-iothub-device-client
```
If running problems installing the sdk refer to this [documentation](https://github.com/Azure/azure-iot-sdk-python/blob/master/doc/python-devbox-setup.md#installs-needed-to-compile-the-sdks-for-python-from-souce-code).


## 2. 
### 2.1 Registering Device and Sending Device Data (Ingestion)

Login to your Azure account and run the following commands to setup you cloud IoT Hub.
Install extension for troubleshooting and monitor events.
```
az extension add --name azure-cli-iot-ext
```
Create your IoT hub and register your Raspberry Pi under the IoT Hub.
```
az iot hub device-identity create --hub-name <YourHubName> --device-id <YourDeviceName>
```
On your raspberry pi, navigate to the root directory of the sample Python project. Then navigate to the iot-hub\Quickstarts\simulated-device directory 
Edit the ```SimulatedDevice.py``` script and edit the ```CONNECTION_STRING``` with the strig provided by the following command:
```
az iot hub device-identity show-connection-string --hub-name <YourHubName> --device-id <YourDeviceName> --output table
```
```
python SimulatedDevice.py
```
Notes: Depending on your installation, you might need to either install the azure-iothub-device-client on the simulated device directory or the copy the iothub_client.so generated during the installation in 1.2 to the simulated device directory.

At this point on your raspberry Pi you will have your data being generated and sent to the IoT Hub. Run the following command on the Azure bash console to confirm that data is being received. 
```
az iot hub monitor-events --device-id <YouDeviceName> --hub-name <YourHubName>
```
Please refer to the following for more information.
##### Guides:
* [Quickstart Send Telemetry](https://docs.microsoft.com/en-us/azure/iot-hub/quickstart-send-telemetry-python)

## 3. Create Event on IoT Hub and set the Stream Analytics Job (Stream Processing)

### 3.1  Create Analytics Job on Azure IoT

![Scheme  of Stream Analytics](https://github.com/Bertolt/Azure_IoT_Tutorials/blob/master/AzureIoTHub/raspi_in_out.png)
#### 3.1.2 Azure Portal
* [Create a Stream Analytics job by using the Azure portal](https://docs.microsoft.com/en-us/azure/stream-analytics/stream-analytics-quick-create-portal)
#### 3.1.3 Azure Power Shell
* [Create a Stream Analytics job by using Azure PowerShell](https://docs.microsoft.com/en-us/azure/stream-analytics/stream-analytics-quick-create-powershell)
#### 3.1.4 Azure Visual Studio
* [Create a Stream Analytics job by using the Azure Stream Analytics tools for Visual Studio](https://docs.microsoft.com/en-us/azure/stream-analytics/stream-analytics-quick-create-vs)

## 4. Create Storage in Azure (Storage)
### 4.1 Blob
* [Upload, download, and list blobs with the Azure portal](https://docs.microsoft.com/en-us/azure/storage/blobs/storage-quickstart-blobs-portal)

For Blob management there is this nice SDK for Python.

* [Upload, download, and list blobs using Python](https://docs.microsoft.com/en-us/azure/storage/blobs/storage-quickstart-blobs-python)
### 4.2 SQL Database
* [Create an Azure SQL database in the Azure portal](https://docs.microsoft.com/en-us/azure/sql-database/sql-database-get-started-portal)

Don't forget to open firewall for your raspberry pi and VS.

* [Create a server-level firewall rule for your SQL database](https://docs.microsoft.com/en-us/azure/sql-database/sql-database-get-started-portal-firewall)
## 5. More Info
For more detailed info visit: 
* [Microsoft Azure Github](https://github.com/Azure)
* [Micorsoft Azure Docs](https://docs.microsoft.com/pt-pt/azure/)
* [Azure IoT SDKs](https://github.com/Azure/azure-iot-sdks)
