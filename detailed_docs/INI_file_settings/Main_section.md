[MAIN] 

|Parameter|Type|Example|Description|
|:---|:----|:----|:----------|
|SDLVersion|String|SDLVersion = {GIT_COMMIT}|SDL source version, represents as a git commit hash value|
|LogsEnabled|Boolean|LogsEnabled = true|SDL logging output enabled/disabled on system|
|AppConfigFolder|String|AppConfigFolder =|The path to application configuration folder where hmi_capabilities,smartDeviceLink.ini, log4cxx.properties are stored.Storage of Policy table files (preload, snapshot) is valid for OpenSource only. The default value is current working directory|
|AppStorageFolder|String|AppStorageFolder = /fs/rwdata/storage/sdl|The root folder for storing all applications folder|
|AppResourceFolder|String|AppResourceFolder =audio8bit.wav|Contains resourses. Default value is SDL working directory|
|ThreadStackSize|Integer|ThreadStackSize = 20480|ThreadStackSize used by SDL only if its required by sytem/platform, wisean empty value is defined or no such parameter exists, stack size will be PTHREAD`_`STACK_MIN(only SDL’s configurable value), which for<br>Ubuntu:<br>THREAD_STACK_MIN = 16384<br>QNX:<br>PTHREAD_STACK_MIN = 256
|MixingAudioSupported|Boolean|MixingAudioSupported = true|Defines if HMI support attenuated mode (able to mix audio sources)|
|HMICapabilities|String|HMICapabilities = hmi_capabilities.json|The filepath to the file of default HMICapabilities. In case HMI doesn’t send some capabilities to SDL, the values from the file are used by SDL|
|MaxCmdID|Int64|MaxCmdID = 2000000000|Maximum cmdId of VR command which may be registered on SDL|
|HMIHeartBeatTimeout|Integer|HMIHeartBeatTimeout = 3000|HMI's heartbeat timeout. The value specifies seconds to send  heartbeat to HMI from SDL|
|DefaultTimeout|Integer|DefaultTimeout = 10000|The response must be send to mobile application (SDL must respond after this timeout if HMI  haven’t respond to a request)|
|AppDirectoryQuota|Int64|AppDirectoryQuota = 104857600|Definines folder size in bytes limitation for any application on SDL|
|AppHMILevelNoneTimeScaleMaxRequests|Integer|AppHMILevelNoneTimeScaleMaxRequests = 0|Number of the requests allowed for the application in NONE HMI Level. If exceeded, the application will be unregistered|
|AppHMILevelNoneRequestsTimeScale|Integer|AppHMILevelNoneRequestsTimeScale = 10|Time period in which AppHMILevelNoneTimeScaleMaxRequests requests number allowed for the application in NONE HMI Level. If exceeded, the application will be unregistered|
|AppTimeScaleMaxRequests|Integer|AppTimeScaleMaxRequests = 0|Number of the requests allowed for the application in any HMI Level except of NONE. If exceeded, the application will be unregistered|
|AppRequestsTimeScale|Integer|AppRequestsTimeScale = 10|Time period in which AppTimeScaleMaxRequests  requests number allowed for the application in any HMI Level except of NONE. If exceeded, the application will be unregistered|
|PendingRequestsAmount|Integer|PendingRequestsAmount = 0|Number of the SDL pending requests allowed for an application. If exceeded, the application will be unregistered. If the value is “0”, the check will be skipped|
|HeartBeatTimeout|Integer|HeartBeatTimeout = 7000|Heart beat timeout used for protocol v3. Timeout must be specified in milliseconds. If timeout is 0 heart beat between Mobile device and SDL will be disabled|
|SupportedDiagModes|String Array=true|SupportedDiagModes = <br>0x01, 0x02, 0x03,<br>0x05, 0x06, 0x07,<br>0x09, 0x0A, 0x18,<br>0x19, 0x22, 0x3E|The list of diagnostic modes supported on a vehicle. Only only the stated values are allowed by SDL in terms of DiagnosticMessage RPC, others are rejected|
|SystemFilesPath|String|SystemFilesPath = /fs/images/ivsu`_`cache|The path to the system file directory for interoperation between SDL and System (e.g. IVSU files and others). If parameter is empty, SDL uses /tmp/fs/mp/images/ivsu_cache by default|
|UseLastState|Boolean|UseLastState = true|To restore the last TCP/IP transport state (name, applications or channels) on SDL on a new device connection and on disconnect|
|TimeTestingPort|Integer|TimeTestingPort = 8090|Port to obtain the performance information about   messages processing  on different component levels. Enabled if SDL built with TIME_TESTER flag|
|ReadDIDRequest|Integer,Array[2]=true|ReadDIDRequest = 5, 1|Limitation for a number of ReadDID requests (the 1st value) per seconds (the 2nd value)|
|GetVehicleDataRequest|Integer, Array[2]=true|GetVehicleDataRequest = 5, 1|Limitation for a number of GetVehicleData requests (the 1st value) per seconds (the 2nd value)|
|AppTransportChangeTimer|Integer|AppTransportChangeTimer = 500 ms|Defines the timeout for waiting the mobile app to reconnect between Bluetooth and USB transports change|
|AppTransportChangeTimerAddition|Integer|AppTransportChangeTimerAddition = 0ms|Defines the timeout  for waiting of every mobile app to reconnect between Bluetooth and USB transports change in case the number of connected apps is more than one|  

