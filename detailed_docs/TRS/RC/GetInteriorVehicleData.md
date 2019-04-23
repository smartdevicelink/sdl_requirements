## Functional Requirements

1.  
In case
- RC app sends valid and allowed by policies GetInteriorvehicleData(module_x) request without `subscribe` parameter  
- and SDL has cached data available for the module_x  

SDL must  
* internally subscribe RC app for requested module_x  
* respond GetInteriorVehicleData(module_x) to RC app

2.  
- RC app sends valid and allowed by policies GetInteriorvehicleData(module_x) request without `subscribe` parameter  
- and SDL has NO cached data available for the module_x

SDL must  
* apply a rate limitation scheme allowing `y` number of GetInteriorVehicleData requests to forward to HMI  
* transfer GetInteriorVehicleData(module_x) request to HMI  
* transfer GetInteriorVehicleData_response(module_x) response received from HMI to RC app

_Note:_ The number `y` shall be configurable in smartDeviceLink.ini file 

3.  
In case  
- RC app sends valid and allowed by policies GetInteriorvehicleData_request(module_x) request with `subscribe=true` parameter
- and SDL has cached module_x subscription status 

SDL must  
* internally subscribe RC app for requested "module_x"  
* respond GetInteriorVehicleData (module_x, isSubscribe=true) to RC app

4.  
In case  
- RC app sends valid and allowed by policies GetInteriorvehicleData_request(module_x) request with `subscribe=true` parameter  
- and SDL has NO cached module_x subscription status  

SDL must  
* transfer GetInteriorVehicleData(module_x, subscribe=true) request to HMI  
* create cache of module_x after receiving response from HMI  
* transfer GetInteriorVehicleData (module_x, iSsubscribed=true) response received from HMI to RC app  

5.  
In case  
- RC app is subscribed to module_1  
- and the same RC app sends GetInteriorVehicleData(module_1, subscribe=true) request  

SDL must  
* NOT transfer GetInteriorVehicleData request to HMI  
* NOT change the subscription status of the RC app  

6.  
In case  
- RC app_1 is subscribed to module_1  
- and RC app_2 sends valid and allowed by policies GetInteriorVehicleData (module_1, subscribe=true) request

SDL must  
* internally subscribe RC app_2 for requested module_1  
* NOT transfer GetInteriorVehicleData request to HMI  
* respond GetInteriorVehicleData (module_x, isSubscribe=true) to RC app_2

7.  
In case
- RC app is the only one subscribed application to module_x 
- and this app sends GetInteriorVD(module_x, subscribe = false) request  

SDL must  
* transfer GetInteriorVehicleData(module_x, subscribe=false) request to HMI  
* delete cache on receiving GetInteriorVehicleData(module_x, subscribe=false) response from HMI  
* transfer GetInteriorVehicleData(module_x, subscribe=false) response received from HMI to RC app
* unsubscribe itself from InteriorVehicleData  

8.  
In case 
- RC app_1 is subscribed to module_1  
- RC app_2 is subscribed to module_1  
- and RC app_1 sends GetInteriorVD(module_1, subscribe = false) request  

SDL must  
* internally unsubscribe RC app_1 for requested module_1  
* NOT transfer GetInteriorVehicleData request to HMI  
* respond GetInteriorVehicleData (module_x, isSubscribe=false) to RC app_1  

9.  
In case 
- RC app_1 is subscribed to module_1  
- RC app_2 is subscribed to module_2  
- and RC app_1 sends GetInteriorVD(module_1, subscribe = false) request  

SDL must  
* transfer GetInteriorVehicleData(module_1, subscribe=false) request to HMI  
* transfer GetInteriorVehicleData(module_1, subscribe=false) response received from HMI to RC app_1  

10.  
In case  
- RC app is subscribed to module_1  
- and RC app starts unregistering

SDL must  
* send GetInteriorVD(module_1, subscribe=false) request to HMI  
* transfer GetInteriorVehicleData(module_1, subscribe=false) response received from HMI to RC app  

_Note:_ SDL does not send GetInteriorVD(module_1, subscribe=false) by app unregistration in case there are some more apps still subscribed to module_1

### Resumption  

1.  
In case  
- RC app was subscribed to some vehicle data 
- and unexpected disconnect occured or SDL was stopped while app registration

SDL must 
* save application subscriptions internally
* keep application subscriptions for 3 (configured by ini file) ignition cycles

2.  
In case  
- RC app was subscribed to some vehicle data 
- and re-registers with valid `hashID` after unexpected disconnect on 4th ignition cycle  

SDL must
* clear all app-persisted data on the 4th IGNITION_ON 
* not resume persistent data  
* respond RegisterAppInterfaceResponse(SUCCESS) to mobile app

3. 
In case  
RC app with valid `hashID` re-registers after unexpected disconnect or new ignition cycle
and sends GetInteriorVehicleData(subscribe=true) request

SDL must  
* restore interior vehicle data subscriptions  
* transfer GetInteriorVehicleData(subscribe=true) request to HMI during data resumption for all necessary modules 
* respond RAI(SUCCESS) to mobile app
* update `hashID` after successful resumption  
* send OnHashChange notification with updated `hashID` to mobile app

