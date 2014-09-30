# **WAMP-Tessel**

**WAMP-Tessel** is an open-source implementation of the **[Web Application Messaging Protocol V2](http://wamp.ws/)** specifically intended to run on the Tessel microcontroller. This project is a stripped-down version of **Autobahn**|JS, a subproject of the [Autobahn project](http://autobahn.ws/).

**Autobahn**|JS is a robust library that can run in multiple environments, including web browsers and in NodeJS, but it can not be used on Tessel because of its large size (by microcontroller standards) and its use of unsupported libraries.

WAMP-Tessel differs most notably from Autobahn|JS in these ways:
 * It is compatible with the Tessel microcontroller;
 * It is NOT compatible with web browser environments;
 * The only supported transport is WebSockets;
 * It supports only the WAMP Basic Profile (https://github.com/tavendo/WAMP/blob/master/spec/basic.md) - no encryption or other Advanced Profile features are supported.

It is licensed under the [MIT licensed](/LICENSE).

## Show me some code

The following example implements all four roles that **Autobahn**|JS offers

 * Publisher
 * Subscriber
 * Caller (calls a remote procedure)
 * Callee (offers a remote procedure)

**The code runs as part of your Tessel project in Node.js!**

```javascript
var wamp = require('wamp-tessel');

var connection = new wamp.Connection({url: 'ws://127.0.0.1:9000/', realm: 'realm1'});

connection.onopen = function (session) {

   // 1) subscribe to a topic
   function onevent(args) {
      console.log("Event:", args[0]);
   }
   session.subscribe('com.myapp.hello', onevent);

   // 2) publish an event
   session.publish('com.myapp.hello', ['Hello, world!']);

   // 3) register a procedure for remoting
   function add2(args) {
      return args[0] + args[1];
   }
   session.register('com.myapp.add2', add2);

   // 4) call a remote procedure
   session.call('com.myapp.add2', [2, 3]).then(
      function (res) {
         console.log("Result:", res);
      }
   );
};

connection.open();
```

## Get it

WAMP-Tessel is available via the Node package manager [here](https://www.npmjs.org/package/WAMP-Tessel). To install:

    npm install wamp-tessel


## More information

For more information, have a look at the Autobahn|JS [project documentation](http://autobahn.ws/js). This provides:

* [a quick 'Getting Started'](http://autobahn.ws/js/gettingstarted.html)
* [tutorials on RPC and PubSub](http://autobahn.ws/js/tutorial.html)
* [a list of all examples in this repo](http://autobahn.ws/js/examples_overview.html)
* [a full API reference](http://autobahn.ws/js/reference.html)

## Acknowledgements

WAMP-Tessel is essentially a stripped-down version of **Autobahn**|JS, a subproject of the [Autobahn project](http://autobahn.ws/). If you want to learn more about how WAMP (or this library!) works, you should check out that project.
