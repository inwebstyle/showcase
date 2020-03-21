openapi: '3.0.3'
info:
  title: "iWine API"
  description: "iWine is a smart wine decanter developed by iWine Solutions. It connects to Wi-Fi and acts as an HTTP server so it can interact with devices from the same local network. The decanter reports the following information: volume of the liquid, alcohol content, sugar content, temperature, and a guess on the type of wine.  You can remotely heat or cool the wine to a certain temperature level or to shake the carafe to saturate the wine with oxygen. The above functionality is implemented through a set of HTTP calls (RESTful API). iWine API describes all necessary API's to implement a mobile client."
  version: "1.0"
  termsOfService: "https://iwine.iot/terms"
  contact:
    name: "iWine API"
    url: "https://iwine.iot/api"
    email: "iwine_email@iwine.iot"
  license:
    name: "License Name"
    url: "https://iwine.iot/license"
servers:
  - url: https://api.iwine.iot/data/1.0/
    description: Production iWine server

paths: 
  /status:
    get:
      tags:
      - Current Decanter Contents Data
      summary: "Call current decanter contents data."
      description: "Access current decanter contents data for a selected iWine decanter on your local network."
      operationId: CurrentDecanterData
      parameters:
        - $ref: '#/components/parameters/name'
        - $ref: '#/components/parameters/id'
        - $ref: '#/components/parameters/vol'
        - $ref: '#/components/parameters/temp'
        - $ref: '#/components/parameters/alc'
        - $ref: '#/components/parameters/sug'
        - $ref: '#/components/parameters/type'
        - $ref: '#/components/parameters/type-guess'
        - $ref: '#/components/parameters/vibr'
        
      responses:
        200:
          description: Successful response
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/200'

        404:
          description: Not found response
          content:
            text/plain:
              schema:
                title: Weather not found
                type: string
                example: Not found
  
components:
  parameters:
    name:
      name: name
      in: query
      description: "**Decanter Name**. *Example: `iWine2020`*. The API responds with a the name of the device."
      schema:
        type: string

    id:
      name: id
      in: query
      description: "**Decanter ID**. *Example: `001`*. You can call by decanter ID. The API responds with the exact result. The limit of devices is 8. *Note: A single ID counts as a one API call. So, if you have 3 decanter IDs, it's treated as 3 API calls.*"
      schema:
        type: string
     
    vol:     
      name: vol
      in: query
      description: "**Volume**. Volume of the liquid in mililiters (ml). *Example: `250`*."
      schema:
       type: string

    temp:
      name: temp
      in: query
      description: "**Temperature**. Temperature of the liquid in degrees Celsius. *Example: `45`*."
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
    
  securitySchemes:
    app_id:
      type: apiKey
      description: API key to authorize requests. If you don't have an OpenWeatherMap API key, use `fd4698c940c6d1da602a70ac34f0b147`.
      name: appid
      in: query
