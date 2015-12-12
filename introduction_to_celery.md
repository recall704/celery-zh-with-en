# Introduction to Celery

>* What is a Task Queue?  
* What do I need?  
* Get Started  
* Celery is…  
* Features  
* Framework Integration  
* Quickjump  
* Installation  


## What is a Task Queue?

Task queues are used as a mechanism to distribute work across threads or machines.  

A task queue’s input is a unit of work called a task. Dedicated worker processes constantly monitor task queues for new work to perform.  

Celery communicates via messages, usually using a broker to mediate between clients and workers. To initiate a task, a client adds a message to the queue, which the broker then delivers to a worker.  

A Celery system can consist of multiple workers and brokers, giving way to high availability and horizontal scaling.  

Celery is written in Python, but the protocol can be implemented in any language. So far there’s [RCelery](http://leapfrogdevelopment.github.com/rcelery/) for the Ruby programming language, [node-celery](https://github.com/mher/node-celery) for Node.js and a [PHP client](https://github.com/gjedeer/celery-php). Language interoperability can also be achieved by [using webhooks](http://docs.celeryproject.org/en/latest/userguide/remote-tasks.html#guide-webhooks).  


## What do I need?
Celery requires a message transport to send and receive messages. The RabbitMQ and Redis broker transports are feature complete, but there’s also support for a myriad of other experimental solutions, including using SQLite for local development.  

Celery can run on a single machine, on multiple machines, or even across data centers.  

> Version Requirements
Celery version 3.0 runs on  
* Python ❨2.5, 2.6, 2.7, 3.2, 3.3❩  
* PyPy ❨1.8, 1.9❩  
* Jython ❨2.5, 2.7❩

>This is the last version to support Python 2.5, and from the next version Python 2.6 or newer is required. The last version to support Python 2.4 was Celery series 2.2.  


## Get Started
If this is the first time you’re trying to use Celery, or you are new to Celery 3.0 coming from previous versions then you should read our getting started tutorials:  
* First Steps with Celery
* Next Steps



## Celery is…


* **Simple**

 Celery is easy to use and maintain, and it doesn’t need configuration files.

 It has an active, friendly community you can talk to for support, including a mailing-list and an IRC channel.

 Here’s one of the simplest applications you can make:
    
    ```python
    from celery import Celery
    
    app = Celery('hello', broker='amqp://guest@localhost//')
    
    @app.task
    def hello():
        return 'hello world'
    ```
* **Highly Available**  

 Workers and clients will automatically retry in the event of connection loss or failure, and some brokers support HA in way of Master/Master or Master/Slave replication.

* **Fast**

  A single Celery process can process millions of tasks a minute, with sub-millisecond round-trip latency (using RabbitMQ, py-librabbitmq, and optimized settings).

* **Flexible**

 Almost every part of Celery can be extended or used on its own, Custom pool implementations, serializers, compression schemes, logging, schedulers, consumers, producers, autoscalers, broker transports and much more.


**It supports**
* Brokers

 [RabbitMQ](http://docs.celeryproject.org/en/latest/getting-started/brokers/rabbitmq.html#broker-rabbitmq), [Redis](http://docs.celeryproject.org/en/latest/getting-started/brokers/redis.html#broker-redis),  
 [MongoDB](http://docs.celeryproject.org/en/latest/getting-started/brokers/mongodb.html#broker-mongodb) (exp), ZeroMQ (exp)  
 [CouchDB](http://docs.celeryproject.org/en/latest/getting-started/brokers/couchdb.html#broker-couchdb) (exp), [SQLAlchemy](http://docs.celeryproject.org/en/latest/getting-started/brokers/sqlalchemy.html#broker-sqlalchemy) (exp)  
 [Django ORM](http://docs.celeryproject.org/en/latest/getting-started/brokers/django.html#broker-django) (exp), [Amazon SQS](http://docs.celeryproject.org/en/latest/getting-started/brokers/sqs.html#broker-sqs), (exp)  
 and more…

* Concurrency  
 prefork (multiprocessing),  
 [Eventlet](http://eventlet.net/), [gevent](http://gevent.org/)  
 threads/single threaded  

* Result Stores  
 * AMQP, Redis  
 * memcached, MongoDB  
 * SQLAlchemy, Django ORM  
 * Apache Cassandra  

* Serialization  
 * pickle, json, yaml, msgpack.  
 * zlib, bzip2 compression.  
 * Cryptographic message signing.  


## Features
* **Monitoring**  
 A stream of monitoring events is emitted by workers and is used by built-in and external tools to tell you what your cluster is doing – in real-time.  
 
 [Read more….](http://docs.celeryproject.org/en/latest/userguide/monitoring.html#guide-monitoring)  

* **Workflows**  

 Simple and complex workflows can be composed using a set of powerful primitives we call the “canvas”, including grouping, chaining, chunking and more.

 [Read more…](http://docs.celeryproject.org/en/latest/userguide/canvas.html#guide-canvas).

* **Time & Rate Limits**

 You can control how many tasks can be executed per second/minute/hour, or how long a task can be allowed to run, and this can be set as a default, for a specific worker or individually for each task type.

 [Read more…](http://docs.celeryproject.org/en/latest/userguide/workers.html#worker-time-limits).
 
* **Scheduling**

 You can specify the time to run a task in seconds or a [datetime](http://docs.python.org/dev/library/datetime.html#datetime.datetime), or or you can use periodic tasks for recurring events based on a simple interval, or crontab expressions supporting minute, hour, day of week, day of month, and month of year.

 [Read more…](http://docs.celeryproject.org/en/latest/userguide/periodic-tasks.html#guide-beat).