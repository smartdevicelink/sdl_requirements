## [SecurityManager]  


|Parameter|Type|Example|Description|
|:---|:----|:----|:----------|
|Protocol|String|Protocol = TLSv1.2, DTLSv1.0.|Supported protocol version TLSv1.2, DTLSv1.0.|
|ForceProtectedService|String|ForceProtectedService = Non|Force protected services (could be id's from 0x01 to 0xFF or “Non” value)|
|ForceUnprotectedService|String|ForceUnprotectedService = Non|Force unprotected services (could be id's from 0x01 to 0xFF or “Non” value)
|UpdateBeforeHours|Integer|UpdateBeforeHours = 24|The "UpdateBeforeHours" parameter defines the amount of time in hours for certificate expiration AND after expiration PTU sequence will be triggered|  

### Functional requirements 

1.  
The "Protocol" parameter at [.ini file](https://github.com/smartdevicelink/sdl_core/blob/develop/src/appMain/smartDeviceLink.ini) of [Security Manager](https://github.com/smartdevicelink/sdl_core/blob/develop/src/appMain/smartDeviceLink.ini#L155) section defines the version of encryption protocol and must be used for:  
a) protected service handshake  
b) encryption  
c) decryption by SDL  

SDL must  

support the following version of "Protocol":  
-> TLSv1.2  
-> DTLSv1.0  

2.  
In case  
the value of "Protocol" param is DTLSv1.0 at [Security Manager] section of .ini file  
and mobile application successfully opens encrypted service (TLS handshake was succesfull)  
and at least one of encrypted packet is **malformed** due to any reason  

SDL must  
- ignore this malformed packet  
- search for the next valid header  
- continue processing of the next valid encrypted packet  

3.  
The "ForceProtectedService" parameter defines services which cannot be started as unprotected

_Info: Service type 0x07 (RPC service) cannot be the value of "ForceprotectedService"_  

4.  
The "ForceUnprotectedService" parameter defines services which cannot be started as protected OR delayed protected  

5.  
The "UpdateBeforeHours" parameter defines the amount of time in hours for certificate expiration AND after expiration PTU sequence will be triggered.
