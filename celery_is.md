# Celery is…

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
