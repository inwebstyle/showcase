# iWine API 

* Version: 1.0
* openapi: 3.0.3
* Terms of service: https://iwine.iot/terms
* License: https://iwine.iot/license
* Production Server: https://api.iwine.iot/data/1.0/
* Contact: email: "iwine_email@iwine.iot"

- - -
**iWine** is a smart wine decanter developed by iWine Solutions. It connects to Wi-Fi and acts as an HTTP server so it can interact with devices from the same local network. The decanter reports the following information: volume of the liquid, alcohol content, sugar content, temperature, and a guess on the type of wine.  You can remotely heat or cool the wine to a certain temperature level or to shake the decanter to saturate the wine with oxygen. The above functionality is implemented through a set of HTTP calls (RESTful API). **iWine API** describes all necessary API's to implement a mobile client.
- - -

## API Calls

### Security Scheme
The application security is realized by the following:
* HTTPS: All communications go though HTTPS via SSL.


### GET /status

**Retrieve decanter contents data**: access the decanter contents data for a selected iWine decanter on your local network.

#### Parameters:

Label     | Type   | Value Range                      | Example       | Description
----------|--------|----------------------------------|---------------|-------------
name      | string |    -                             | `iWineBottle1`| **Decanter Name**. A human-friendly name of the iWine decanter.
id        | string |    -                             | `abc12348`    | **Decanter ID**. A machine name of the iWine decanter. 
vol       | number | 0-4000                           | `250`         | **Volume**. Current volume of the liquid in mililiters (mL). 
temp      | number | 0-100                            | `45`          | **Temperature**. Current temperature of the liquid in degrees Celsius.
alc       | number |  0-90                            | `12.5`        | **Alcohol Content**. Current alcohol content of the liquid in %.
sug       | number | 0-250                            | `10`          | **Sugar Content**. Current sugar content of the liquid in g/L.
type      | string | array {red, white, rose, orange} |`red`          | **Wine Type**. Type of the wine in the iWine decanter. Possible values: `red`, `white`, `rose`, and `orange`. 
type-guess| number | 0-100                              | `50`         | **Wine Type Guess Confidence Coefficient**. A confidence coefficient of the wine type in %.
vibr      | string | array {on, off}                  | `on`          | **Vibration** on/off. Shows if the decanter is currently shaking. Possible values: `on` and `off`. Default value is `off`.

#### Responses
        
**Successful Response**

    {
      "name": "iWineBottle1",
      "id": "abc12348",
      "vol": 1000,
      "temp": 45,
      "alc": 12,
      "sug": 10,
      "type": "red",
      "type-guess": 0.5,
      "vibr": "On"
    }


**Error Codes**

Code |  Description
-----|------------------
404  | Device not found
           
  

### POST /{parameter}
Using these API's you can:
* Bring the decanter contents to a desired temperature level.
* Shake the decanter to saturate the wine with oxygen or stop the shaking process.

#### Parameters:

Label     | Type   | Value Range     | Example | Description
----------|--------|-----------------|---------|-------------
temp      | number | 0-100           | `45`    | **Target temperature** of the liquid in degrees Celsius.
vibr      | string | array {on, off  | `on`    | **Vibration**: `on` - start shaking, `off` - stop shaking. 

> Example 1: Cool the wine from the current temperature (45C) to 18C

```POST /temp/{18}```

> Example 2: Shake the decanter to saturate the wine with oxygen:

```POST /vibr/{on}```


#### Responses

**Successful Response**

    {
      Request successfully processed.
    }
    
**Error Codes**

Code | Description
-----|--------------
405  | Invalid input
