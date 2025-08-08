# Azure-IoT-Hub
Use Case for communication from Industrial Edge Device to Azure IoT Hub via MQTT.

- [Azure IoT Hub](#azure-iot-hub)
- [Overview](#overview)
  -  [Reference Architecture](#Reference-Architecture)
  -  [Network Architecture](#Network-Architecture)
- [Communication with Azure](Communication-with-Azure)
- [Synchronization to Azure IoT Hub](#Synchronization-to-Azure-IoT-Hub)


# Overview
The following image represents the data flow that will be achieved in this example, which covers everything from the acquisition of data from a machine in full production to its storage and processing in the cloud in general:
<img width="2306" height="1187" alt="image" src="https://github.com/user-attachments/assets/b485ae29-5fbc-4727-b01b-c3053a0fd7fc" />
# Reference-Architecture
This representation visualizes how data moves across edge applications within edge devices to the Microsoft Azure IoT Hub. The specific configurations of these applications will be detailed in this repository:

<img width="1190" height="1167" alt="image" src="https://github.com/user-attachments/assets/d2033b1b-2df0-4488-af95-6cd86065360b" />

# Network-Architecture
The following image illustrates the interaction and communication between all the devices used:

# Communication-with-Azure

To communicate with Azure IoT Hub using the MQTT protocol in IIH Essentials, the method used is called "Use the MQTT protocol directly from a device". The steps portrayed in this documentation were extracted from Use the MQTT protocol directly from a device in Azure's "Communicate with an IoT hub using the MQTT protocol" article. Additional context can be found in the article.

It is possible to connect to a device in Azure IoT Hub either using a shared access signature, i.e. username and password authentication, or using X.509 certificates, i.e. client certificates. For this documentation, a shared access signature will be used. The MQTT communication will be established using MQTT via WebSockets over port 443. Other protocols can be used as well, but please note that all device communication with IoT Hub must be secured using TLS. IoT Hub doesn't support insecure MQTT connections over port 1883.

## Synchronization-to-Azure-IoT-Hub

The generic MQTT synchronization of IIH can synchronize to any MQTT broker, including brokers on AWS or Azure
<img width="2278" height="1188" alt="image" src="https://github.com/user-attachments/assets/fd2facda-2491-4567-a1c1-bd6a5a9b43ee" />




## Command Layout

Below are examples of command-line instructions in Markdown format. These commands are used for interacting with Azure IoT Hub.

### Example Commands

#### Generate SAS Token

To generate a SAS token, use the following command:

```bash
az iot hub generate-sas-token --hub-name YourHubName --device-id YourDeviceId --duration 172000


## Documentation


you can further documentation :

- [Docs for Industrial Operations X] (https://docs.industrial-operations-x.siemens.cloud/r/en-us/v2.2.0/industrial-information-hub-essentials/working-with-data/synchronizing-data/setting-up-data-destinations/synchronizing-data-to-a-generic-mqtt-broker/connecting-to-popular-mqtt-brokers/connecting-to-azure-iot-hub)
