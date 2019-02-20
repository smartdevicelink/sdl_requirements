
## Use Case 1: Notifying HMI with OnServiceUpdate about status of services for secure video streaming

**Main Flow:** 

_Pre-conditions:_

a. SDL and HMI are started

b. Mobile navigation application is registered on SDL

_Steps:_

1. Mobile application requests to stream VIDEO data securely

_Expected:_  
2. SDL sends OnServiceUpdate(VIDEO, REQUEST_RECEIVED) notification to inform HMI about received request
3. A pop-up with the system status appers on HMI (e.g. _"Start video stream"_)
4. SDL sends GetSystemTime request to get real UTC time for certificate validation  
5. HMI sends GetSystemTime response with real UTC time to SDL  
6. SDL validates certificate, certificate is valid  
7. SDL sends a request to decrypt the certificate to HMI (per EXTERNAL_PROPRIETARY flow) 
8. SDL receives a successful response with decrypted certificate from HMI  
9. SDL sends OnServiceUpdate(VIDEO, REQUEST_ACCEPTED) notification to HMI
10. A pop-up with the system status disappers on HMI  
11. SDL sends StartServiceACK to mobile navigation application  
12. Mobile navigation application streams video data

**Exception 1:**  
5.1 HMI doesn’t send GetSystemTime response or responds with any `<errorCode>` or sends invalid response  
5.2 SDL sends OnServiceUpdate(VIDEO, INVALID_TIME) notification to HMI  
5.3 A pop-up on HMI informs the user of the system status or further steps to take (e.g. _"Unable to get valid time. Make sure your device has access to GPS signal."_)  
5.4 SDL sends StartServiceNACK to the mobile navigation application  
5.5 Mobile navigation application does not stream video data


## Use Case 2: Notifying HMI with OnServiceUpdate about fails during opening secure video service

**Main Flow:**

_Pre-conditions:_

a. SDL and HMI are started

b. Mobile navigation application is registered on SDL

_Steps:_

1. Mobile application requests to stream VIDEO data securely  
2. SDL sends OnServiceUpdate(VIDEO, REQUEST_RECEIVED) notification to inform HMI about received request  
3. A pop-up with the system status appers on HMI (e.g. _"Start video stream"_)  
4. SDL sends GetSystemTime request to get real UTC time for certificate validation  
5. HMI sends GetSystemTime response with real UTC time to SDL  
6. SDL validates certificate, certificate is invalid  

_Expected:_  
7. SDL sends OnStatusUpdate(UPDATE_NEEDED) notification to inform HMI that PTU is needed  
8. SDL triggers PTU  
9. PTU is completed  
10. PTU brings valid cert
11. SDL sends OnStatusUpdate(UP_TO_DATE) notification to HMI  
12. SDL sends OnServiceUpdate(VIDEO, REQUEST_ACCEPTED) notification to HMI  
13. A pop-up with the system status disappers on HMI  
14. SDL sends StartServiceACK to mob app  
15. Mobile navigation application streams video data

**Exception 1:**  
9.1 PTU times out; 5 PTU retry sequences are unsuccessful  
9.2 SDL sends OnServiceUpdate notification to inform HMI about failed PTU  
9.3 A pop-up with the system status appers on HMI (e.g. _"Unable to update apps. Make sure your device has an internet connection."_)  
9.4 SDL sends StartServiceNACK to mob app  
9.5 Mobile navigation application does not stream video data  

**Exception 2:**  
10.1 PTU brings invalid cert/expired cert  
10.2 SDL sends OnServiceUpdate notification to inform HMI that cert is invalid  
10.3 A pop-up with the system status appers on HMI (e.g. _"System is unable to authenticate the app."_)  
10.4 SDL sends StartServiceNACK to mob app  
10.5 Mobile navigation application does not stream video data
