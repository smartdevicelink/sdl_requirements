## Use case 1: Get real system time during security TLS/DTLS handshake

_Pre-conditions:_  
a. Ignition on  
b. SDL and HMI are started  
c. Mobile application successfully registers at system  

_Steps:_  
1. Application sends request to SDL to establish secure service

_Expected:_
2. HMI notifies SDL about readiness to provide system time  
3. SDL sends request to obtain current UTC time
4. SDL starts DefaultTimeout for waiting the response from HMI  
5. HMI sends valid response during timeout  
6. SDL's SecurityManager sends the certificate from "module_config" section of PolicyTable to mobile app  
7. Application validates this (module's = HeadUnit's) certificate to know whether it's signed  
8. Certificate is valid
9. Successful Handshake is performed  
10. SDL sends StartService_ACK(encryption = true) to mobile application
11. Secure Service is established  

**Alternative flow 1**  

4.a.1 HMI does not respond during timeout or sends invalid response  
4.a.2 SDL logs error internally  
4.a.3 Handshake is failed  
4.a.4 SDL sends StartService_NACK to application (in case `<service>` is a value of "ForceProtectedService" param of .ini file)
4.a.5 Secure Service is not established  

**Exception 1:**  
4.a.4.a SDL sends StartService_ACK(encryption = false) (in case `<service>` is not a value either of "ForceProtectedService" or "ForceUnprotectedService" params of .ini file)  
4.a.4.b Secure Service is not established   

**Alternative flow 2:**  
8.a.1 Certificate is not signed or expired or empty
8.a.2 PoliciesManager must start a PolicyTable Update sequence  
8.a.3 SDL receives one more time expired certificate during PTU  
8.a.4 SDL sends StartService_NACK(encryption = false) to mobile application
8.a.5 Handshake is failed  
8.a.6 Secure Service is not established  