1. 
The **"SDLVersion"** parameter at .ini file defines the current version of SDL.  

2.  
The **"LogsEnabled"** parameter at .ini file defines the possibility of logging events:
- in case the value is "true" - events will be logged
- in case the value if "false" - logging of events will be dropped  

3.  
The **"AppConfigFolder"** parameter at .ini file defines the path to SDL configuration files ('HMI_capabilities.json', smartdevicelink.ini file, etc.)
The default path -> path to current working directory.  

4.  	
SDL must name **"AppStorageFolder"** for each app by the following template: `<app_id>_<device_id>`. 

5.  	
The **"AppResourceFolder"** parameter at .ini file defines the path to storage woth resourses (e.g. audio8bit.wav).

6.  
The **"ThreadStackSize"** parameter at .ini file defines the stack size.

7.  
The **"MixingAudioSupported"** parameter at .ini file defines whether system supports mixing of audio sources and ATTENUATED mode.  

8.  
The **"HMICapabilities"** parameter at .ini file defines the file with default capabilities.

9.  
The **"MaxCmdID"** parameter at .ini file defines the maximum number of cmdID which can be registerd on SDL.

10.   
The **"DefaultTimeout"** parameter at .ini file defines time in ms for waiting the response from HMI.
The default value is 10 000ms (10sec).

11.  	
The **"AppDirectoryQuota"** parameter at .ini file defines the available disk space in bytes for each application file handling
The default value is 104857600 bytes

12.  
The **"AppHMILevelNoneTimeScaleMaxRequests"** parameter at .ini file defines the allowed number of requests for mobile app in NONE HMI level during time period defined at "AppHMILevelNoneRequestsTimeScale".  

In case smartDeviceLink.ini file contains any of "AppHMILevelNoneTimeScaleMaxRequests= 0" or "AppHMILevelNoneRequestsTimeScale= 0" (or one of them is omitted), SDL must stop checking the amount of allowed requests per time scale.  

In case 
the number of the requests allowed for the application in NONE HMI Level is exceeded AppHMILevelNoneTimeScaleMaxRequests value in AppHMILevelNoneRequestsTimeScale a period of time,  

SDL must 
unregister the application with OnAppInterfaceUnregistered(REQUEST_WHILE_IN_NONE_HMI_LEVEL) notification and within the same ignition cycle futher registrations of the application must be failed with resutCode TOO_MANY_PENDING_REQUESTS .

13.  
The **"AppHMILevelNoneRequestsTimeScale"** parameter at .ini file defines the allowed period of time for mobile app in NONE to send amount of requests defined at **"AppHMILevelNoneTimeScaleMaxRequests"**  

In case smartDeviceLink.ini file contains any of "AppHMILevelNoneTimeScaleMaxRequests= 0" or "AppHMILevelNoneRequestsTimeScale= 0" (or one of them is omitted), SDL must stop checking the amount of allowed requests per time scale.  

In case  
the number of the requests allowed for the application in NONE HMI Level is exceeded AppHMILevelNoneTimeScaleMaxRequests value in AppHMILevelNoneRequestsTimeScale a period of time,  

SDL must  
unregister the application with OnAppInterfaceUnregistered(REQUEST_WHILE_IN_NONE_HMI_LEVEL) notification and within the same ignition cycle futher registrations of the application must be failed with resutCode TOO_MANY_PENDING_REQUESTS.

15.  

The **"AppTimeScaleMaxRequests"** param defines the allowed amount of requests for mobile app in any HMILevel except NONE during period os time defines at "AppRequestsTimeScale"  
_Note:_ AppRequestsTimeScale limitation must be applicable to RPCs, control frames and heartBeats messages.  

The **"AppTimeScaleMaxRequests"** and **"AppRequestsTimeScale"** limitation must be applicable to RPCs, control frames and heartBeats messages 
and must NOT be applicable for Video(11) and Audio(10) services.  

In case .ini file contains any of "AppTimeScaleMaxRequests = 0" or "AppRequestsTimeScale = 0" 
or one of them is omitted  
SDL must
stop checking the amount of allowed requests per time scale.  

In case smartDeviceLink.ini file contains any of "AppTimeScaleMaxRequests = 0" or "AppRequestsTimeScale = 0" (or one of them is omitted), SDL must stop checking the amount of allowed requests per time scale.
_Note:_ AppRequestsTimeScale limitation must be applicable to RPCs, control frames and heartBeats messages.  


