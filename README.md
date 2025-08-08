# Azure-IoT-Hub
Use Case for communication from Industrial Edge Device to Azure IoT Hub via MQTT.
<img width="2805" height="1417" alt="image" src="https://github.com/user-attachments/assets/1157268b-dcd2-4f85-8032-b908eeabb21d" />
# Overview
To communicate with Azure IoT Hub using the MQTT protocol in IIH Essentials, the method used is called "Use the MQTT protocol directly from a device". The steps portrayed in this documentation were extracted from Use the MQTT protocol directly from a device in Azure's "Communicate with an IoT hub using the MQTT protocol" article. Additional context can be found in the article.

It is possible to connect to a device in Azure IoT Hub either using a shared access signature, i.e. username and password authentication, or using X.509 certificates, i.e. client certificates. For this documentation, a shared access signature will be used. The MQTT communication will be established using MQTT via WebSockets over port 443. Other protocols can be used as well, but please note that all device communication with IoT Hub must be secured using TLS. IoT Hub doesn't support insecure MQTT connections over port 1883.

## Synchronization to Azure IoT Hub
The generic MQTT synchronization of IIH can synchronize to any MQTT broker, including brokers on AWS or Azure
<img width="2278" height="1188" alt="image" src="https://github.com/user-attachments/assets/fd2facda-2491-4567-a1c1-bd6a5a9b43ee" />




## Command Layout

Below are examples of command-line instructions in Markdown format. These commands are used for interacting with Azure IoT Hub.

### Example Commands

#### Generate SAS Token

To generate a SAS token, use the following command:

```bash
az iot hub generate-sas-token --hub-name YourHubName --device-id YourDeviceId --duration 172000
