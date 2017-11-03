## Use Case 1: 

**Main Flow:**

_Pre-conditions:_

a. SDL and HMI are started
b. Mobile application is registered and running on SDL

_Steps:_

1. Mobile application requests to get current value of VehicleData param(s)  

_Expected:_

2. SDL validates parameters of the request
3. SDL checks if the request is allowed by Policies
4. SDL transfers the request with valid and allowed parameters to HMI
5. SDL receives response from HMI
6. SDL transfers response to mobile application  

**Exception 1:**

2.1.a Request is invalid

2.1.b SDL responds INVALID_DATA, success:false to mobile application and doesn't transfer request to HMI

**Exception 2:**

3.1.a Request is not allowed by Policies

3.1.b SDL responds DISALLOWED, success:false to mobile application and doesn't transfer this request to HMI

**Exeption 3:**  
4.1.a The request contains and-or more allowed params and one and-or more NOT-allowed params by Policies  
4.1.b SDL transfer request to HMI with allowed params only  
4.1.c SDL ignores the NOT-allowed params  
4.1.d respond to mobile application transfered vehicleData values from HMI with "ResultCode: <applicable-result-code>, success: <applicable flag>" + "info" parameter listing the params disallowed by policies  

**Exeption 4:**  
4.2.a The request has "parameters" field empty in PolicyTable  
4.2.b SDL responds DISALLOWED, success:false to mobile application  

**Exeption 5:**  
4.3.a The request has "parameters" field omitted at PolicyTable  
4.3.b SDL transfers received request with all requested parameters as is to HMI  
4.3.c SDL responds with received resultCode from HMI to mobile app