In case  
the number of the requests allowed for the application in HMI Level other than NONE (that is: in BACKGROUND, LIMITED, FULL) is exceeded AppTimeScaleMaxRequests value in AppRequestsTimeScale a period of time,  

SDL must  
- unregister the application with OnAppInterfaceUnregistered(TOO_MANY_REQUESTS) notification  
- and send notification OnAppUnregistered (unexpectedDisconnect: false, appID) to HMI  
- and within the same ignition cycle further registrations of the application must be failed with resutCode TOO_MANY_PENDING_REQUESTS.

16.  	
The **"AppRequestsTimeScale"** param defines the allowed period of time for mobile app to send the amount of requests defined at "AppTimeScaleMaxRequests".
_Note:_ AppRequestsTimeScale limitation must be applicable to RPCs, control frames and heartBeats messages.


17.  
The **"PendingRequestsAmount"** parameter defines the allowed amount of pending requests for mobile app.

In case smartDeviceLink.ini file contains "PendingRequestsAmount = 0" (or the parameter is omitted), SDL must stop checking the amount of pending requests for the applications.

In case  
smartDeviceLink.ini file contains "PendingRequestsAmount = [value_more_than_zero]"  

SDL must  
- check the amount of pending requests  
- respond with (TOO_MANY_PENDING_REQUESTS, success:false) to mobile app in case application got a number of pending requests (which didn't got an answer on mobile side yet) more than defined at "PedingRequestsAmount"

_Important SDL Note:_ The number of pending requests is counted by SDL for all the registered applications in a whole.

18.  
	
The **"HeartBeatTimeout"** parameter defines the value in ms for sending HeartBeat signals over control service beetween mobile app and SDL. 

19.  
The **"SupportedDiagModes"** must be configurable parameter and listed as the hex values in smartDeviceLink.ini file, section MAIN.  
The hex values must be converted to "Integer" by SDL in terms of the mobile API protocol while being transfered to mobile application via supportedDiagModes parameter of RegisterAppInterface response.

_Information:_ Default values for GEN3 are the following:  
SupportedDiagModes = 0x01, 0x02, 0x03, 0x05, 0x06, 0x07, 0x09, 0x0A, 0x18, 0x19, 0x22, 0x3E

SDL must support the list of diagnostic modes defined in 'SupportedDiagModes' param of smartDeviceLink.ini file for the DiagnosticMessage RPC. Any other modes must be rejected by SDL.  

_Information:_ The current value of 'SupportedDiagModes' must be the following:  
$01, $02, $03, $05, $06, $07, $09, $0A, $18, $19, $22, $3E  

In case app sends DiagnosticMessage with different value in first element of 'messageData' array to SDL then defined in 'supportedDiagModes' param of .ini file  
SDL must respond+ with (ResultCode:REJECTED, success: false) to the app and not transfer this RPC to HMI.  

In case app sends DiagnosticMessage with same value in first element of 'messageData' array to SDL as one of defined in 'supportedDiagModes' param of .ini file,  
SDL must transfer this RPC to HMI then transfer HMI's response to this mobile app.  


20.  
The **"SystemFilePath"** parameter defines the path to system file directory.  

On getting valid PutFile request with systemFile="true" value, SDL must store the data into SystemFilesPath.

21.  
The **"UseLastState"** (Boolean) parameter at .ini file allows to restore the last transport state (e.g. name, channels, etc.) on SDL for a new device connection OR in case of re-connection.  

22.  
The **"TimeTestingPort"** parameter defines the port to test the performance of messages processing.  

23.  
The **"ReadDIDRequest"** parameter defines the allowed number of ReadDID_requests from mobile app (the first value of param) during period of time (the second value of param).  

In case  
mobile app sends ReadDID_requests to mobile app  
and the amount of requests more than defined at "ReadDIDRequest" param (first value) during period of time defined at "ReadDIDRequest" param (second value) at .ini file  

SDL must  
respond 'REJECTED, success, false' to mobile app  

24.  
The **"GetVehicleDataRequest"** parameter defines the allowed number of GetVehicleData_requests from mobile app (the first value of param) during period of time (the second value of param)  

In case  
mobile app sends GetVehicleData_requests to mobile app  
and the amount of requests more than defined at "GetVehicleDataRequest" param (first value) during period of time defined at "GetVehicleDataRequest" param (second value) at .ini file  

SDL must  
respond 'REJECTED, success, false' to mobile app  

25.  
The **"AppTransportChangeTimer"** parameter defines the timeout period SDL Core will wait for an application to re-register via alternate transport during 'transport switch'.  
If the application fails to re-register within this time, SDL Core shall inform HMI that the application has unregistered.

26.  
**AppTransportChangeTimerAddition** parameter defines the timeout for waiting of every mobile app to reconnect between Bluetooth and USB transports change in case the number of connected apps is more than one.