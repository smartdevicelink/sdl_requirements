## Use Case 1: Notification about changes to VehicleData

**Main Flow:**

_Pre-conditions:_

a. SDL and HMI are started

b. Mobile applications are registered on SDL

c. Mobile applications are subscribed on VehicleData changes notification

_Steps:_

1. Any change in VehicleData is registered on HMI (e.g. externalTemperature data value has been changed)

2. HMI sends the notification about changes to SDL

_Expected:_

3. SDL transfers the notification with updated VehicleData value to subscribed mobile application


**Exception 1**

2.1.a Received notification about changes to VehicleData from HMI is invalid

2.1.b SDL logs the error internally and ignores such notification without transfering it to any of subscribed mobile applications  

**Exception 2**

2.2.a Notification about changes to VehicleData is not allowed by Policies for mobile application

2.2.b SDL doesn't transfer the notification to such mobile application 

**Exception 3**  

2.3.a  Notification about changes to VehicleData is allowed by Policies for this mobile app  
and "parameters" field is omitted at PolicyTable  

2.3.b SDL transfers received notification with all parameters as is to mobile app  

2.3.c SDL responds with received resultCode from HMI to mobile app  

**Exception 4** 

2.4.a Notification about changes to VehicleData is allowed by Policies for this mobile app  
and "parameters" field is empty at PolicyTable   

2.4.b SDL doesn't transfer the notification to mobile application and logs corresponding error internally









