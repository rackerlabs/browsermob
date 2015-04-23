# browsermob

Create a HAR file with incremental "pages" in your Selenium tests.

https://github.com/lightbody/browsermob-proxy#rest-api

Command Line

```
npm install --save-dev browsermob
./node_modules/.bin/browsermob-manager update
./node_modules/.bin/browsermob-manager start # optional --port PORT, defaults to 9090
```

API

```js
var browsermob = require('browsermob');

// attempting to call `.start()` without a live server running will throw an exception
proxy = browsermob.proxy({
    address: 'http://localhost:8080',
    harSessionStartingPort: 8081
});

session = proxy.start(PORT_NUMBER); // POST call to ADDRESS:PORT/proxy/PORT_NUMBER
session.port // will match PORT_NUMBER
proxy.sessions.then(function (ports) {
    console.log(ports); // [8081]
});
session.record(HARFILE_NAME); // PUT request /proxy/PORT_NUMBER/har/, starts a HAR file recording session
session.next(NEW_HARFILE_NAME); // PUT request /proxy/PORT_NUMBER/har/pageRef
var harfile = session.record(); // PUT request /proxy/PORT_NUMBER/har, returns a HAR file, and begins a new one
// run your (Selenium) tests through the proxy
var harfile = session.stop(); // GET request /proxy/PORT_NUMBER/har, then DELETE request /proxy/PORT_NUMBER
var grade = harfile.grade; // run a yslow report, and get the overall score (0 - 100)
```
