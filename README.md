# iWine API 

* Version: "1.0"
* Terms of service: https://iwine.iot/terms
* License: https://iwine.iot/license
* Production Server: https://api.iwine.iot/data/1.0/
* Contact: email: "iwine_email@iwine.iot"

- - -
**iWine** is a smart wine decanter developed by iWine Solutions. It connects to Wi-Fi and acts as an HTTP server so it can interact with devices from the same local network. The decanter reports the following information: volume of the liquid, alcohol content, sugar content, temperature, and a guess on the type of wine.  You can remotely heat or cool the wine to a certain temperature level or to shake the carafe to saturate the wine with oxygen. The above functionality is implemented through a set of HTTP calls (RESTful API). iWine API describes all necessary API's to implement a mobile client.
- - -

## API Calls

### GET /status

**Retrieve decanter contents data**: Access the decanter contents data for a selected iWine decanter on your local network.

Parameters:

Label | Type | Values | Example | Description
-----|-------|--------|---------|-------------
name | string|  - | `iWine2020`| **Decanter Name**. The API responds with a the name of the device.
id | string|  - | `001`| **Decanter ID**. You can call by decanter ID. The API responds with the exact result. 
vol | string|  - | `250`| **Volume**. Volume of the liquid in mililiters (ml). 
temp | string| 0-100 | `45`| **Temperature**. Temperature of the liquid in degrees Celsius..
alc | string|  - | iName| jkljlkj
sug | string|  - | iName| jkljlkj
type | string|  - | iName| jkljlkj
type-guess | string|  - | iName| jkljlkj
vibr | string|  - | iName| jkljlkj

#### Responses
        
Successful Response

responses:
        200:
          description: Successful response
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/200'

Not-Found Response

        404:
          description: Not found response
          content:
            text/plain:
              schema:
                title: Weather not found
                type: string
                example: Not found
  

     
    vol:     
      name: vol
      in: query
      description: ""
      schema:
       type: string

    temp:
      name: temp
      in: query
      description: ""
      schema:
        type: string

    alc:
      name: alc
      in: query
      description: "**Alcohol Content**. Alcohol content of the liquid in %. *Example: `12`*."
      schema:
        type: string

    sug:
      name: sug
      in: query
      description: "**Sugar Content**. Sugar content of the liquid in g/L. *Example: `10`*."
      schema:
        type: string

    type:          
      name: type
      in: query
      description: "**Wine Type**. Type of the wine in the iWine decanter. Must be used together with `type-guess`. *Example: `red`*."
      schema:
        type: string
        enum: [unknown, red, white, rose, orange]
    
    type-guess:          
      name: type-guess
      in: query
      description: "**Wine Type Confidence Coefficient**. As the type of the wine in the iWine decanter is determined using machine learning algorithms, it is accompanied by a certain confidence coefficient. Must be used together with `type`. *Example: `0.5`*."
      schema:
        type: string

    vibr:
      name: vibr
      in: query
      description: '**Vibration**. *Example: on*. Possible values: `on` and `off`.'
      schema:
        type: string
        enum: [on, off]
  schemas:
    200:
      title: Successful response
#      type: object
      properties:
        name:
          type: string
          description: NAME of the decanter
          example: iWineBottle1
        id:
          type: string
          description: ID of the decanter
          example: abc12348
        vol:
          type: string
          description: Volume, mL
          example: 1000
        temp:
          type: string
          description: Temperature, C
          example: 45
        alc:
          type: number
          description: Alcohol content, %
          example: 12
        sug:
          type: number
          description: Sugar content, g/L
          example: 10
        type:
          type: array
          description: Wine type in the decanter
          example: red
        type-guess:
          type: number
          description: Wine Type Confidence Coefficient
          example: 0.5
        vibr:
          type: array
          description: Vibration On/Off
          example: On

