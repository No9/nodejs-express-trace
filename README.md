# nodejs-express-trace
An appsody stack based on [nodejs-express](https://github.com/appsody/stacks/tree/master/incubator/nodejs-express) with dynamic tracing

This project provides USDT support to an express app to enable dynamic tracing of User Statically Defined Tracing points in your express application. This allows for better debugging experience in production environments.


### usage 

Create a trace enabled node.js project.

```
$ mkdir my-app
$ cd my-app
$ appsody init nodejs-express
```

In the project app.js add a trace event
```
const app = require('express')()

app.get('/event', function (req, res, next) {
  res.trace('some.event', 'some event data');
  res.send("Hello from Appsody!");
});
 
module.exports.app = app;
```

Run the project
```
$ appsody run
```

As root trace the requests with bpftrace
```
$ sudo bpftrace -p `pgrep node` -e 'usdt::trace { printf("evt:fired %s : %s\n", str(arg0), str(arg1)) ; }'
```
