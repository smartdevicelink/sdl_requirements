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
The "UpdateBeforeHours" parameter defines the amount of time in hours for certificate expiration AND after expiration PTU sequence will be triggered


### Malformed messages filtering criteria  
In case  
SDL receives message from mobile app  
and this message does not match one or more of verification criteria  

SDL must  
mark such message as **malformed** 

Verification criteria:  

1. Protocol version shall be from 1 to 3
2. ServiceType shall be equal 0x0 (Control), 0x07 (RPC), 0x0A (PCM), 0x0B (Video), 0x0F (Bulk)
3. Frame type shall be 0x00 (Control), 0x01 (Single), 0x02 (First), 0x03 (Consecutive)
4. For Control frames Frame info value shall be from 0x00 to 0x06 or 0xFE(Data Ack), 0xFF(HB Ack)
5. For Single and First frames Frame info value shall be equal 0x00
6. For Control frames Data Size value shall be less than MTU header
7. For Single and Consecutive Data Size value shall be greater than 0x00 and shall be less than N (this value will be defined in .ini file)
8. Message ID be equal or greater than 0x01 for non-Control Frames
9. Data size of First Frame shall be equal 0x08 (payload of this packet will be 8 bytes).

**AppLink Core must ignore any malformed messages and no harm to the system must occur from those malformed messages**
