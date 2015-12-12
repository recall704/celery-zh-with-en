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
For a description of broker URLs and a full list of the various broker configuration options available to Celery, see [Broker Settings](http://docs.celeryproject.org/en/latest/configuration.html#conf-broker-settings).


## Installing the RabbitMQ Server

See [Installing RabbitMQ](http://www.rabbitmq.com/install.html) over at RabbitMQ’s website. For Mac OS X see [Installing RabbitMQ on OS X](http://docs.celeryproject.org/en/latest/getting-started/brokers/rabbitmq.html#installing-rabbitmq-on-os-x).  
> Note  
>If you’re getting nodedown errors after installing and using rabbitmqctl then this blog post can help you identify the source of the problem:

> http://somic.org/2009/02/19/on-rabbitmqctl-and-badrpcnodedown/