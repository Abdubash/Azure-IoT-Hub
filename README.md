# Azure-IoT-Hub
Use Case for communication from Industrial Edge Device (IED) to Azure IoT Hub via MQTT.

- [Azure IoT Hub](#azure-iot-hub)
- [Overview](#overview)
  -  [Reference Architecture](#Reference-Architecture)
  -  [Network Architecture](#Network-Architecture)
- [Communication with Azure](Communication-with-Azure)
- [Synchronization to Azure IoT Hub](#Synchronization-to-Azure-IoT-Hub)
- [IoT Azure Hub Configuiration](#Configuration-IoT-Azure-Hub)
- [Industrial Edge Device Configuration](#Industiral-Edge-Device-Configuration)
- [Documentation](#Documentaion)



# Overview
The following image represents the data flow that will be achieved in this example, which covers everything from the acquisition of data from a machine in full production to its storage and processing in the cloud in general:

<img width="2306" height="1187" alt="image" src="https://github.com/user-attachments/assets/b485ae29-5fbc-4727-b01b-c3053a0fd7fc" />

# Reference-Architecture
This representation visualizes how data moves across edge applications within edge devices to the Microsoft Azure IoT Hub. The specific configurations of these applications will be detailed in this repository:

<img width="1190" height="1167" alt="image" src="https://github.com/user-attachments/assets/d2033b1b-2df0-4488-af95-6cd86065360b" />

# Network-Architecture
The following image illustrates the interaction and communication between all the devices used:

<img width="1354" height="1351" alt="image" src="https://github.com/user-attachments/assets/d47fb500-364b-424c-984e-0003f3065c2b" />


# Communication-with-Azure

To communicate with Azure IoT Hub using the MQTT protocol in IIH Essentials, the method used is called "Use the MQTT protocol directly from a device". The steps portrayed in this documentation were extracted from Use the MQTT protocol directly from a device in Azure's "Communicate with an IoT hub using the MQTT protocol" article. Additional context can be found in the article.

It is possible to connect to a device in Azure IoT Hub either using a shared access signature, i.e. username and password authentication, or using X.509 certificates, i.e. client certificates. For this documentation, a shared access signature will be used. The MQTT communication will be established using MQTT via WebSockets over port 443. Other protocols can be used as well, but please note that all device communication with IoT Hub must be secured using TLS. IoT Hub doesn't support insecure MQTT connections over port 1883.


### Checkpoint
- A commomunication between IED and Azure IoT Hub is possible via secured websocket over 443 port
  - Username \& Password or X.509 Certificate

## Synchronization-to-Azure-IoT-Hub

The generic MQTT synchronization of IIH can synchronize to any MQTT broker, including brokers on AWS or Azure

<img width="2278" height="1188" alt="image" src="https://github.com/user-attachments/assets/fd2facda-2491-4567-a1c1-bd6a5a9b43ee" />

## Configuration-IoT-Azure-Hub

- Add Device to your IoT Hub
    <img width="578" height="498" alt="image" src="https://github.com/user-attachments/assets/e817e4e0-b1aa-456a-a76c-fa5601c3e3e3" />


## Industiral-Edge-Device-Configuration

1. Setting up data destinations in Common Configuration app
   a. Add destiniation and choose Generic MQTT Broker

   <img width="757" height="659" alt="image" src="https://github.com/user-attachments/assets/c856ef3a-d670-4922-a26a-cba591322a3e" />

2. Fill out the configuration with the required information

   - Add Name for your Data Destination
   - MQTT host: is your host name in Azure IoT Hub

      <img width="1688" height="384" alt="image" src="https://github.com/user-attachments/assets/9cd3c877-cc41-4052-9a82-c6bf26289fcf" />

   - MQTT Port: 443
   - MQTT path: /$iothub/websocket
   - Enter MQTT Client ID: the client ID is you Device ID from Azure Iot Hub
     
     <img width="578" height="498" alt="image" src="https://github.com/user-attachments/assets/643941f6-e48c-42c7-9ce9-4c5967d4eab0" />

   - upload configuration file and change {your-device-id} with your Device ID from IoT Hub
			```
  		{
		    "topic": "devices/{your-device-id}/messages/events/",
		    "payload": {
		        "Topic": "$topic",
		        "Name": "$attributeName",
		        "Hierarchy": "$path",
		        "IED_Model": "IED 427 Factory 1",
		        "datapoints": {
		            "Measurement": "$value",
		            "Time": "$timestamp:ISO8601",
		            "Quality_Code": "$qualityCode"
		        }
		    }
      }
         ```
   - MQTT authentication method: Use credentials
     	- Username: is build of hostname/deviceId/?api-version=2021-04-12
     	   For the "Username" field, use {iotHub-hostname}/{device-id}/?api-version=2021-04-12, where {iotHub-hostname} is the full CName of the IoT hub.
					
					for example AVSHub.azure-devices.net/AlphaAA/?api-version=2021-04-12
     	- Password: the password is SAS Token generated on https://portal.azure.com/#cloudshell/

		For the "Password field", use a SAS token. The following snippet shows the format of the SAS token:
		
		SharedAccessSignature sig={signature-string}&se={expiry}&sr={URL-encoded-resourceURI}
		
		for generation use the following command:
		```
		'az iot hub generate-sas-token --hub-name YourHubName --device-id YourDeviceId --duration 172000'
 	    ```
		for example
		'az iot hub generate-sas-token --hub-name AVSHub --device-id Abdul_Test --duration 172000'
		
		<img width="1220" height="305" alt="image" src="https://github.com/user-attachments/assets/a4c96f85-9cc3-426c-aef4-869835dff88c" />

     	 In case of subscription error run the following command first
  
     	 ```
		 az account set --subscription yoursubscriotionID
	  	 ```
    
		'az account set --subscription msa-000934'


		<img width="930" height="496" alt="image" src="https://github.com/user-attachments/assets/9144ca7c-95cb-4a0f-b45c-9d439dc234d6" />


					


   - Set Advances Setting to Active
   - Set Status to Active

		<img width="421" height="186" alt="image" src="https://github.com/user-attachments/assets/646d127c-a874-4e76-b6d5-ed16b6bd84ba" />

   - Check Messages in Azure
  
			  ```
			  az iot hub monitor-events --hub-name <iot-hub-name> --device-id <device-id-name>
			  ```
     
			<img width="991" height="496" alt="image" src="https://github.com/user-attachments/assets/207e01cb-3122-47a1-ab23-f855e29f296d" />



   - asd


## Documentation
you can further documentation :
	
	- [Docs for Industrial Operations X] (https://docs.industrial-operations-x.siemens.cloud/r/en-us/v2.2.0/industrial-information-hub-essentials/working-with-data/synchronizing-data/setting-up-data-destinations/synchronizing-data-to-a-generic-mqtt-broker/connecting-to-popular-mqtt-brokers/connecting-to-azure-iot-hub)
