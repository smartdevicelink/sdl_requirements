## Functional Requirements

1.  
In case   
SDL is started and has one or more apps connected  
and 'LowVoltage' event occurs (battery voltage hits below the predefined threshold set by the system)

and SDL receives "LOW_VOLTAGE" UNIX signal from HMI

SDL must  

1) stop any read/write activities until getting "WAKE_UP" UNIX signal from HMI
2) ignore all the requests from mobile applications without providing any kind of response  
3) ignore all responses and messages from HMI except for "WAKE_UP" or "IGNITION_OFF" UNIX signals
4) stop audio/video streaming services  
5) resumes its regular work after receiving a "WAKE_UP" UNIX signal

2.  
In case   
app is running on SDL and has some data that can be resumed  

and SDL receives "LOW_VOLTAGE" and then "WAKE_UP" UNIX signal from HMI

SDL must  
1) Unfreeze SDL processes
2) Unregister internally on SDL all the apps
3) Wait for transport reinitiating and apps reconnection
4) Check if `hashID` provided by the app in the RAI_request matches the `hashID` stored by the system
5) Resume the application's data in case provided `hashID` matches with the stored one

3. 
In case   
app is running on SDL and has some data that can be resumed  

and SDL receives "LOW_VOLTAGE" and then "IGNITION_OFF" UNIX signal 

SDL must  
Finish its work successfully and terminated all the processes

3.  
In case   
SDL processes writing to policies database  
and HMI sends "LOW_VOLTAGE" and then "WAKE_UP" UNIX signal  

SDL must  
1) complete writing operation  
2) reflect the last known correct state of policy database

4.  
In case   
SDL processes a RPC  
and HMI sends "LOW_VOLTAGE" and then "WAKE_UP" UNIX signal  

SDL must  
1) resume its work successfully (as for Resumption)
2) discard processing of the RPC  

5.  
In case   
SDL is started with an app running in any HMI level  
'LowVoltage' event does NOT occur  
and HMI sends "WAKE_UP" UNIX signal  

SDL must  
ignore "WAKE_UP" signal and continue working as assigned

6. 
In case   
SDL is started with an app running in any HMI level  
'LowVoltage' event does NOT occur   
and HMI sends "IGNITION_OFF" UNIX signal  

SDL must  
ignore "IGNITION_OFF" signal and continue working as assigned

7.  
In case   
SDL is started with an app running in any HMI level  
and app gracefully unregisters before HMI sends "LOW_VOLTAGE" UNIX signal
and then the same app registers again after "WAKE_UP" UNIX signal  

SDL must  
1) NOT resume app data  
2) NOT resume app HMI level


## Non-Functional requirements  

UNIX signals are used to exchange shutdown and wake-up signals between HMI and SDL Core during a 'LowVoltage' event.

Unix signals provide ability to use signals from SIGRTMIN to SIGRTMAX for custom needs. 

1. SDL must use this range for handling
 `LOW_VOLTAGE`, `WAKE_UP`, `IGNITION_OFF` notifications

The offset for SIGRTMIN from this notifications is defined in SmartDeviceLink.ini.:
```
[MAIN]
LowVoltageSignal = 1 ; Offset for from SIGRTMIN
WakeUpSignal = 2 ; Offset for from SIGRTMIN
IgnitionOffSignal = 2 ; Offset for from SIGRTMIN
```

OEM manufacturer may redefine this signals or expand it for custom needs.

2. SDL must restore the applications to the same HMI level that they were in within `ResumptionDelayBeforeIgn` seconds (value defined in SmartDeviceLink.ini.) before receiving the LOW_VOLTAGE signal.

3. Applications register within `ResumptionDelayAfterIgn` seconds (value defined in SmartDeviceLink.ini.) of receipt of the WAKE_UP signal

```
[Resumption]
; Timeout in seconds to store hmi_level for media app before ign_off
ResumptionDelayBeforeIgn = 30;
; Timeout in seconds to restore hmi_level for media app after sdl run
ResumptionDelayAfterIgn = 30;
```

## Diagrams
LowVoltage

![LowVoltage](././Low_Voltage.png)





