## A. Summary :

- JMeter and Browser are configured Properly but JMeter Recorder is not Recording when dealing with server on localhost

## B. Steps to Reproduce

1. Create New Project in JMeter
2. Add Thread Group
3. Add Recording Controller to the created Thread Group
4. Add HTTP(s) Test Script Recorder and Set Target to be Recording Controller
5. Run the server on the  localhost
6. Open browser 
7. Start Recording in JMeter
8. Enter localhost in URL with the associated Port for the server and Path
9. Start doing any requests 
10. Stop Recording in JMeter

## C. Expected Result

- Requests are created under the recorder controller after running that request within the browser

## D. Actual Result

- Recorder Starts and Stops without any Requests Created under the Controller

## E. Configuration

### 1. Browser

- Proxy is set to Manual Configuration
    - HTTP Proxy is set to `localhost`
    - Port is set to `8080`
    - “Also use this proxy for HTTPS” is Checked

### 2. JMeter

- HTTP(s) Test Script Controller
    - Port is set to `8080` to match Proxy
    - Target Controller : Test Plan  → Thread Group → Recording Controller
    - URL Patterns to Exclude is set to Suggested Excludes

## F. Environment

- Operating System : Win 10 Pro 64x
- Browser : Firefox V.127.0.1 64-bit
- JMeter V.5.6.3

## G. Fix

## 1. Cause :

→ Mozilla has issued update in which the local host cannot be proxied which cause a major issue since JMeter is listening for requests through the Porxy server

## 2. Fix :

- Open Firefox
- Enter `about:config` in URL bar
- Search for **`network.proxy.allow_hijacking_localhost` and set value to True**
