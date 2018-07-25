## Use Case 1: Handling LOW_VOLTAGE event signals

**Main Flow:**

_Pre-conditions:_   
a.	SDL and HMI are started  
b.	Mobile application is running on SDL in FULL/LIMITED HMI level  
c.	Mobile application has some data that can be resumed

_Steps:_

1.	The battery voltage hits below a certain predefined threshold set by the system
2.	SDL gets LOW_VOLTAGE signal from HMI

_Expected:_   
3.	SDL stops all read write activities, audio/video streaming, ignores all requests from application and responses/messages from HMI  
4.	Voltage is restored  
5.	SDL gets WAKE_UP signal from HMI  
6.	SDL starts its work successfully  
7.	Application is unregistered and device is disconnected due to LOW_VOLTAGE event  
8.	Application with the same hashID re-connects and re-registers within predefined timer  
9.	SDL resumes app's data  
10.	SDL resumes app's HMI level to prior to the LOW_VOLTAGE event state

_Exception 1:_  
1.1.a The battery does not get uncharged (no LOW_VOLTAGE event occurred)  
1.1.b SDL gets  WAKE_UP signal  from HMI  
1.1.c SDL ignores WAKE_UP signal and continue working as usual

_Exception 2:_  
1.2.a The battery does not get uncharged (no LOW_VOLTAGE event occurred)  
1.2.b SDL gets IGNITION_OFF signal from HMI  
1.2.c SDL ignores IGNITION_OFF signal and continue working as usual

_Exception 3:_  
1.3.a SDL is in progress of processing some RPC  
1.3.b SDL gets LOW_VOLTAGE signal from HMI  
1.3.c SDL gets WAKE_UP signal from HMI  
1.3.e SDL starts its work successfully  
1.3.f SDL discards processing RPC 

_Exception 4:_  
1.4.a SDL is is writing to policies database
1.4.b SDL gets LOW_VOLTAGE signal from HMI  
1.4.c SDL gets WAKE_UP signal from HMI  
1.4.e SDL and Policies Manager must keep policies database correct and working  
1.4.f Policy database must reflect the last known correct state

**Alternative flow 1**  
4.1 Voltage is NOT restored  
4.2 SDL gets IGNITION_OFF signal from HMI  
4.3 SDL finishes its work successfully

### Use Case 2: App unregisters before LOW_VOLTAGE event  
_Pre-conditions:_   
a.	SDL and HMI are started  
b.	Mobile application is running on SDL in FULL/LIMITED HMI level  
c.	Mobile application has some data that can be resumed  

_Steps:_  

1. Application unregisters gracefully   
2. SDL gets LOW_VOLTAGE signal from HMI  
3. SDL stops all read write activities, audio/video streaming, ignores all requests from application and responses/messages from HMI
4. Voltage is restored
5. SDL gets WAKE_UP signal from HMI
6. SDL starts its work successfully
7. Application with the same hashID registers again after WAKE_UP signal  

_Expected:_  

8. SDL does not resume app's data  
9. SDL does not resume app's HMI level


## Use Case 3: Resumption due to LOW_VOLTAGE event

**Main Flow:**  

_Pre-conditions:_  
a.	SDL and HMI are started  
b.	Mobile application is running on SDL in FULL HMI level  
c.	Mobile application has some data that can be resumed

_Steps:_  
1. Application gets disconnected during the time frame of 30 sec (inclusive) before HMI sends LOW_VOLTAGE signal  
2. The same application registers during 30 sec. after HMI sends WAKE_UP signal in the same ignition cycle
3. There is no application currently running in FULL HMI level 

_Expected:_  
4. SDL resumes FULL HMILevel for app  
5. SDL resumes persistent data

_Exception 1:_  
2.1 The same application registers in 35 sec after "WAKE_UP" unix signal in the same ignition cycle  
2.2 There is no application currently in FULL  
2.3 SDL does not resume FULL HMILevel for app   
2.4 SDL resumes persistent data  

_Exception 2:_  
3.1 There is application currently in FULL  
3.2 SDL does not resume FULL HMILevel for app  
3.3 SDL resumes persistent data
