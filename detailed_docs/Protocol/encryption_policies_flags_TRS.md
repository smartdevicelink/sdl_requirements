## Functional requirements

1.  
In case  
in `app_policies` flag has value `encryption_required=true` or value does not exist   
and in function group `encryption_required=true` 

SDL must  

send OnPermissionsChange(requireEncryption=true) notification to mob app 
receive/send all the RPCs within that function group encrypted when encryption for RPC service 7 is enable

2.  
In case  
in `app_policies` flag has value `encryption_required=false` 

SDL must 

NOT check the value of `encryption_required` in the function group  
NOT enable encryption for RPC service 7

3.  
In case  
in function group `encryption_required=false` or value does not exist for a function group

SDL must 
receive/send RPC messages of that function group unencrypted in both encryption enabled and disabled SDL services

## Non-functional requirements

1. An optional Boolean flag `encryption_required` must be added to each app within `app_policies` to indicate whether the app requires encryption or not.

```json
"app_policies": {
    "default": {
+     "encryption_required": false,
      "keep_context": false,
      "steal_focus": false,
      "priority": "NONE",
      "default_hmi": "NONE",
      "groups": [
        "Base-4"
      ]
    },
    "appid_123456789": {
+     "encryption_required": true,
      "keep_context": false,
      "steal_focus": true,
      "priority": "NONE",
      "default_hmi": "NONE",
      "groups": [
        "Base-4", "RemoteControl"
      ]
    },
    ...
}
```

2. An optional Boolean flag `encryption_required` must be added to a function group to indicate whether all the RPCs within this function group require encryption not. 

_Example_

```json
"RemoteControl": {
+   "encryption_required" : true,
    "rpcs": {
        "GetInteriorVehicleData": {
            "hmi_levels": ["BACKGROUND", "FULL", "LIMITED"]
        },
        "GetInteriorVehicleData": {
            "hmi_levels": ["BACKGROUND", "FULL", "LIMITED"]
        },
        ...
    }
}
```

_Note: SDL must cascade this flag from the function group level to each RPC level._
