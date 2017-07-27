## Functional Requirements

1.
In case 
- RC app sends valid and allowed by policies GetInteriorvehicleData_request with "subscribe" parameter
- and SDL received GetInteriorVehicledata_response with resultCode: <"any_not_erroneous_result"> and without "isSubscribed" parameter from HMI 

SDL must

transfer GetInteriorVehicleData_response  with resultCode:<"any_not_erroneous_result"> and with added isSubscribed: <"current_subscription_status"> to the related app

Info: In case in HMI response the subscription status was not signed or changed then SDL just transfers received result code with current subscription status without changing it

2.
In case 

- RC app sends valid and allowed-by-policies GetInteriorVehicleData_request without "subscribe" parameter 
- and SDL gets GetInteriorVehicleData_response with resultCode: <"any-result"> and with "isSubscribed" param from HMI 

SDL must 

transfer GetInteriorVehicleData_response with resultCode: <"any-result"> and without "isSubscribed" param to the related app. 

Information: SDL ignores HMI's subscription status (since the app does not request it), but just transfers HMI's response with removed "isSubscribed" param to the app. 

3.
In case 

- RC app sends valid and allowed by policies GetInteriorvehicleData_request 
- and SDL received GetInteriorVehicledata_response with successful result code and current module data from HMI

SDL must

transfer GetInteriorVehicleData_response  with  provided from HMI current module data for allowed module and control items

4. 
In case 

- RC app sends valid and allowed-by-policies GetInteriorVehicleData_request with "subscribe:true" parameter 
- and SDL received GetInteriorVehicleData_response with "isSubscribed: true", "resultCode: SUCCESS" from HMI 

SDL must 

- internally subscribe this application for requested <"moduleType_value">
- transfer GetInteriorVehicleData_response with "isSubscribed: true", "resultCode: SUCCESS", "success:true" to the related app. 

5. 
In case 

- RC app sends valid and allowed-by-policies GetInteriorVehicleData_request with "subscribe:true" parameter 
- and SDL received GetInteriorVehicleData_response with "isSubscribed: false", "resultCode: SUCCESS" from HMI 

SDL must 

transfer GetInteriorVehicleData_response with "isSubscribed: false", "resultCode: SUCCESS", "success:true" to the related app. 

Information: SDL does not subscribe the app in this case, but just transfers HMI's response to the app. 

6.
In case 

- RC app sends valid and allowed-by-policies GetInteriorVehicleData_request with "subscribe:true" parameter 
- and SDL gets GetInteriorVehicleData_response without "isSubscribed" parameter and with "resultCode: SUCCESS" from HMI

SDL must 

transfer GetInteriorVehicleData_response with "isSubscribed: false", "resultCode: SUCCESS", "success:true" to the related app. 

Information: SDL does not subscribe the app in this case, but just transfers HMI's response with added "isSubscribed" param to the app.

7.
In case 

- RC app sends valid and allowed-by-policies GetInteriorVehicleData_request with "subscribe:true" parameter 
- and SDL received GetInteriorVehicleData_response with resultCode: <"any-erroneous-result"> from HMI 

SDL must 

transfer GetInteriorVehicleData_response with resultCode: <"any-erroneous-result"> and without "isSubscribed" param to the related app. 

Information: SDL does not subscribe the app in this case, but just transfers HMI's response with removed "isSubscribed" param to the app. 

8.
In case 

- RC app is subscribed to <"moduleType_value"> 
- and sends valid and allowed-by-policies GetInteriorVehicleData_request with "subscribe:false" parameter 
- and SDL gets GetInteriorVehicleData_response with "isSubscribed: false", "resultCode: SUCCESS" from HMI 

SDL must 

- internally un-subscribe this application for requested <"moduleType_value">
- transfer GetInteriorVehicleData_response with "isSubscribed: false", "resultCode: SUCCESS", "success:true" to the related app.

9.
In case 
- RC app is subscribed to <"moduleType_value"> 
- and sends valid and allowed-by-policies GetInteriorVehicleData_request with "subscribe:false" parameter 
- and SDL gets GetInteriorVehicleData_response with "isSubscribed: true", "resultCode: SUCCESS" from HMI 

SDL must 

transfer GetInteriorVehicleData_response with "isSubscribed: true", "resultCode: SUCCESS", "success:true" to the related app. 

Information: SDL does not un-subscribe the app in this case, but just transfers HMI's response to the app. 

10.
In case 
- RC app is subscribed to <"moduleType_value"> 
- and sends valid and allowed-by-policies GetInteriorVehicleData_request with "subscribe:false" parameter 
- and SDL gets GetInteriorVehicleData_response without "isSubscribed" param and with "resultCode: SUCCESS" from HMI 

SDL must 

transfer GetInteriorVehicleData_response with added "isSubscribed: true", "resultCode: SUCCESS", "success:true" to the related app. 

Information: SDL does not un-subscribe the app in this case, but just transfers HMI's response with added "isSubscribed: true" to the app. 

11.
In case 
- RC app is subscribed to <"moduleType_value"> 
- and sends valid and allowed-by-policies GetInteriorVehicleData_request with "subscribe:false" parameter 
- and SDL gets GetInteriorVehicleData_response with resultCode: <"any-erroneous-result"> from HMI 

SDL must 

transfer GetInteriorVehicleData_response with resultCode: <"any-erroneous-result"> and without "isSubscribed" param to the related app. 

Information: SDL does not un-subscribe the app in this case, but just transfers HMI's response with removed "isSubscribed" param to the app. 

12. 
In case 
- RC app is subscribed to <"moduleType_value"> 
- and sends valid and allowed-by-policies GetInteriorVehicleData_request with "subscribe:true" parameter for the same <"moduleType_value"> 

SDL must
- forward request to HMI without "subscribe:true" parameter
- not change the subscription status of the app

_Information:_ SDL transfers HMI's response with added "isSubscribed: true" (current subscription status saved on SDL) to the app. 
If HMI response has "isSubscribed" parameter, SDL must ignore it.

13.
In case
- RC app is not subscribed to <"moduleType_value"> 
- and sends valid and allowed-by-policies GetInteriorVehicleData_request with "subscribe:false" parameter for the same <"moduleType_value"> 

SDL must
- forward request to HMI without "subscribe:false" parameter
- not change the subscription status of the app

_Information:_ SDL transfers HMI's response with added "isSubscribed: false" (current subscription status saved on SDL) to the app.
If HMI response has "isSubscribed" parameter, SDL must ignore it.

## Diagrams

Sunscribe on OnInteriorVehicleData
![Sunscribe on OnInteriorVehicleData](https://github.com/smartdevicelink/sdl_requirements/blob/master/detailed_docs/accessories/Subscrine_on_OnInteriorVehicleData.png)

Unsubscribe from OnInteriorVehicleData
![Unsubscribe from OnInteriorVehicleData](https://github.com/smartdevicelink/sdl_requirements/blob/master/detailed_docs/accessories/Unsubscribe_from_OnInteriorVehicleData.png)
