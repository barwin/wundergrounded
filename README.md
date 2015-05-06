# Wundergrounded

A Node.js module that wraps Weather Underground API's in a flexible, easy-to-use interface. Offers optional request bundling, rate limiting, and caching of responses (all in the name of cutting down on the overall number of HTTP requests). Heavily inspired by [wundernode](https://github.com/evalcrux/wundernode) and [wundergroundnode](https://github.com/cendrizzi/wundergroundnode).


## Installation

```npm install wundergrounded --save```


## Usage

### Initialization

The code below shows how to create a barebones Wundergrounded instance:
```javascript
var Wundergrounded = require('wundergrounded');
var wundergrounded = new Wundergrounded();
```


#### Enabling caching
To enable caching of the responses recieved from Weather Underground's API, you would do the following:
```javascript
// Configure a new instance with default caching values
var wundergrounded = new Wundergrounded().cache();
```
See the [cache()](api-cache) API documentation below for more information and configuration options.

#### Enabling rate-limiting
Depending on your plan, Weather Underground limits the number of requests you can make to their API. To create an instance that will automatically limit the number of requests you send to them:
```javascript
// Configure a new instance with default limit values
var wundergrounded = new Wundergrounded().limit();
```
See the [limit()](api-limit) API documentation below for more information and configuration options.

#### Enable both
Create a new instance with both rate limiting and caching enabled:
```javascript
// Configure a new instance with caching and limiting enabled
var wundergrounded = new Wundergrounded().cache().limit();
```

### Getting data

#### Making a request for a single feature
If you just need one feature for a specific location (i.e. current conditions for 27705):
```javascript
wundergrounded.conditions('27705', function(error, response) {
  if(!error) {
  	// do something with the response
  } else {
    // handle the error
  }
});
```

<a name="bundled-request-example"></a>
#### Making a bundled request for multiple features
If you need more than one feature for a specific location (i.e. current conditions, hourly forecast, and the 10-day forecast for 27705):
```javascript
wundergrounded.conditions().hourly().forecast10day().request('27705', function(error, response) {
  if(!error) {
  	// do something with the response
  } else {
    // handle the error
  }
});
```
Note that when bundling requests, the [request()](api-request) call is what ultimately executes the request and retrieves data.


## API docs

