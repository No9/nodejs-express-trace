# nodejs-express-trace
An appsody stack based on [nodejs-express](https://github.com/appsody/stacks/tree/master/incubator/nodejs-express) with dynamic tracing

This project provides USDT support to an express app to enable dynamic tracing of User Statically Defined Tracing points in your express application. This allows for better debugging experience in production environments.


### usage 

Create a trace enabled node.js project.

```
$ appsody init No9/nodejs-express-trace
```

In your appsody project
```
const app = require('express')()

app.get('/event', function (req, res, next) {
  res.trace('some.event', 'some event data');
  res.send("Hello from Appsody!");
});
 
module.exports.app = app;
```

Trace the requests with bpftrace
```
% bpftrace -p `pgrep node` -e 'usdt::trace { printf("evt:fired %s : %s\n", str(arg0), str(arg1)) ; }'
```
