## Overview

![](https://iothubstorageaccts.blob.core.windows.net/pic/architect.PNG)

**Before build solution you need prepare**

1. Install VS code 

   https://code.visualstudio.com/

2. Global Azure Subscription with 100 USD at least

   <https://azure.microsoft.com/en-us/>

3. Install Device Explorer

   <https://github.com/Azure/azure-iot-sdk-csharp/tree/master/tools/DeviceExplorer>

4. Active Power BI Online 

   https://msit.powerbi.com/

**Step by Step setup IOT Temperature & Humidity Guide**

1. ###### Setup IOT hub

   - Create a new IoT Hub with price tier F1
   - Create a new IoT Device under the IoT Hub you just established
   - Copy connection string from IoT Device just built 
   - Create a consumer group which is actually prepared for stream analysis job 
   - Create an access policy that gives all permissions for easy testing. 

2. ###### Setup IOT Device Simulator 

   - Open your project folder in VS code, **Ctrl+Shift+P** type `azure sign in`  entry the code prompt and then entry [subscription account] with [password] at first time. 

   - **Ctrl+Shift+P** type `azure IOT select ` with entry to select IOT hub you created just. 

   - **Ctrl+Shift+P** type `azure IOT Create Device` entry [new device name] with Entry

   - Once created new device, it would list in **AZURE IOT HUB DEVICES**, right click your device and select  **Generate Code** select **Python** as language, save the script to your project folder. 

   - Start **Debug** the script in VS Code

     ![](https://iothubstorageaccts.blob.core.windows.net/pic/debug.PNG)

3. ###### Setup Machine Learning project to get data of \[probability of rain\]

   - Create whether forecast machine learning web service and connect to stream analysis job  

     Start from step **Deploy the weather prediction model as a web service**

     <https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-weather-forecast-machine-learning>

     ![](https://iothubstorageaccts.blob.core.windows.net/pic/studio.PNG)

   - Replace Query script with 

     ```sql
     WITH machinelearning AS (SELECT deviceId, EventEnqueuedUtcTime, temperature, humidity,iotmlfunc(temperature, humidity) AS result from iothubinput)
     Select machinelearning.EventEnqueuedUtcTime, machinelearning.deviceId, CAST (result.[temperature] AS FLOAT) AS temperature, CAST (result.[humidity] AS FLOAT) AS humidity, CAST (result.[Scored Probabilities] AS FLOAT ) AS 'probabalities of rain', 0 as Min, 1 as Max 
     Into powerbi
     From machinelearning
     
     SELECT * INTO eventhub FROM iothubinput
     
     SELECT deviceId, EventEnqueuedUtcTime, temperature, humidity INTO iothubblob FROM iothubinput
     ```

   - Use JSON as output datatype for IOT hub input

4. ###### Create outputs with Event Hub, Blob and Power BI at Stream Analysis Job

   - Create **Event Hub** at Azure portal 

   - Create **Storage Account** at Azure portal

   - Add a new **Blob Container** under **Storage Account** newly created

   - Create outputs with **Event Hub**, **Azure Blob Storage** and **Power BI** in **Stream Analysis Job**

     ![](https://iothubstorageaccts.blob.core.windows.net/pic/outputs.PNG)

5. ###### Start Stream Analysis Job and Verify

   - Verify there has stream coming into SAS by view of **Job diagram**

     ![](https://iothubstorageaccts.blob.core.windows.net/pic/job.PNG)

6. ###### Verify event hub message by Device Explorer Twin

7. ###### Create Power BI dashboard by output dataset

   - Select dataset create in Stream Analysis Job's output

     ![](https://iothubstorageaccts.blob.core.windows.net/pic/dataset.PNG)

   - Select temperature filed in dataset to show up

     ![](https://iothubstorageaccts.blob.core.windows.net/pic/temperature.PNG)

   - Generate dashboard like below

     ![](https://iothubstorageaccts.blob.core.windows.net/pic/powerbi.PNG)

8. ###### Visualize real-time sensor data from your Azure IoT hub by using the Web Apps feature of Azure App Service

   ###### <https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-live-data-visualization-in-web-apps>

   ![](https://iothubstorageaccts.blob.core.windows.net/pic/webapp.PNG)
