<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**

- [State](#state)
  - [Setting State when Performing a Match](#setting-state-when-performing-a-match)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

## State

- Key/values for internal state
- some request matcher can be made only when hoverfly is in certain state
- some matcher can mutate the hoverfly state

### Setting State when Performing a Match

- Response has fields: `transitionsState` and `removesState`

```json
"request": {
    "path": [
        {
            "matcher": "exact",
            "value": "/pay"
        }
    ]
},
"response": {
    "status": 200,
    "body": "eggs and large bacon",
    "transitionsState" : {
        "payment-flow" : "complete"
    },
    "removesState" : [
        "basket"
    ]
}
```

| **Current State of Hoverfly**    | **New State of Hoverfly?** | **reason**                                       |
| -------------------------------- | -------------------------- | ------------------------------------------------ |
| payment-flow=pending,basket=full | payment-flow=complete      | Payment value transitions, basket deleted by key |
| basket=full                      | payment-flow=complete      | Payment value created, basket deleted by key     |
|                                  | payment-flow=complete      | Payment value created, basket already absent     |

### Requiring State in order to Match

- Can add a `requiresState` at the request

```json
"request": {
    "path": [
        {
            "matcher": "exact",
            "value": "/basket"
        }
    ]
    "requiresState": {
        "eggs": "present",
        "bacon" : "large"
    }
},
"response": {
    "status": 200,
    "body": "eggs and large bacon"
}
```

| **Current State of Hoverfly** | **matches?** | **reason**                                         |
| ----------------------------- | ------------ | -------------------------------------------------- |
| eggs=present,bacon=large      | true         | Required and current state are equal               |
| eggs=present,bacon=large,f=x  | true         | Additional state ‘f=x’ is not used by this matcher |
| eggs=present                  | false        | Bacon is missing                                   |
| eggs=present,bacon=small      | false        | Bacon is has the wrong value                       |

### Managing state via Hoverctl

- Can get or set the state via cli 

```bash
$ hoverctl state --help
$ hoverctl state get-all
$ hoverctl state get key
$ hoverctl state set key value
$ hoverctl state delete-all
```