## Objective
Gain access to Jack's camera. What's the third item on Jack's TODO list?

## Solution

#### Start the GateXOR to get the Wireguard Client Keys
```
[Interface]
Address = 10.1.1.2/24
PrivateKey = KzOKjYql2NFLfoiaZ1S75+1FXKA58cfLdfj8AJan8L4=
ListenPort = 51820

[Peer]
PublicKey = s0wzBpdsMyNsOjM7zMqauUAUFwoNTGv44HFOHdy5F0U=
Endpoint = 34.70.170.253:51820
AllowedIPs = 10.1.1.1/3
```
``` bash
# Copy/Paste works best with gedit in this vnc container
gedit /etc/wireguard/wg0.conf
# OR
nano /etc/wireguard/wg0.conf
# OR
vim /etc/wireguard/wg0.conf
```

Then we need to take down the interface and bring it back up:

``` bash
# Bring down
wg-quick down wg0
# Bring up
wg-quick up wg0
```
Then we run the consumer test tools
```
root@cebd83b7652e:/opt/nmf/consumer-test-tool# ./consumer-test-tool.sh 
2023-12-27 00:43:54.042 esa.mo.helpertools.helpers.HelperMisc loadProperties
INFO: Loading properties file:/opt/nmf/consumer-test-tool/consumer.properties
2023-12-27 00:43:54.288 esa.mo.nmf.ctt.guis.ConsumerTestToolGUI <init>
INFO: The file with provider URIs is not present.
2023-12-27 00:43:54.656 java.util.prefs.FileSystemPreferences$1 run
INFO: Created user preferences directory.

```

#### Connect to the Directory Service

- This was already in the readme file in the zip which we downloaded to build our Docker image
```
maltcp://10.1.1.1:1024/nanosat-mo-supervisor-Directory
```
- Select the Camera option and click Connect to Selected Provider

#### Enable the Camera App
- Click on the camera and select runApp

#### Connect to Camera Directory Service and Connect to the of "Parameter" Service Provider
 
#### Enable the "generationEnabled" of "NumberOfSnapsTaken" and "Base64SnapImage" Parameter Service

#### Go the Action Tab and submit action for "Action Services" of "Base64SnapImage"

#### List the parameters which we can read
```bash
root@cebd83b7652e:/opt/nmf/cli-tool# ./cli-tool.sh parameter list -r maltcp://10.1.1.1:1025/camera-Directory
2023-12-27 14:44:26.774 esa.mo.helpertools.helpers.HelperMisc loadProperties
INFO: Loading properties file:/opt/nmf/cli-tool/provider.properties
2023-12-27 14:44:26.805 esa.mo.helpertools.helpers.HelperMisc loadProperties
INFO: Loading properties file:/opt/nmf/cli-tool/settings.properties
2023-12-27 14:44:26.806 esa.mo.helpertools.helpers.HelperMisc loadProperties
INFO: Loading properties file:/opt/nmf/cli-tool/transport.properties
2023-12-27 14:44:26.832 esa.mo.helpertools.helpers.HelperMisc loadProperties
INFO: Loading properties file:/opt/nmf/cli-tool/consumer.properties
2023-12-27 14:44:26.888 org.ccsds.moims.mo.mal.transport.MALTransportFactory newFactory
INFO: New transport factory registered with classname: esa.mo.mal.transport.tcpip.TCPIPTransportFactoryImpl
2023-12-27 14:44:26.896 org.ccsds.moims.mo.mal.encoding.MALElementStreamFactory newFactory
INFO: New encoding factory registered with classname: esa.mo.mal.encoder.binary.fixed.FixedBinaryStreamFactory
2023-12-27 14:44:26.906 esa.mo.mal.transport.tcpip.TCPIPTransport createTransportAddress
INFO: TCP/IP Transport address: 10.1.1.2:1025
2023-12-27 14:44:26.915 esa.mo.mal.transport.gen.GENTransport manageCommunicationChannel
INFO: Establishing connection to remote root URI: maltcp://10.1.1.1:1025
2023-12-27 14:44:26.918 esa.mo.mal.transport.tcpip.TCPIPConnectionPoolManager get
INFO: The socket doesn't exist! Creating a new one...
2023-12-27 14:44:27.454 esa.mo.helpertools.helpers.HelperMisc loadProperties
INFO: Reloading properties file:/opt/nmf/cli-tool/consumer.properties



Found the following parameters: 
Domain: [esa, NMF_SDK, camera]
  - NumberOfSnapsTaken
  - Base64SnapImage
  - GPS.Latitude```

