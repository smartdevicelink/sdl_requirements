## Functional Requirements

1. 
In case remote-control application requested to be registered on SDL

SDL must

Register such application with REMOTE_CONTROL appHMI type

2. 
In case "moduleType" field exists in Preloaded PT

PoliciesManager must

add "moduleType" field to policies database, when creating this database 

3. 
In case Updated PT has "moduleType" field

PoliciesManager must

add "moduleType" field to Local PT

4.
"moduleType" field must be supported only in "default" and <"appID"> sub-sections of "app_policies" section in PT 

_Information:_ 
The above requirement means, that in case "moduleType" exists in any other section beside the stated above, SDL must ignore it and consider the corresponding PT as valid.

5.
In case "moduleType" has invalid value and one or more valid values

PoliciesManager must

cut this invalid value off and leave valid values as they are.

_Info:_ moduleType valid values are only those listed in "ModuleType" enumeration of MOBILE_API (that is, any other values must be treated as invalid).A baselined mobile_API has the following values of "ModuleType": "CLIMATE", "RADIO".

6.
In case in Updated PT or Preloaded PT "moduleType" has only invalid values in the array,

PoliciesManager must

ignore the whole "moduleType" field and not write it into Local PT.

7.
In case "moduleType" has empty array,

PoliciesManager must add it to Local PT

8. In case

"moduleType" in app's assigned policies has one or more valid values, 

PoliciesManager must allow app's remote-control RPCs that contain only the listed value(s) of "moduleType" parameter

9. 
In case 

"moduleType" in app's assigned policies has an empty array,

PoliciesManager must

allow app's remote-control RPCs that contain any of "moduleType" parameters

10.

In case 

"moduleType" does not exist in app's assigned policies,

PoliciesManager must

disallow app's remote-control RPCs that contain any of "moduleType" parameters

## Non-functional requirements:

a. In "functional_groupings" section of Policy table must be added new functional grouping "RemoteControl":

```xml
"RemoteControl": {
                "rpcs": {
                    "ButtonPress": {
                        "hmi_levels": ["BACKGROUND",
                        "FULL",
                        "LIMITED"]
                    },
                    "GetInteriorVehicleData": {
                        "hmi_levels": ["BACKGROUND",
                        "FULL",
                        "LIMITED"]
                    },
                    "SetInteriorVehicleData": {
                        "hmi_levels": ["BACKGROUND",
                        "FULL",
                        "LIMITED"]
                    },
                    "OnInteriorVehicleData": {
                        "hmi_levels": ["BACKGROUND",
                        "FULL",
                        "LIMITED"]
                    },
		    "SystemRequest": {
                        "hmi_levels": ["BACKGROUND",
                        "FULL",
                        "LIMITED",
                        "NONE"]
                    }
                }
            }
```

b. To assign permissions for remote-control registered applications new field "groups_primaryRC" must be supported for "default" and "<appID>" sub-sections of "app_policies" section.

_Info:_ Includes "RemoteControl" functional grouping and default UI commands.

c. To control access of application to specific module type of the vehicle new optional moduleType field is added for SnapshotPT, Updated PT, Preloaded PT (that is, in case "moduleType" is omitted in the table, the table is still valid).

_Info:_ "moduleType": ["RADIO", "CLIMATE", "SEAT"]
