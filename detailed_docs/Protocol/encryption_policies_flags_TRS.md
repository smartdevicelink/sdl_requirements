## Functional requirements

1.  
In case  
in `app_policies` flag in the app level has value `encryption_required=true`  
and in function group `encryption_required=true` 

SDL must  

receive/send all the RPCs within that function group encrypted when encryption for RPC service 7 is enable

2. 
In case  
in `app_policies` flag in the app level has value `encryption_required=false`

SDL must  
NOT check the flag in the function group level or RPC level  
send RPCs unencrypted

3.
In case  
in `app_policies` flag in the app level `encryption_required` does not exist

SDL must  
check the value of `encryption_required` in the function group  
receive/send all the RPCs within that function group encrypted only in function group `encryption_required=true`  
receive/send all the RPCs within that function group UNencrypted if in function group `encryption_required=false` OR does not exist

### Expected system behavior of RPC

|#|app_policies<br>encryption_required|functional_group<br>encryption_required|RPC<br>requireEncryption|top level of OnPermissionsChange<br>requireEncryption|PermissionItem<br>requireEncryption| 
|-|:-------------------------------:|:--------------------------------:|:-----------------------:|:-----------:|:-----------:|
|1|TRUE|TRUE|TRUE|TRUE|TRUE|
|2|TRUE|FALSE|FALSE|TRUE|MISSING|
|3|TRUE|MISSING|FALSE|TRUE|MISSING|
|4|FALSE|TRUE|FALSE|FALSE|MISSING|
|5|FALSE|FALSE|FALSE|FALSE|MISSING|
|6|FALSE|MISSING|FALSE|FALSE|MISSING|
|7|MISSING|TRUE|TRUE|TRUE|TRUE|
|8|MISSING|FALSE|FALSE|MISSING|MISSING|
|9|MISSING|MISSING|FALSE|MISSING|MISSING|

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