### Initialization functions

  <a name="api-cache"></a>
  - ####cache([secondsInCache], [secondsBetweenChecks])

  Configures your Wundergrounded client to cache responses that are received from the Weather Underground API.
    - ```secondsInCache``` - *(optional)* Number of seconds to keep responses in the cache. Defaults to 300.
    - ```secondsBetweenChecks``` - *(optional)* Number of seconds between eviction checks. Defaults to 30.


  <a name="api-limit"></a>
  - ####limit([numberPer], [timePeriod])

  Configures your Wundergrounded client to limit the number of requests it makes to the Weather Underground API. This uses [limiter](https://github.com/jhurliman/node-rate-limiter) under the hood and accepts similar parameters.
  	- ```numberPer``` - *(optional)* Number of requests to make per the specified time period. Defaults to 10.
  	- ```timePeriod``` - *(optional)* The time period to use when limiting (i.e. 'second', 'minute', 'hour', 'day'). Defaults to 'minute'.

### Feature functions

**Note:** All of Wundergrounded's "feature functions" that retrieve Weather Underground API data are chainable. All chained API calls get bundled together on one request, which reduces overall network traffic (and, consequently, the number of requests you make to Weather Underground.) You can read more about combining requests from [Weather Underground's API docs](http://www.wunderground.com/weather/api/d/docs?d=data/index#standard_request_url_format), or see an example of this chainability above in the ["Making a bundled request for multiple features"](bundled-request-example) section.

**Note:** Only supply the ```query``` and ```callback``` parameters to these functions if you don't plan on chaining (bundling) requests.

  - ####alerts([query], [callback])

  Refer to Weather Underground's [alerts](http://www.wunderground.com/weather/api/d/docs?d=data/alerts) documentation for info on this feature.
    - ```query``` - *(optional)* The query to send to the Weather Underground API.
    - ```callback``` - *(optional)* A callback function to invoke once a response is received.

  - ####almanac([query], [callback])

    Refer to Weather Underground's [almanac](http://www.wunderground.com/weather/api/d/docs?d=data/almanac) documentation for info on this feature.
    - ```query``` - *(optional)* The query to send to the Weather Underground API.
    - ```callback``` - *(optional)* A callback function to invoke once a response is received.

  - ####astronomy([query], [callback])

  Refer to Weather Underground's [astronomy](http://www.wunderground.com/weather/api/d/docs?d=data/astronomy) documentation for info on this feature.
    - ```query``` - *(optional)* The query to send to the Weather Underground API.
    - ```callback``` - *(optional)* A callback function to invoke once a response is received.

  - ####conditions([query], [callback])

  Refer to Weather Underground's [conditions](http://www.wunderground.com/weather/api/d/docs?d=data/conditions) documentation for info on this feature.
    - ```query``` - *(optional)* The query to send to the Weather Underground API.
    - ```callback``` - *(optional)* A callback function to invoke once a response is received.

  - ####currenthurricane([query], [callback])

  Refer to Weather Underground's [currenthurricane](http://www.wunderground.com/weather/api/d/docs?d=data/currenthurricane) documentation for info on this feature.
    - ```query``` - *(optional)* The query to send to the Weather Underground API.
    - ```callback``` - *(optional)* A callback function to invoke once a response is received.

  - ####forecast([query], [callback])

  Refer to Weather Underground's [forecast](http://www.wunderground.com/weather/api/d/docs?d=data/forecast) documentation for info on this feature.
    - ```query``` - *(optional)* The query to send to the Weather Underground API.
    - ```callback``` - *(optional)* A callback function to invoke once a response is received.

  - ####forecast10day([query], [callback])

  Refer to Weather Underground's [forecast10day](http://www.wunderground.com/weather/api/d/docs?d=data/forecast10day) documentation for info on this feature.
    - ```query``` - *(optional)* The query to send to the Weather Underground API.
    - ```callback``` - *(optional)* A callback function to invoke once a response is received.

  - ####geolookup([query], [callback])

  Refer to Weather Underground's [geolookup](http://www.wunderground.com/weather/api/d/docs?d=data/geolookup) documentation for info on this feature.
    - ```query``` - *(optional)* The query to send to the Weather Underground API.
    - ```callback``` - *(optional)* A callback function to invoke once a response is received.

  - ####history(date, [query], [callback])

  Refer to Weather Underground's [history](http://www.wunderground.com/weather/api/d/docs?d=data/history) documentation for info on this feature.
    - ```date``` - The [Date](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Date) for which to retrieve history information
    - ```query``` - *(optional)* The query to send to the Weather Underground API.
    - ```callback``` - *(optional)* A callback function to invoke once a response is received.

  - ####hourly([query], [callback])

  Refer to Weather Underground's [hourly](http://www.wunderground.com/weather/api/d/docs?d=data/hourly) documentation for info on this feature.
    - ```query``` - *(optional)* The query to send to the Weather Underground API.
    - ```callback``` - *(optional)* A callback function to invoke once a response is received.
    
  - ####hourly10day([query], [callback])

  Refer to Weather Underground's [hourly10day](http://www.wunderground.com/weather/api/d/docs?d=data/hourly10day) documentation for info on this feature.
    - ```query``` - *(optional)* The query to send to the Weather Underground API.
    - ```callback``` - *(optional)* A callback function to invoke once a response is received.
    
  - ####planner(start, end, [query], [callback])

  Refer to Weather Underground's [planner](http://www.wunderground.com/weather/api/d/docs?d=data/planner) documentation for info on this feature.
    - ```start``` - The start [Date](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Date)
    - ```end``` - The end [Date](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Date)
    - ```query``` - *(optional)* The query to send to the Weather Underground API.
    - ```callback``` - *(optional)* A callback function to invoke once a response is received.
    
  - ####rawtide([query], [callback])

  Refer to Weather Underground's [rawtide](http://www.wunderground.com/weather/api/d/docs?d=data/rawtide) documentation for info on this feature.
    - ```query``` - *(optional)* The query to send to the Weather Underground API.
    - ```callback``` - *(optional)* A callback function to invoke once a response is received.
    
  - ####satellite([query], [callback])

  Refer to Weather Underground's [satellite](http://www.wunderground.com/weather/api/d/docs?d=data/satellite) documentation for info on this feature.
    - ```query``` - *(optional)* The query to send to the Weather Underground API.
    - ```callback``` - *(optional)* A callback function to invoke once a response is received.
    
  - ####tide([query], [callback])

  Refer to Weather Underground's [tide](http://www.wunderground.com/weather/api/d/docs?d=data/tide) documentation for info on this feature.
    - ```query``` - *(optional)* The query to send to the Weather Underground API.
    - ```callback``` - *(optional)* A callback function to invoke once a response is received.
    
  - ####webcams([query], [callback])

  Refer to Weather Underground's [webcams](http://www.wunderground.com/weather/api/d/docs?d=data/webcams) documentation for info on this feature.
    - ```query``` - *(optional)* The query to send to the Weather Underground API.
    - ```callback``` - *(optional)* A callback function to invoke once a response is received.
    
  - ####yesterday([query], [callback])

  Refer to Weather Underground's [yesterday](http://www.wunderground.com/weather/api/d/docs?d=data/yesterday) documentation for info on this feature.
    - ```query``` - *(optional)* The query to send to the Weather Underground API.
    - ```callback``` - *(optional)* A callback function to invoke once a response is received.
    

  <a name="api-request"></a>
  - ####request(query, callback)

  Function for actually "firing" off an HTTP request to the Weather Underground API- used when chaining (bundling) multiple features on one call. An example of it being used can be found in the [Making a bundled request for multiple features](bundled-request-example) section above.
    - ```query``` - The query to send to the Weather Underground API.
    - ```callback``` - A callback function to invoke once a response is received.

 ## Release history

  - 0.1.0 Initial release