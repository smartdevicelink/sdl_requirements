## Use Case 1: Subscribe to VehicleData

**Main Flow:**

_Pre-conditions:_

a. SDL amd HMI are started

b. Mobile application is registered on SDL

_Steps:_

1. Mobile application requests to subscribe to VehicleData change notifications
2. SDL validates parameters of the request
3. SDL checks if the request is allowed by Policies
4. SDL checks if requested for subscribtion parameter is stored on the list of successfully subscribed params
5. SDL transfers the request with valid and allowed parameters to HMI
6. SDL receives succussful response from HMI
7. SDL stores the param in list internally
8. SDL transfers response to mobile application
9. Any change on VehicleData happens on HMI

_Expected:_

9. HMI sends the notification to SDL with the changes in VehicleData
10. SDL transfers the notification to subscribed application


**Exception 1**

2.a.1 Request is invalid

2.a.2 SDL responds INVALID_DATA, success:false to mobile application and doesn't subscribe to vehicle data change notifications

**Exception 2**

3.a.1 Request is not allowed by Policies

3.a.2 SDL responds DISALLOWED, success:false to mobile app and doesn't subscribe to vehicle data change notifications  

**Exception 3**  
6.a.1 SDL receives erroneous result from HMI for the parameter  

6.a.2 SDL responds success:false to mobile application and doesn't store the param in list

## Use Case 2: Mobile application is already subscribed to VehicleData

**Main Flow:**

_Pre-conditions:_

a. SDL and HMI are started

b. Mobile application is registered on SDL

c. Mobile application is subscribed to single or multiple VehicleData change notification

_Steps:_

1. The same mobile application sends new request to subscribe to all VehicleData params change notification

_Expected:_

2. SDL responds with result code IGNORED, success:false to mobile application
3. SDL keeps the subscription status of the application unchanged


**Alternative flow**  

1.a.1 Mobile application_2 sends new request to subscribe to Vehicle Data (param_1, param_2) change notification  

_Expected:_  

1.a.2 SDL responds SUCCESS, success:true on subscription request of mobile application_2  

1.a.3 SDL doesn't transfer new subscription request to HMI  

1.a.4 SDL transfers stored in SDL list VehicleData parameters to mobile application_2  

