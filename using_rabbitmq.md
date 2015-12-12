# Using RabbitMQ

* Installation & Configuration  
* Installing the RabbitMQ Server  
 * Setting up RabbitMQ  
 * Installing RabbitMQ on OS X  
   * Configuring the system host name  
   * Starting/Stopping the RabbitMQ server  



## Installation & Configuration

RabbitMQ is the default broker so it does not require any additional dependencies or initial configuration, other than the URL location of the broker instance you want to use:

```
>>> BROKER_URL = 'amqp://guest:guest@localhost:5672//'
```