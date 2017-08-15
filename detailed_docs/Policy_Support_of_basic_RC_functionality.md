## Use Case 1: Policies check

**Main Flow:**

_Pre-conditions:_

a. device is connected to SDL

b. application is not registered on SDL yet

_Steps:_

1. RC-application requests to register on SDL
2. SDL checks parameters of the request and registers application with REMOTE_CONTROL appHMIType
3. SDL assigns "moduleType" in application's Policy Table
4. SDL assigns "RemoteControl" functional grouping to such application
5. Application requests one of the RC functions with allowed moduleType

_Expected:_

6. SDL allows RC-specific functionality for available RC-modules of the vehicle for such application

**Exceptions:**

5.1 Application requests one of the RC functions with moduleType that is not assigned in its Policy

5.2 SDL disallows such request

**Alternative flow 1:**

1.a.1 Non-RC application registers on SDL

1.a.2. SDL registers application with other than REMOTE_CONTROL appHMIType

1.a.3 SDL does not assign any "moduleType" in application's Policy Table

1.a.4 SDL assigns functional grouping without "RemoteControl" group for such application

1.a.5 Application requests one of the RC functions

_Expected:_

1.a.6 RC-request from non-RC application is disallowed

> Requirement: [#7](https://github.com/smartdevicelink/sdl_requirements/issues/7)
