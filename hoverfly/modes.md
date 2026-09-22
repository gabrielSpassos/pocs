## Hoverfly Modes

#### Capture Mode

- Used for API simulations
- Can't be used when is webserver, only as a proxy
- intercepts communication between the client application and the external service.
- records the request and response
- once the capture is done, could replace the external service
- By default, Hoverfly will ignore duplicate requests if the request has not changed. 
    - This can be a problem when trying to capture a stateful endpoint that may return a different response each time you make a request.
    - stateful mode can solve this issue

![capture](assets/capture.mermaid.webp)