## Functional requirements

### Establishing encrypted service  
1.  
In case
mobile application successfully registers on SDL  
and sends StartService (`<ServiceType>`, encrypted:true)  
and TLS/DTLS handshake is successfull

SDL must

respond ACK via StartService over control service to mobile application  
start secure `<ServiceType>` service

_Information:_  
1. Control Service provides the following features:  
1.1. TLS/DTLS handshake (uses routine implemented by OpenSSDL library)  
1.2. error handling

2.  
In case  
mobile application successfully registers on SDL  
and sends StartService (`<ServiceType>`, encrypted:true)  
and TLS/DTLS handshake is failed  

SDL must behave  
depending on `<ServiceType>`requested for encryption: 

a. in case `<ServiceType>` is a value of "ForceProtectedService" param of .ini file,  
this `<ServiceType>`must receive NACK from SDL (`<ServiceType>` must not be started as non-encrypted).  

b. in case `<ServiceType>` is not a value either of "ForceProtectedService" or "ForceUnprotectedService" params of .ini file,  
this `<ServiceType>` must receive ACK with encryption flag OFF (`<ServiceType>` must be started as non-encrypted).


### Ending of encrypted service  

3.  
In case  
mobile application sends EndService (`<encrypted_ServiceType>`) with valid `<hashID>` over control service

SDL must  
- respond EndService_ACK to mobile app  
- close all opened services on established session with this mobile application  
- close the established session with this mobile application

Information:  
a. `<hashID>` mobile application receives in case of opening session from SDL

## Diagram

StartService with the same service type

![StartService with the same service type](https://github.com/smartdevicelink/sdl_requirements/blob/master/detailed_docs/Protocol/assets/StartService_with_the_same_service_type.png)

