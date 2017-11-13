## Use Case 1: Unsubscribe from VehicleData

**Main Flow:**

_Pre-conditions:_

a. SDL and HMI are started

b. Mobile application is registered on SDL

c. Mobile application is subscribed to VehicleData

_Steps:_

1. Mobile application requests to unsubscribe from VehicleData changes notifications
2. SDL validates parameters of the request
3. SDL checks if the request is allowed by Policies  
4. SDL transfers the request with valid and allowed parameters to HMI
5. SDL receives response from HMI
6. SDL stores the subscription status of the application internally
7. SDL transfers response to mobile app
8. Any change on VehicleData happens on HMI

_Expected:_

9. HMI sends the notification to SDL with the changes in VehicleData
10. SDL does not transfer the notification to unsubscribed application

**Alternative flow 1:** In case there are more than one applications subscribed to VehicleData change notifications

4.a.1 SDL does not transfer the request to HMI

4.a.2 SDL unsubscribes the requesting application from Vehicle Data change notifications

4.a.3 SDL keeps all other applications subscription status unchanged

4.a.4 Any change on VehicleData happens on HMI

_Expected:_

4.a.5 HMI sends the notification to SDL with the changes in VehicleData

4.a.6 SDL transfers the notification to subscribed applications

4.a.7 SDL does not transfer the notification to unsubscribed application

**Exception 1**

2.1.a Request is invalid

2.1.b SDL responds INVALID_DATA, success:false to mobile application and doesn't unsubscribe from VehicleData change notifications

**Exception 2**

3.1.a Request is not allowed by Policies

3.1.b SDL responds DISALLOWED, success:false to mobile app and doesn't unsubscribe from VehicleData change notifications


**Exception 3**

5.1.a HMI did not respond during default timeout

5.1.b SDL responds GENERIC_ERROR, success:false to mobile application and does not unsubscribe from VehicleData changes notifications


## Use Case 2: Mobile application is already unsubscribed from VehicleData

**Main Flow:**

_Pre-conditions:_

a. SDL amd HMI are started

b. Mobile application is registered on SDL

c. Mobile application is unsubscribed from VehicleData changes notification

_Steps:_

1. The same mobile application sends new request to unsubscribe from VehicleData changes notification

_Expected:_

2. SDL responds with result code IGNORED, success:false to mobile application  

3. SDL keeps the subscription status of the application unchanged (application remains unsubscribed)




