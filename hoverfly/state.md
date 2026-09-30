<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**

- [State](#state)

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
