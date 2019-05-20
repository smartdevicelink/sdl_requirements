## Functional Requirements
1.
In case  
mobile app successfully registers on SDL  
and sends StartService request for a service (RPC/Audio/Video)

SDL must  
send OnServiceUpdate(`serviceType`, `REQUEST_RECEIVED`) notification to HMI

_Note: The only time when SDL would not be able provide the `appID` would be during the first StartService request for the RPC service before RAI was sent._

2.
In case  
mobile app requested StartService  
SDL has valid certificates  
and the requested service successfully started  

SDL must  
send OnServiceUpdate(`serviceType`, `REQUEST_ACCEPTED`) notification to HMI

3.  
In case  
mobile app sends StartService request  
and SDL doesn't have certificates/has expired certificate/certificate is about to expire in next 24 hours  
and the requested service is not able to start  

SDL must  
send OnServiceUpdate(`serviceType`, `REQUEST_REJECTED`)

4.
In case  
mobile app sends StartService request  
SDL sends GetSystemTime() to HMI to get a valid SystemTime for certificate authentication  
and Service is rejected because the system was unable  
HMI responds GetSystemTime(REJECTED) 

SDL must  
send OnServiceUpdate(`serviceType`, `REQUEST_REJECTED`, reason=`INVALID_TIME`) to HMI

5.  
In case  
mobile app sends StartService request  
the requested service is not able to start due to invalid/expired certificate

SDL must  
send OnServiceUpdate(`serviceType`,`REQUEST_REJECTED`, reason=`INVALID_CERT`) to HMI

6.
In case  
SDL triggers PTU  
and Policy Table Update times out  
and PTU retry attempts have been exhausted (is defined by the size of array `seconds_between_retries`in PT)

SDL must  
send OnServiceUpdate(`serviceType`, `REQUEST_REJECTED`, reason=`PTU_FAILED`) notification to HMI  
send StartServiceNACK(VIDEO) to the navi mobile app

7.
In case
PTU brings invlid/expired certificate

SDL must  
send OnServiceUpdate(`serviceType`, `REQUEST_REJECTED`, reason=`INVALID_CERT`) notification to HMI  
send StartServiceNACK(`serviceType`) to the mobile app

8.
In case
in .ini file ForceProtectedService=0x0A
and the mob app requests to start unprotected serviceType 0x0A

SDL must  
send OnServiceUpdate(`serviceType`, `REQUEST_REJECTED`) notification to HMI  
send StartServiceNACK(`serviceType`) to the mobile app

## Non-Functional requirements

### HMI_API

```xml
<function name="OnServiceUpdate" messagetype="notification">
  <description>
    Must be sent by SDL to HMI when there is an update on status of certain services.
  </description>
  <param name="serviceType" type="Common.ServiceType" mandatory="true">
    <description>Specifies the service which has been updated.</description>
  </param>
  <param name="serviceEvent" type="Common.ServiceEvent" mandatory="false">
    <description>Specifies service update event.</description>
  </param>
  <param name="reason" type="Common.ServiceStatusUpdateReason" mandatory="false">
    <description>
      The reason for a service event. Certain events may not have a reason, such as when a service is ACCEPTED (which is the normal expected behavior).
    </description>
  </param>
   <param name="appID" type="Integer" mandatory="false">
       <description>ID of the application which triggered the update.</description>
   </param>	
</function>
```

## Diagram

![OnServiceUpdate notification][OnServiceUpdate]

[OnServiceUpdate]:../accessories/OnServiceUpdate.png
