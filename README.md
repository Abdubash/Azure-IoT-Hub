# Azure-IoT-Hub
It is possible to connect to a device in Azure IoT Hub either using a shared access signature, i.e. username and password authentication, or using X.509 certificates, i.e. client certificates. For this documentation, a shared access signature will be used. The MQTT communication will be established using MQTT via WebSockets over port 443. Other protocols can be used as well, but please note that all device communication with IoT Hub must be secured using TLS. IoT Hub doesn't support insecure MQTT connections over port 1883.

## Command Layout

Below are examples of command-line instructions in Markdown format. These commands are used for interacting with Azure IoT Hub.

### Example Commands

#### Generate SAS Token

To generate a SAS token, use the following command:

```bash
az iot hub generate-sas-token --hub-name YourHubName --device-id YourDeviceId --duration 172000
