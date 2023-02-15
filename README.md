# Setup Steps

- Turn on your Redwave FTIR device
- Connect your device to your PC via USB
- Query your PC for the port that the device is connected to 
- In `response.py`, change the port value
- In `request.py`, change the port value below to for a list of available commands.
- Open two terminals
- In terminal 1: run `python3 response.py`
- In terminal 2, run `python3 request.py`
- In `request.py`, change the command `{"command":"disconect"}` as desired; see documentation for a list of available commands

<br />

# API Description

_NOTE: The API will be available on Redwave's XplorIR devices starting with release v0.40. The first release of the API will only return fixed (not real) data.  Additional information on when the API will feature real data and more endpoints will be posted on this page._

_NOTE 2: This API is not in a stable state and is subject to change._

The following commands are available in a request/response fashion. Our Team Leader app or your own proprietary app will send a JSON command to Redwave's XplorIR device and the device's API return a JSON response. 

## disconnect

To disconnect the cleian

`{"command":"disconect"}`

Disconnects client from host

## get_wifi_info

`{"command":"get_wifi_info"}`

Gets Wifi information including: a list of available networks, the network that the device is connected to, and whether the device has wifi enabled.

## set_wifi

Sets Wifi settings including whether Wifi is on/off and which network it should be connected to.

`{"command":"set_wifi"}`

## get_commands

This command will list the available commands on the device.  

`{"command":"get_commands"}`

The response for this command is as follows:

```
{
    "description": "A list of commands",
    "message":"Successfully retrieved a list of commands",				
    "results": [
        {
            "command": "disconnect",
            "description": "Diconnects client from the host."
        },
        {
            "command": "get_sessions",
            "description": "Returns a list of sessions."
        },
        {
            "command": "get_wifi_info",
            "description": "Returns a list of available wifi networks, the currently connected network, and whether wifi is enabled."
        }
    ],
    "status": "success",
}
```

## get_sessions

Each time you run the device in single point mode or in continuous monitoring mode, we store the results as a session.  A single point session will have a single value while a continous monitoring session will have one or many values. To get a list of sessions from the device, send the following _get_sessions_ command:

`{"command":"get_sessions"}`

The response from this command is as follows:

```
{
    "errors": null,
    "description": "A list of sessions",
    "results": [
        {				
            "date": "2023-01-31T20:47:37.224256",                
            "name": "2023-01-31/20-47-37"
            "type": "singlePoint",
            "uuid": "3eca380e-54c6-4a2f-9d9f-cd86fbd05c96",
        },
        {
            "date": "2023-02-02T10:37:17.114257",
            "name": "2023-02-02/10-37-17",
            "type": "continousMonitoring",
            "uuid": "7yty380e-54c6-4a2f-9d9f-cd86fbd05c96",    							
        }
    ]
}
```

## get_session

This endpoint gets details of a current sessions

`{"command":"get_session", "args": { "id": "7yty380e-54c6-4a2f-9d9f-cd86fbd05c96" }}`

The response from this command is as follows:

```
{
    "message": "Successfully retrieved session data",
    "results": {
        "date": "2023-01-31T20:47:37.224256",
        "name": "2023-01-31/20-47-37",
        "uuid": "3eca380e-54c6-4a2f-9d9f-cd86fbd05c96",						
        "data": [
            {            
                "uuid": "3eca380e-54c6-4a2f-9d9f-cd86fbd05c96",
                "name": "2023-01-31/20-45-37",   
                "date": "2023-01-31T20:45:37.224256",
                "coords": {
                    "lat": 12345,
                    "lon": 12321
                },            
                "compounds": []
            }, 
            {      
                "uuid": "3eca380e-54c6-4a2f-9d9f-cd86fbd05c96",
                "name": "2023-01-31/20-46-37",
                "date": "2023-01-31T20:46:37.224256",
                "coords": null,            
                "compounds": []
            },         
            {         
                "uuid": "eb8f268e-8007-45e9-9438-aadec17ac09f",
                "name": "2023-01-31/20-47-37",            
                "date": "2023-01-31T20:47:37.224256",
                "coords": {
                    "lat": 12345,
                    "lon": 12321
                },            					
                "compounds": [
                    {
                        "cas_number": "67-63-0",
                        "name": "2-Propanol",
                        "score": 0.982,
                        "is_top_hit": true
                    }
                ]
            }
        ]
    },
    "status": "success"
}
```

## get_sample

The following endpoint will get detailed information on a specific sample.

`{"command":"get_sample"}`

The response from this command is as follows:

```
{         
    "uuid": "eb8f268e-8007-45e9-9438-aadec17ac09f",  
    "name": "2023-01-31/20-47-37",          
    "date": "2023-01-31T20:47:37.224256",
    "coords": {
        "lat": 12345,
        "lon": 12321
    },            					
    "compounds": [
        {
            "cas_number": "67-63-0",
            "name": "2-Propanol",
            "score": 0.982,
            "is_top_hit": true
        }
    ]
}
```
