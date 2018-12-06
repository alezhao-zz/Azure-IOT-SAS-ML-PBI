## Build Azure IOT solution with ML and Power BI

![iot](C:\Users\alezhao\Pictures\iot.png)

**Before build solution you need prepare**

1. Azure IOT Similator

   https://docs.microsoft.com/en-us/azure/iot-hub/quickstart-send-telemetry-python

2. Global Azure Subscription with 100 USD at least

   <https://azure.microsoft.com/en-us/>

3. Dara Science Virtual Machine

   https://azure.microsoft.com/en-us/services/virtual-machines/data-science-virtual-machines/

4. Install Device Explorer

   <https://github.com/Azure/azure-iot-sdk-csharp/tree/master/tools/DeviceExplorer>

5. Power BI Online Active

   https://msit.powerbi.com/

**Step by Step setup IOT Temperature & Humidity Guide**

1. ###### Environment Prepare

   Follow up IOT HUB + Tensor Flow setup (Harry Zhu)

   https://www.azure.cn/zh-cn/blog/2018/02/06/Predict-temperature-using-Azure-IoT-development-board-and-machine-learning-algorithms

​	Install whole package below and upload Arduino sample code to AZ3166

​	https://microsoft.github.io/azure-iot-developer-kit/docs/get-started/#a-download-latest-package

2. ###### Setup Stream Analysis Job at Azure portal follow up Harry's guide where under

   Complete all of steps except 4, replace with

   *

   ```sql
   WITH machinelearning AS (*
   
   *SELECT deviceId, EventEnqueuedUtcTime, temperature, humidity, iotmlfunc(temperature, humidity) AS result from iothubinput*
   
   *)*
   
   *Select machinelearning.EventEnqueuedUtcTime, machinelearning.deviceId, CAST (result.\[temperature\] AS FLOAT) AS temperature, CAST (result.\[humidity\] AS FLOAT) AS humidity, CAST (result.\[Scored Probabilities\] AS FLOAT ) AS \'probabalities of rain\', 0 as Min, 1 as Max*
   
   *Into powerbi*
   
   *From machinelearning*
   
   *SELECT \* INTO eventhub FROM iothubinput*
   
   *SELECT deviceId, EventEnqueuedUtcTime, temperature, humidity INTO iothubblob FROM iothubinput
   ```

3. ###### Create IOT Hub as inputs at Stream Analysis Job

   Please use JSON as output datatype

4. ###### Create outputs with Event Hub, Blob and Power BI at Stream Analysis Job

5. ###### Setup Machine Learning project to get data of \[probability of rain\]

   <https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-weather-forecast-machine-learning>

   Create functions at Stream Analysis Job where connect to ML web service

6. ###### Start Stream Analysis Job and Verify

7. ###### Verify event hub message by Device Explorer Twin

8. ###### Create Power BI dashboard by output dataset

   Select dataset create in Stream Analysis Job's output

   Select temperature filed in dataset to show up

   Generate dashboard like below

9. ###### Visualize real-time sensor data from your Azure IoT hub by using the Web Apps feature of Azure App Service

   <https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-live-data-visualization-in-web-apps>

10. ###### Create Tensor Flow project by Jupiter Notebook

    <https://www.azure.cn/zh-cn/blog/2018/02/06/Predict-temperature-using-Azure-IoT-development-board-and-machine-learning-algorithms>

    Follow up Harry's guide where start at

    You could use DSVM or Azure Machine Learning Workbench

    Tips

    Please reference my code \[iothubweather.ipynb\] by VS Code

    Please setup 4900 at iteration loop

    My version include Temperature and Humidity

