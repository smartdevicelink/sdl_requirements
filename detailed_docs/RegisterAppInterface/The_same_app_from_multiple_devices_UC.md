## Use case 1: Registering two applications from the same mobile device

_Pre-conditions:_  

a. SDL and HMI are started  
b. mobile device is connected via any transport  
c. mobile app_1 (appName="first_app", appID=1) is registered 

_Steps:_
1. mobile app_2 sends RAI request with 
* the same `appName` and `apppID` OR 
* the same `appID` and different `appName` 

as mobile app_1 from the same device 

_Expected:_

2. SDL validates `appName` and `appID`
3. SDL rejects the app with same app name or the same app id from one device  
4. SDL sends RegisterAppInterface (resultCode = APPLICATION_REGISTERED_ALREADY) to mobile app_2

**Alternative flow:**  
1.1 RAI request contains the same `appName` and different `appID`  
1.2 SDL sends DUPLICATE_NAME to mobile app_2

## Use case 2: Registering the same app from multiple devices

_Pre-conditions:_  

a. SDL and HMI are started  
b. mobile device_1 is connected via any transport  
c. mobile device_2 is connected via any transport  
d. mobile app_1 (appName="first_app", appID=1) is registered on mobile device_1

_Steps:_
1. mobile app_2 sends RAI with  
the same `appName` and `apppID` as mobile app_1   
from the mobile device_2

_Expected:_  
2. SDL validates `appName` and `appID`  
3. SDL assigns unique internal `appID` for mobile app_2  
4. SDL sends OnAppRegistered **(appName = appName1, appID=2)** notification to HMI  
5. SDL sends RegisterAppInterface (resultCode = SUCCESS) to app_2

**Alternative flow 1:**  
1.1.1 mobile device_2 sends RAI with the same `appName` as app_1 and different `appID`  
1.1.2 SDL sends OnAppRegistered **(appName="appName1", appID=2)** notification to HMI  
1.1.3 SDL sends RegisterAppInterface (resultCode = SUCCESS) to app_2

**Alternative flow 2:**  
1.2.1 mobile device_2 sends RAI with different from app_1 `appName` and the same `appID`  
1.2.2 SDL sends OnAppRegistered **(appName="appName2", appID=2)** notification to HMI  
1.2.3 SDL sends RegisterAppInterface (resultCode = SUCCESS) to app_2