4.  
In case  
- RC app with valid `hashID` re-registers after unexpected disconnect or new ignition cycle
- and sends GetInteriorVehicleData(subscribe=true) request
- and HMI responds with error resultCode

SDL must  
* process unsuccess response from HMI  
* respond RegisterAppInterfaceResponse(success=true,result_code=RESUME_FAILED) to mobile application  

5.  
In case
- RC app with valid `hashID` re-registers after unexpected disconnect or new ignition cycle
- and sends GetInteriorVehicleData(subscribe=true) request for all modules
- and HMI responds with error resultCode to RC.GetInteriorVD(subscribe=true, modules_1) request

SDL must
* process unsuccess response from HMI  
* remove all restored data for others modules  
* respond RegisterAppInterfaceResponse(success=true,result_code=RESUME_FAILED) to mobile application

6. 
In case  
- RC app successfully unsubscribes from vehicle data before unexpected disconnect or new ignition cycle  
by sending GetInteriorVehicleData(module_1, subscribe=false) request
- and then re-registers with valid `hashID` after unexpected disconnect or in new ignition cycle

SDL must  
* not resume the vehicle data  
* not send GetInteriorVehicleData(subscribe=true) request to HMI  
* not update `hashID`
 
 7.  
 In case  
 - RC app_1, RC app_2 are subscribed to module_1  
 - and transport disconnect occurs  
 - and RC app_1, RC app_2 with valid `hashID` re-register after unexpected disconnect  

 SDL must  
 * send GetInteriorVehicleData(subscribe=true, modules_1) request to HMI during resumption data for RC app_1  
 * restore subscription for RC app_2 internally  
 * respond RAI(SUCCESS) to mobile apps  
 * update `hashID` after successful resumption  
 * send OnHashChange notifications with updated `hashID` to mobile apps 

8.  
In case
 - RC app_1 is subscribed to module_1
 - RC app_2 is subscribed to module_2
 - and RC app_1, RC app_2 with valid `hashID` re-register after unexpected disconnect and send GetInteriorVehicleData(subscribe=true) requests 
 - HMI responds with error resultCode to RC app_1
 - and HMI responds with success to RC app_2

SDL must  
* process unsuccess response from HMI  
* respond RegisterAppInterfaceResponse(success=true,result_code=RESUME_FAILED) to RC app_1  
* respond RegisterAppInterfaceResponse(success=true,result_code=SUCCESS) to RC app_2


## Functional Requirements
#### Limitation for GetInteriorVehicleDataRequest
 When there is no cache available and before SDL forwards a GetInteriorVehicleData request to HMI, SDL shall apply a rate limitation scheme similar to that of the GetVehicleData request.  
SDL allows x number of GetVehicleData requests per second. The number x is 5 and it can be configured in smartDeviceLink.ini file.  
Similarly, SDL allows y number of GetInteriorVehicleData requests to forward to HMI. The number y shall be configurable in smartDeviceLink.ini file.  

SDL calculates amount of requests in time frame. During 100 milliseconds it can process 5 requests, but the 6th one will be rejected until the time frame ends.  

1.  
In case  
RC app has no subscriptions for module_1 (SDL must transfer GetInteriorVehicleData requests to HMI)  
and mobile app starts sending GetInteriorVehicleData(module_1, without subscribe parameter)  

SDL receives `x` number of GetInteriorVehicleData requests per `y` and starts rejecting `x`+1GetInteriorVehicleData requests with result code "REJECTED" until `y` time expires.

2.  
In case  
RC app has subscriptions for module_1 (SDL must not transfer GetInteriorVehicleData requests to HMI)  
and starts sending GetInteriorVehicleData(module_1)  

the limitation for GetInteriorVehicleData request is not applied

```
[MAIN]
; Limitation for a number of GetVehicleData requests (the 1st value) per (the 2nd value) seconds
GetVehicleDataRequest = 5, 1
+; Limitation for a number of GetInteriorVehicleDataRequest requests (the 1st value) per (the 2nd value) seconds
+GetInteriorVehicleDataRequest = 20, 1
```


#### HMI API

```xml
<function name="GetInteriorVehicleData" messagetype="request">
  <param name="moduleType" type="Common.ModuleType" mandatory="true" >
    <description>The module data to retrieve from the vehicle for that type</description>
  </param>
  <param name="subscribe" type="Boolean" mandatory="false" defvalue="false">
    <description>If subscribe is true, the head unit will send onInteriorVehicleData notifications for the module type</description>
  </param>
</function>
```

```
<function name="GetInteriorVehicleData" messagetype="response">
  <param name="moduleData" type="Common.ModuleData" mandatory="true" >
  </param>
  <param name="isSubscribed" type="Boolean" mandatory="false" >
    <description>Is a conditional-mandatory parameter: must be returned in case "subscribe" parameter was present in the related request.
    if "true" - the "moduleType" from request is successfully subscribed and  the head unit will send onInteriorVehicleData notifications for the moduleDescription.
    if "false" - the "moduleType" from request is either unsubscribed or failed to subscribe.</description>
  </param>
</function>
```

## Diagrams  
GetInteriorVehicleData
![GetInteriorVehicleData](https://github.com/smartdevicelink/sdl_requirements/blob/master/detailed_docs/accessories/GetInteriorVehicleData.png)