## Functional requirement

### Establishing Session  

1.  
In case  
mobile application sends ControlFrame (V=`<app_lowest_version>`)  
and SDL responds with ControlFrame (V=`<SDL_highest_version>`)  
and then SDL receives ControlFrame (V=`<the_highest_version_for_both>`) from mobile application 

SDL must  

- respond ACK to the mobile app  
- and start session over the highest supported version of protocol for both sides (mobile app and SDL) 

2.  
In case  
mobile application sends valid ControlFrame (`<V>`, SessionID=0, StartService, ServiceType=7)   

SDL must
respond ControlFrame(`<V>`, `<SessionID>`, StartService_ACK, `<ServiceType>`) to mobile application

_Information:  
a. each new session should start 7 = RPC service firstly  
b. hashID=4 bytes (located in payload) must be provided from SDL over ControlFrame_  

3.  
In case  
mobile application requests to open a session (sends valid ControlFrame (`<V>`, SessionID=0, StartService, ServiceType=7)  

SDL must  
generate a "hashID" of 4 bytes long and send in payload via StartService_ACK message to mobile application


SDL must  

- respond ACK to the mobile app  
- and start session over the highest supported version of protocol for both sides (mobile application and SDL)  

### Ending Session  

4.  
In case 
mobile application sends EndService (ServiceType=7) with valid `<hashID>` over control service

SDL must

- respond EndService_ACK to mobile app
- close all opened services on established session with this mobile app
- close the established session with this mobile app

5.  
In case  
mobile application requests to close a session ( sends valid ControlFrame (`<V>`, SessionID, EndService, ServiceType=7)  

either without or with invalid hashID  

SDL must  
respond with EndService_NACK and keep the session with this application open  

## Diagram  

![Start session and open services](https://github.com/smartdevicelink/sdl_requirements/blob/master/detailed_docs/Protocol/assets/Start_session_and_open_services.png) 
