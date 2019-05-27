## Functional requirements

### Processig RPCs before establishing encrypted RPC service 

1.  
In case   
app is registered and activated
and RPC service 7 is started in unprotected mode
and the app sends RPC request (encryption_required=true) that needs protection per Policies

SDL must  
reject RPC request with result code `ENCRYPTION_NEEDED`

2.  
In case   
app is registered and activated
and RPC service 7 is started in unprotected mode
and the app sends RPC request (encryption_required=false) that doesn't need protection per Policies  

SDL must  
- transfer this request to HMI
- respond with `<result_code>` received from HMI to mob app

3.  
In case   
app is registered and activated
and RPC service 7 is started in unprotected mode
and HMI sends a notification

SDL must  
- transfer this notification to mob app only if it does not need protection

### Processig RPCs after establishing encrypted RPC service 

4.  
In case   
app is registered and activated
and RPC service 7 is started in protected mode  

and the app sends encrypted RPC request (encryption_required=true) that needs protection per Policies

SDL must  
- transfer this request to HMI
- respond with `<result_code>` received from HMI to mob app

5.  
In case   
app is registered and activated
and RPC service 7 is started in protected mode 

and the app sends encrypted RPC request (encryption_required=true) that doesn't need protection per Policies

SDL must  
- transfer this request to HMI
- send encrypted response received from HMI to mobile app

6.  
In case   
app is registered and activated
and RPC service 7 is started in protected mode 

and the app sends unencrypted RPC request (encryption_required=false) that doesn't need protection per Policies  

SDL must  
- transfer this request to HMI
- send unencrypted response received from HMI to mobile app


