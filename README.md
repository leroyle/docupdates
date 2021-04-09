
### 1. A word about join failures

The process of joining the Helium network can seem to be fickle at times. There are several factors to keep in mind
##### Credentials
Helium only supports OTAA joins at this time. This means your edge node device must have provisioned with the application the proper LoRaWan credentials that "must" match those seen within the device configuration within the Console. These includes
```
- Device EUI
- App EUI
- App Key
```

These "must" be added to your device sketch in the proper format and the proper Endian-ness, which can vary from one edge node device LoRaWan runtime implementation to the next.

##### Distance to Nearest Hotspot
While LoRaWan is advertised as a long distance communications protocol there are many factors that can limit connectivity. The primary being unubstructed line of sight. Trees, buildings, mountains can limit the reach of your devices.
If there is any distance at all between your edge node device and the nearest hot spot and you are having trouble joining the network. It is suggested you take a field trip to get closer to the target hot spot to see if connection can be made when in close proximity.
Most runtime implementations will vary the data rate used for joining to try to compenstate for hot spot distance. The specific algorithm is highly implementation dependent.
 
 ##### Edge node device LoRaWan runtime join retry implementation
 Each runtime implementation of the LoRaWan specification is going to be different. Some runtimes will continuously try to join the network until power is removed. Some may provide an API or global variable which determines the number of join attempts to make before either pausing before retrying, aborting the join retry with notification sent to the device application via a callback mechanism, or aborting retry without notifying the device application (which is really not acceptable).
 

### 2. Helium Console Activities

Now that basic communications between the edge node device and the Helium network has been confirmed there are a few things one can do within the Helium Console product that you should be aware of when implementing your own custom device application.


#### Console Decoder Function

The LoRaWan specification encourages minimal over the air data packet transmissions. It is 
expected that the edge node device will implement a data packing scheme so as to reduce the overall size of the data packet that is transmitted over the air.
However, the compacted data would then need to be converted back to a more useable form at some point before it can be processed by the cloud integration server. This re-exapansion or conversion can be done as the data passes through the Helium Console or once received by the cloud integration server.



If one chooses to perform the re-expansion via the Helium Console functionality this is accomplished by implementing a Console decoder function written in javascript. Refer to the Helium documentation here https://docs.helium.com/use-the-network/console/functions/#decoder-function-output  for details concerning implementing this decoder function.

The supplied Helium sample device applications may include a very simple decoding javascript function as the sample itself sends very simple data. There is a repository of commonly used decoders that one can either use as is or use as a reference when developing a custom decoding scheme. The repository is community provide so feel free to contribute your custom decoder.
The repository can be found at: https://github.com/helium/console-decoders.

NOTE: The user data packet is encoded using a base64 format when being sent over the air. The data presented to the Console decoder function has been base64 decoded for you. If one waits and decodes at the cloud integration server that packet data is still base64 encoded thus you will need to base64 decode the packet  before re-expansion at the server.


#### Helium Console device debug view

Often it is helpful to be able to visually see the data flows that occur between the edge node device and the cloud integration server. The Helium Console provides a method for viewing  packet data as well as other associated meta-data as the packets flow through the Console. Due to security considerations the 
data is not automatically stored, nor is the presentation of the data started without a specific user action.
The usage of the Console debug view is documented here: https://docs.helium.com/use-the-network/console/debug/
The debug view can be very helpful when trying to diagnose communications issues.

  
  
  #### Network Data Flow
  The following diagram illustrates one possible communication flow from an edge node device, through the Helium Console and on to the integration server.
  Note: the ordering of the data flows as seen within the Console device debug view may not eactly match those seen in the illustration below.
  
  ![](./flow.png)

  
  ## Editor Note:
  We should add the flow for confirmed messages, downlinks, ADR 
  