```

#### Interact with the parameter subscribe option to read the base64 encoded parameter values
```bash 
root@cebd83b7652e:/opt/nmf/cli-tool# ./cli-tool.sh parameter subscribe -r maltcp://10.1.1.1:1025/camera-Directory 
2023-12-27 14:53:14.068 esa.mo.helpertools.helpers.HelperMisc loadProperties
INFO: Loading properties file:/opt/nmf/cli-tool/provider.properties
2023-12-27 14:53:14.099 esa.mo.helpertools.helpers.HelperMisc loadProperties
INFO: Loading properties file:/opt/nmf/cli-tool/settings.properties
2023-12-27 14:53:14.100 esa.mo.helpertools.helpers.HelperMisc loadProperties
INFO: Loading properties file:/opt/nmf/cli-tool/transport.properties
2023-12-27 14:53:14.126 esa.mo.helpertools.helpers.HelperMisc loadProperties
INFO: Loading properties file:/opt/nmf/cli-tool/consumer.properties
2023-12-27 14:53:14.187 org.ccsds.moims.mo.mal.transport.MALTransportFactory newFactory
INFO: New transport factory registered with classname: esa.mo.mal.transport.tcpip.TCPIPTransportFactoryImpl
2023-12-27 14:53:14.196 org.ccsds.moims.mo.mal.encoding.MALElementStreamFactory newFactory
INFO: New encoding factory registered with classname: esa.mo.mal.encoder.binary.fixed.FixedBinaryStreamFactory
2023-12-27 14:53:14.206 esa.mo.mal.transport.tcpip.TCPIPTransport createTransportAddress
INFO: TCP/IP Transport address: 10.1.1.2:1025
2023-12-27 14:53:14.216 esa.mo.mal.transport.gen.GENTransport manageCommunicationChannel
INFO: Establishing connection to remote root URI: maltcp://10.1.1.1:1025
2023-12-27 14:53:14.218 esa.mo.mal.transport.tcpip.TCPIPConnectionPoolManager get
INFO: The socket doesn't exist! Creating a new one...
2023-12-27 14:53:14.996 esa.mo.helpertools.helpers.HelperMisc loadProperties
INFO: Reloading properties file:/opt/nmf/cli-tool/consumer.properties


[1703688801918] - numberofsnapstaken: 1
[1703688803933] - base64snapimage: /9j/4AAQSkZJRgABAQAAAQABAAD/2wBDAAUDBAQEAwUEBAQFBQUGBwwIBwcHBw8LCwkMEQ8SEhEPERETFhwXExQaFRERGCEYGh0dHx8fExciJCIeJBweHx7/2wBDAQUFBQcGBw4ICA4eFBEUHh4eHh4eHh4eHh4eHh4eHh4eHh4eHh4eHh4eHh4eHh4eHh4eHh4eHh4eHh4eHh4eHh7/wAARCAgACAADAREAAhEBAxEB/8QAHQAAAAcBA
```
#### Capture the Base64 contents to file
```
./cli-tool.sh parameter subscribe -r maltcp://10.1.1.1:1025/camera-Directory  > out.txt
```

#### Transfer the file to our system

```bash
sudo docker cp cebd83b7652e:/opt/nmf/cli-tool/out.txt .
```
```bash
base64 -d out.txt.proc2 > img.proc2
```
```bash
file img.proc2               
img.proc2: JPEG image data, JFIF standard 1.01, aspect ratio, ...
```
![image](https://github.com/dibsy/sans-holiday-hack-2023/assets/1623243/60474560-0f3c-479a-b5f3-ad14718bc86e)
