# opc-ua-to-thingspeak
Tiny project within the IIOT subject. Data from OPC UA server is being listened, converted into variables and written into ThingSpeak channel via NODE-RED.

Detailed description:
-IIOT subject: Industrial Internet of Things (abbreviation in my university is B-PIOT - )
-you have different variables that are stored in OPC UA server, every has different address and store different time-sensitive value. Objective is to read necessary data from server, compute four variables and write it into four different fields in ThingSpeak server. From OPC UA server you get six vital variables: actual speed(CurMachSpeed), required speed(MachSpeed), productProcessed, productDefective, StateCumulativeTime (array of numbers with modes and values), TimeSinceReset. 

You compute:
  
  quality by dividing (productProcessed-productDefective) by productProcessed, cannot be greater than "1"
  
  availability by dividing 24th slot of StateCumulativeTime array by TimeSinceReset, cannot be greater than "1"
 
  performance by dividing actual speed by required speed, cannot be greater than "1"
 
  OEE by multiplying all three previous values together, consequently cannot be greater than "1"

Right after that four values is being delivered in matching fields in thingspeak server. More detailed instructions are shown in slovak language in document "piot3" in this repository.

How to run:
Assuming that you have already installed Node-red (check by writing "node --version" in command prompt), npm (npm --version), installed node-red-contrib-thingspeak4 and node-red-contrib-opcua packages in Node-red enviroment, have basic knowledge of how servers works, how Thingspeak stores data and how to work with inject and logic blocks in Node-red.

1.Copy code from code.json and paste it via Import.

2.Change OPC UA server address; ThingSpeak WRITE-API key if needed.

3.Deploy the flow. Click on variables once(it is crucial to click on StateCumulativeTime before the TimeSinceReset, as we click it manually it can be that availability will be greater than "1" so that we are breaking condition mentioned above), together there will be six of them, logic block will compute all four variables and afterwards send it to your Thingspeak server.
