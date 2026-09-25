## Hoverfly Simulations

- Core feature
- Capture HTTP Traffic as simulations
- Simulations can later be used for testing
- Simulation JSON can be exported, edited, imported in and out Hoverfly
    - JSON must follow Hoverfly Simulation Schema
- Simulations consist of `Request Matchers and Responses`, `Delays` and `Metadata` (“Meta”).

### Request Responses Pairs

- Hoverfly matches `incoming request` from client to `stored requests`
- `stored requests` has association with `stored response` that will be returned to client if match is successful
- The match logic can be configured via `Request Matchers`

#### Request Matchers

- When request is captured, a `Request Matcher` is created for each request field
- Consist in: 
    - request field name
    - type of match to compare vs the incoming request
    - request field value
- There is many flavors of [type of match](https://docs.hoverfly.io/en/latest/pages/reference/hoverfly/request_matchers.html#request-matchers)
- There is also two matching strategies `strongest match` (default) and `first match` (legacy)  
    - [matching strategies](matching-strategies.md)
- So the following request:

|    Field    | Matcher Type | Value                                | 
| ----------- | ------------ | -----                                |
| scheme      |     exact    | “https”                              | 
| method      |     exact    | “GET”                                |
| destination |     exact    | “docs.hoverfly.io”                   | 
| path        |     exact    | “/pages/keyconcepts/templates.html”  |
| query       |     exact    | “query=true”                         |
| body        |     exact    | “”                                   |
| headers     |     exact    |                                      |

- Is stored as:
```json
"request": {
    "path": [
        {
            "matcher": "exact",
            "value": "/pages/keyconcepts/templates.html"
        }
    ],
    "method": [
        {
            "matcher": "exact",
            "value": "GET"
        }
    ],
    "destination": [
        {
            "matcher": "exact",
            "value": "docs.hoverfly.io"
        }
    ],
    "scheme": [
        {
            "matcher": "exact",
            "value": "http"
        }
    ],
    "body": [
        {
            "matcher": "exact",
            "value": ""
        }
    ],
    "query": {
        "query": [
            {
            "matcher": "exact",
            "value": "true"
            }
        ]
    }
}
```

- Match any subdomain, can use the `glob matcher`
```json
"destination": [
    {
        "matcher": "glob",
        "value": "*.hoverfly.io"
    }
]
```

- also can use more than one request matcher for each field
    - This will match on any subdomain of `hoverfly.io` which begins with the letter `d`.
        - docs.hoverfly.io MATCH
        - dogs.hoverfly.io MATCH
        - cats.hoverfly.io NOT MATCH
```json
"destination": [
    {
        "matcher": "glob",
        "value": "*.hoverfly.io"
    },
    {
        "matcher": "regex",
        "value": "(\\Ad)"
    }
]
```

#### Responses

- Each Request Match Set has a response tied to it

```json
"response": {
    "status": 200,
    "body": "Response from docs.hoverfly.io/pages/keyconcepts/templates.html",
    "encodedBody": false,
    "headers": {
        "Hoverfly": [
            "Was-Here"
        ]
    },
    "templated": false
}
```

- Can use binary data encoded as base64

```json
"body": "YmFzZTY0IGVuY29kZWQ=",
"encodedBody": true,
```

- Can use response from a file
    - `-response-body-files-path` var so set where the file is, by default is the working directory
    - when `body` and `bodyFile` are set, `body` takes precedence 

```json
"response": {
  "status": 200,
  "encodedBody": false,
  "templated": false,
  "bodyFile": "responses/200-success.json"
}
```

- Can download the file too
    - use this car to allow `-response-body-files-allow-origin`
        - `hoverfly -response-body-files-allow-origin="https://raw.githubusercontent.com/"`

```json
"response": {
  "status": 200,
  "encodedBody": false,
  "templated": false,
  "bodyFile": "https://raw.githubusercontent.com/SpectoLabs/hoverfly/master/core/handlers/v2/schema.json"
}
```

### Delays

- Add latency do the captured responses
- Can configure delay by URL pattern or HTTP methods
- delay value in miliseconds

### Meta

- Metadata of the simulation
- simulation version
- Hoverfly version that exported the simulation
- Date and time of the export

```json
"meta": {
    "schemaVersion": "v5.2",
    "hoverflyVersion": "v1.2.0",
    "timeExported": "2020-04-25T17:56:32+03:00"
}
```