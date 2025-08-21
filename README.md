# Azure-IoT-Hub
Use Case for communication from Industrial Edge Device (IED) to Azure IoT Hub via MQTT.

- [Azure IoT Hub](#azure-iot-hub)
- [Overview](#overview)
  - [Reference Architecture](#reference-architecture)
  - [Network Architecture](#network-architecture)
- [Prerequisites](#Prerequisites)
- [Communication with Azure](#communication-with-azure)
- [Synchronization to Azure IoT Hub](#synchronization-to-azure-iot-hub)
- [IoT Azure Hub Configuration](#configuration-iot-azure-hub)
- [Industrial Edge Device Configuration](#industrial-edge-device-configuration)
  - [Define Model in Data Destination Hierarchy](#define-model-in-data-destination-hierarchy)
  - [Check Messages in Azure](#Check-messages-in-azure)
- [Summary](#Summary)
- [Documentation](#documentation)

# Overview
The following image represents the data flow that will be achieved in this example, covering everything from the acquisition of data from a machine in full production to its storage and processing in the cloud:

<img width="1218" height="544" alt="image" src="https://github.com/user-attachments/assets/b2db553f-b10f-4899-894a-1b6247c5bc75" />


# Reference Architecture
This representation visualizes how data moves across edge applications within edge devices to the Microsoft Azure IoT Hub. The specific configurations of these applications are detailed in this repository:

<img width="627" height="605" alt="image" src="https://github.com/user-attachments/assets/fabb2b68-417d-4bd6-a5bc-48909264b4ab" />


# Network Architecture
The following image illustrates the interaction and communication between all the devices used:

<img width="900" height="899" alt="image" src="https://github.com/user-attachments/assets/110c5c46-4f7d-4069-aaec-ba6275a33cf2" />



# Prerequisites

- Industrial Edge Device
- IIH
    - Common Configuration V 2.2
    - IIH Essentials V 2.2
    - IIH Semantics V 2.2

> [!IMPORTANT]
> Please ensure that your Industrial Edge Device has a stable Internet connection.

# Communication with Azure

To communicate with Azure IoT Hub using the MQTT protocol in IIH Essentials, the method used is called "Use the MQTT protocol directly from a device". The steps portrayed here were extracted from Azure's "Communicate with an IoT hub using the MQTT protocol" article. Additional context can be found in the article.

[Full Documentation](https://docs.industrial-operations-x.siemens.cloud/r/en-us/v2.2.0/industrial-information-hub-essentials/working-with-data/synchronizing-data/setting-up-data-destinations/synchronizing-data-to-a-generic-mqtt-broker/connecting-to-popular-mqtt-brokers/connecting-to-azure-iot-hub)

It is possible to connect to a device in Azure IoT Hub either using a shared access signature (username and password authentication) or using X.509 certificates (client certificates). In this documentation, a shared access signature will be used. The MQTT communication will be established using MQTT via WebSockets over port 443. Other protocols can be used as well, but all device communication with IoT Hub must be secured using TLS. IoT Hub doesn't support insecure MQTT connections over port 1883.

### Checkpoint
- Communication between IED and Azure IoT Hub is possible via a secured websocket over port 443
  - Username & Password or X.509 Certificate

## Synchronization to Azure IoT Hub

The generic MQTT synchronization of IIH can synchronize to any MQTT broker, including brokers on AWS or Azure:

<img width="328" height="863" alt="image" src="https://github.com/user-attachments/assets/60e07180-f05c-44a0-82b7-eb9b51c639a4" />




## Configuration IoT Azure Hub

- If you don't have an IoT Hub Device, add a Device to your IoT Hub:


    <img width="578" height="498" alt="image" src="https://github.com/user-attachments/assets/e817e4e0-b1aa-456a-a76c-fa5601c3e3e3" />

[Further Documentation on adding IoT Device in Azure IoT Hub](https://learn.microsoft.com/en-us/azure/iot-hub/create-connect-device?tabs=portal#add-a-device)

## Industrial Edge Device Configuration

1. Setting up data destinations in the Common Configuration app:

   a. Add destination and choose Generic MQTT Broker

   <img width="757" height="659" alt="image" src="https://github.com/user-attachments/assets/c856ef3a-d670-4922-a26a-cba591322a3e" />

2. Fill out the configuration with the required information

   <img width="1001" height="897" alt="image" src="https://github.com/user-attachments/assets/d327669c-bc8d-40a2-ae3d-bffa42a7221b" />



   - Add Name for your Data Destination
   - Select **WebSocket Secure** for MQTT Protocol
   - MQTT host: your host name in Azure IoT Hub

      <img width="1688" height="384" alt="image" src="https://github.com/user-attachments/assets/9cd3c877-cc41-4052-9a82-c6bf26289fcf" />

   - MQTT Port: 443
   - MQTT path: /$iothub/websocket
   - Enter MQTT Client ID: the client ID is your Device ID from Azure IoT Hub
     
     <img width="578" height="498" alt="image" src="https://github.com/user-attachments/assets/643941f6-e48c-42c7-9ce9-4c5967d4eab0" />

   - Upload configuration file and change `{your-device-id}` with your Device ID from IoT Hub
      
      ```json
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
     - Username: built from hostname/deviceId/?api-version=2021-04-12
       For the "Username" field, use `{iotHub-hostname}/{device-id}/?api-version=2021-04-12`, where `{iotHub-hostname}` is the full CName of the IoT hub.
       
       for example `AVSHub.azure-devices.net/AlphaAA/?api-version=2021-04-12`
     - Password: the password is a SAS Token generated on https://portal.azure.com/#cloudshell/ using Azure CLI
        -  Check Microsoft Documentation for Azure CLI [How-to use the Azure CLI in a Bash scripting language](https://learn.microsoft.com/en-us/cli/azure/use-azure-cli-successfully-bash?view=azure-cli-latest), it is also possible to install Azure CLI on your host locally.


       For the "Password field", use a SAS token. The following format is used for the SAS token:
       
       `SharedAccessSignature sig={signature-string}&se={expiry}&sr={URL-encoded-resourceURI}`
       
       For generation use the following command:
       
       ```bash
       az iot hub generate-sas-token --hub-name YourHubName --device-id YourDeviceId --duration 172000
       ```
       For example:
       
       ```bash
       az iot hub generate-sas-token --hub-name Testhub --device-id TestDevice --duration 172000
       ```

       <img width="1220" height="305" alt="image" src="https://github.com/user-attachments/assets/a4c96f85-9cc3-426c-aef4-869835dff88c" />

     In case of subscription error, run the following command first:

     ```bash
     az account set --subscription yoursubscriotionID
     ```
    
     Example:
     ```bash
     az account set --subscription xxx-123456
     ```
Your Subscribtion can be foound in your IoT Hub
<img width="930" height="496" alt="image" src="https://github.com/user-attachments/assets/9144ca7c-95cb-4a0f-b45c-9d439dc234d6" />

   - Set Advanced Setting to Active
   - Set Status to Active

<img width="421" height="186" alt="image" src="https://github.com/user-attachments/assets/646d127c-a874-4e76-b6d5-ed16b6bd84ba" />

   - ## Define Model in Data Destination Hierarchy
        1. Make sure you have defined you **Model** in IIH Semantics\Manage Data\Model
            -  <img width="1617" height="1102" alt="image" src="https://github.com/user-attachments/assets/8ab91c11-d76b-47a3-b78d-d20307f9f763" />

        2. Go to your **Data Destination** in IIH Semantics\Send data\Data Destinations, on the right the **Model should appear**
            - <img width="2544" height="774" alt="image" src="https://github.com/user-attachments/assets/7d8a4767-ca8b-4943-a082-cb61efe50d8e" />
      
        3. Choose you model and import it to the **Hierarchy**, by clicking on Create
            - <img width="521" height="431" alt="image" src="https://github.com/user-attachments/assets/2e61296c-dc66-4ffa-b625-549a9c7db49e" />

        4. Your Model is now imported and the data is being sent to your **Azure IoT Hub**
            - <img width="2548" height="918" alt="image" src="https://github.com/user-attachments/assets/52fb7950-6023-468a-a73e-0ae258e3f5fe" />

        5. Alternatively, it is possible to create you own hierarchy
            - <img width="2040" height="649" alt="image" src="https://github.com/user-attachments/assets/adc297bb-72ab-434b-8454-f44de4bbeb7e" />
   - ## Check Messages in Azure
  
      ```bash
      az iot hub monitor-events --hub-name <iot-hub-name> --device-id <device-id-name>
      ```

<img width="1421" height="889" alt="image" src="https://github.com/user-attachments/assets/2a430abd-383d-456e-93e7-e2072ff069df" />



## Summary

This documentation outlines the process of establishing communication between an **Industrial Edge Device (IED)** and Azure IoT Hub using MQTT via WebSockets on port 443. The configuration involves setting up data destinations in the **Common Configuration** app and authenticating with a Shared Access Signature (SAS). The guide includes step-by-step instructions for configuring both the IoT Hub and the Edge Device, ensuring secure communication through TLS, and monitoring the messages in Azure. The document also provides necessary commands for SAS token generation and subscription management.


## Documentation
Further documentation can be found here:
[Connecting to Azure IoT Hub](https://docs.industrial-operations-x.siemens.cloud/r/en-us/v2.2.0/industrial-information-hub-essentials/working-with-data/synchronizing-data/setting-up-data-destinations/synchronizing-data-to-a-generic-mqtt-broker/connecting-to-popular-mqtt-brokers/connecting-to-azure-iot-hub)
