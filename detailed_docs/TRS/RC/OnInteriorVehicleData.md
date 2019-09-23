## Functional Requirements

1.  
In case  
application is registered with REMOTE_CONTROL appHMIType and subscribed to a module  
and HMI sends OnInteriorVehicleData notification due to data changes in the subscribed module  

SDL must  
transfer OnInteriorVehicleData notification to subscribed app  

2.
In case  
application is registered with REMOTE_CONTROL appHMIType and subscribed to a module  
and HMI sends OnInteriorVehicleData notification with `gpsData` structure with any parameters except for `longitudeDegrees`, `latitudeDegrees`, `altitude`  

SDL must  
- cut off all odd parameters
- send OnInteriorVehicleData(`longitudeDegrees`, `latitudeDegrees`, `altitude`) to mobile app

3.
In case  
application is registered with REMOTE_CONTROL appHMIType and subscribed to RADIO module  
and HMI sends OnInteriorVehicleData notification with `shifted` item in `stationLocation` parameter of `moduleData` parameter  

SDL must  
- cut off all parameters except for `longitudeDegrees`, `latitudeDegrees`, `altitude` from `stationLocation` structure and  
- send OnInteriorVehicleData notification without `shifted` parameter to mobile app

4.
In case
RC app sends is subscribed to a moduleData 
and any data change occures to this moduleData
and HMI sends OnInteriorVehicleData notification

SDL must
- transfer OnInteriorVehicleData notification (including `moduleId` for a `ModuleData`) to the subscribed RC app
- update the cache


## Non-Functional Requirements
### Mobile API

```
    <function name="OnInteriorVehicleData" functionID="OnInteriorVehicleDataID" messagetype="notification" since="4.5">
        <param name="moduleData" type="ModuleData"  mandatory="true">
        </param>
    </function>
```

### HMI API

```
<function name="OnInteriorVehicleData" messagetype="notification">
  <param name="moduleData" type="Common.ModuleData" mandatory="true" >
  </param>
</function>
```
