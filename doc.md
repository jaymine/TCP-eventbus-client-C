
**C TCP eventbus client** is the Vert.x TCP eventbus client module for C. I hope you are familiar with Vert.x TCP eventbus bridge. If not get more details from [here](http://vertx.io/docs/vertx-tcp-eventbus-bridge/java/ "http://vertx.io/docs/vertx-tcp-eventbus-bridge/java/"). 

- [Open project on Github](https://github.com/jaymine/TCP-eventbus-client-C)

## Implementation ##

Under Eventbus, 2 threads are running for input and output streams.

![Complete Design](https://raw.githubusercontent.com/jaymine/TCP-eventbus-client-Python/gh-pages/2.png)

TCP eventbus bridge has 2 special features

- LV messages 
- Json schema

![LV Message](https://raw.githubusercontent.com/jaymine/TCP-eventbus-client-Python/gh-pages/3.png)

Every message consists of a json message and the length of the json message. Length of a json message is an unsigned integer. It means 4 bytes. But the module is done that work for you. You just have to focus on the json message.


### Create Eventbus ###

to import eventbus,

	#include "vertx.h"

to create eventbus,

	setHost("127.0.0.1") 
	setPort(7000)
	setTimeOut(1000)
	create_eventbus();
    start_eventbus();

to create Eventbus those parameters used.

- Host - default is "127.0.0.1"
- Port - default is 7000
- Time Out - Default is 1000 milliseconds. Socket is using polling method to establish an asynchronous environment. Time out is polling time period. 

### Addressing ###

Every eventbus message has an address and the client will handle the message according to its address. An address can be any string.

	eg: "vertx", "pink pig", "x.y.z"

### Handling Messages ###

To handle a message, Handlers are used. Handlers are functions that take only one argument (String *). 

to implement a handler,

	void myHandler(String *msg){
    	printf("%s",*msg);
    	i++;
	}
	

### Register Handlers ###

In order to handle messages, Handlers should be registered under an address.

to register a handler,

	eventbus_register("Get",myHandler);

After that any message that comes to the address will be sent to the handler.

### Unregister Handlers ###

to unregister an address,

	eventbus_unregister("Get");

### Send ###

Messages can be sent,

	eventbus_send("Get",NULL,"{\"type\":\"text\"}","{\"message\":\"welcome\"}");
    eventbus_send("Get","Get","{\"type\":\"text\"}","{\"message\":\"welcome\"}");
		

send takes 4 arguments

- address
- replyAddress
- headers - Json String
- message - Json String


### Publish ###

Messages can be published in this way,

	eventbus_publish("Get","{\"type\":\"text\"}","{\"message\":\"welcome\"}");

publish takes 3 arguments 

- address
- headers - Json String
- message - Json String

### Close Connection ###

You must close the connection at the end of the program. If not program wont close at the end.

	close_eventbus(); 

## Examples ###

You can get examples from Github

* [Simple example with the code](https://github.com/jaymine/TCP-eventbus-client-C/tree/master/test "Get client and server codes")

