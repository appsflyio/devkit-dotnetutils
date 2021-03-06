# About appsfly.io Dev Kit .Net Utils
This library contains resources to help communicate with appsfly.io execution server.
For all communications with execution server, your application should be registered and a secret key needs to be generated. 

Please contact integrations@appsfly.io for your credientials.

#  Get Started
 <a name="SECRET_KEY"></a><a name="APP_KEY"></a><a name="EXECUTOR_URL"></a>
#### Application Params
| Key | Description |
| --- | --- |
| SECRET_KEY   | Secret Key is required for encryption. Secret Key should be generated on the Appsfly publisher dashboard |
| APP_KEY  | Application key to identify the publisher instance|
| EXECUTOR_URL | Url to reach appsfly.io Microservices |

**NOTE:** Above params are needed for checksum generation. Please refer to the methods mention below.

 <a name="MODULE_HANDLE"></a> <a name="UUID"></a>
#### Micro Module Params

| Key | Description |
| --- | --- |
| MODULE_HANDLE  | Each micromodule of a service provider is identified by MODULE_HANDLE |
| UUID  | UniqueID to identify user session|

 <a name="INTENT"></a> <a name="PAYLOAD"></a>
#### Intent Params
| Key | Description |
| --- | --- |
| INTENT | Intent is like an endpoint you are accessing to send messages |
| PAYLOAD | Data payload |

# Integration options  

### Option 1: Appsfly DLL Util
The DLL can be included to handle authorization. There is no need for you to handle checksum generation and verification.

#### Setup DLL

Add References
###### Step 1. Add dll to your project references
```
// Util DLL can be downloaded from this repo
```

###### Step 2. Add the namespace "appsfly_dotnetutil"

#### Configuration
```
AppInstance.AFConfig config = new AppInstance.AFConfig("EXECUTOR_URL", "SECRET_KEY", "APP_KEY");
```  
#### Execution
```
AppInstance activitiesProvider = new AppInstance(config, "MODULE_HANDLE");
activitiesProvider.exec("INTENT", "PAYLOAD_TXT", "UUID", (result) => { Console.WriteLine(result) });
OR
activitiesProvider.execSync("INTENT", "PAYLOAD_TXT", "UUID", (result) => { Console.WriteLine(result) });
```

### Option 2: API Endpoint
appsfly.io exposes a single API endpoint to access Microservices directly.

#### Endpoint
[EXECUTOR_URL](#EXECUTOR_URL)/executor/exec

#### Method
POST

#### Headers
| Header | Description |
| --- | --- |
| X-UUID | [UUID](#UUID) |
| X-App-Key | [APP_KEY](#APP_KEY)|
| X-Module-Handle | [MODULE_HANDLE](#MODULE_HANDLE)|
| X-Checksum | CHECKSUM. Please use [Appsfly DLL](https://github.com/appsflyio/devkit-javautils/blob/master/src/main/java/io/appsfly/crypto/CtyptoUtil.java) to generate and verify checksum. |
| Content-Type | Must be "application/json" |

#### Body
[INTENT](#INTENT), [PAYLOAD](#PAYLOAD)
``` 
{
  "intent":"INTENT",
  "data":"PAYLOAD"
 } 
 ```

----------------------------------------

### Micro Service Response
Response format will be dependent on microservice. Please go through [this documentation](https://github.com/appsflyio/micro-module-documentations) for different microservices.
