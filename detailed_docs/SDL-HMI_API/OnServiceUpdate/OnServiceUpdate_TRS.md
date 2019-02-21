## Functional Requirements
1.
In case  
mobile navi app successfully registers on SDL  
and sends StartService request for the Video Service

SDL must  
send OnServiceUpdate notification with the status of services to HMI

2.
In case  
ForceProtectedService=Non in .ini file  
and registred mobile navi app sends StartService(VIDEO, encryption=true) request to SDL

SDL must  
- transfer StartStream request to HMI  
- send OnServiceUpdate(appID, VIDEO, REQUEST_RECEIVED) notification to HMI  
and start processing all respective steps for openning secure Video Service (getting the current system time, performing a policy table update, decrypting certificates and ensuring validity of the certificates.)

_Note: The only time when SDL would not be able provide the `appID` would be during the first StartService request for the RPC service before RAI was sent._

3.
In case  
mobile navi app sends StartService(VIDEO, encryption = true) request to SDL
and SDL has valid certificates

SDL must  
send OnServiceUpdate(appID, VIDEO, REQUEST_ACCEPTED) notification to HMI

4.
In case  
mobile navi app sends StartService request for the Video Service  
and SDL doesn’t have certificates/has expired certificate/certificate is about to expire in next 24 hours

SDL must  
- send OnStatusUpdate(UPDATE_NEEDED) notification to HMI
- start PTU  
- send OnStatusUpdate(UP_TO_DATE) notification to HMI after PTU successfully completed
- validate cerificate
- send OnServiceUpdate(appID, VIDEO, REQUEST_ACCEPTED) to HMI 

5.
In case Policy Table Update times out  
and PTU retry attempts have been exhausted (is defined by the size of array `seconds_between_retries`in PT)

SDL must  
send OnServiceUpdate(VIDEO, REQUEST_REJECTED, PTU_FAILED) notification to HMI  
send StartServiceNACK(VIDEO) to the navi mobile app

6.
In case
PTU brings invlid cert/expired cert

SDL must  
send OnServiceUpdate(VIDEO,INVALID_CERT) notification to HMI  
send StartServiceNACK(VIDEO) to the navi mobile app

## Non-Functional requirements

### HMI_API

```xml
<function name="OnServiceUpdate" messagetype="notification">
  <description>
    Must be sent by SDL to HMI when there is an update on status of certain services.
    Services supported with current version: Video
  </description>
  <param name="serviceType" type="Common.ServiceType" mandatory="true">
    <description>Specifies the service which has been updated.</description>
  </param>
  <param name="serviceEvent" type="Common.ServiceEvent" mandatory="false">
    <description>Specifies service update event.</description>
  </param>
  <param name="reason" type="Common.ServiceUpdateReason" mandatory="false">
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
