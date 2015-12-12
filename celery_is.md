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


# 什么是celery

* 简单

　 Celery 易于使用和维护，并且它 不需要配置文件 。

　 Celery 有一个活跃、友好的社区来让你寻求帮助，包括一个 邮件列表 和一个 IRC 频道 。

　下面是一个你可以实现的最简应用：

    ```
    from celery import Celery
    
    app = Celery('hello', broker='amqp://guest@localhost//')
    
    @app.task
    def hello():
        return 'hello world'
    ```
* 高可用性  

  倘若连接丢失或失败，职程和客户端会自动重试，并且一些中间人通过 主/主 或 主/从 方式复制来提高可用性.


* 快速

 单个 Celery 进程每分钟可处理数以百万计的任务，而保持往返延迟在亚毫秒级（使用 RabbitMQ、py-librabbitmq 和优化过的设置）

* 灵活

 Celery 几乎所有部分都可以扩展或单独使用。可以自制连接池、 序列化、压缩模式、日志、调度器、消费者、生产者、自动扩展、 中间人传输或更多
 
 
**支持的功能**
* 中间人

 [RabbitMQ](http://docs.celeryproject.org/en/latest/getting-started/brokers/rabbitmq.html#broker-rabbitmq), [Redis](http://docs.celeryproject.org/en/latest/getting-started/brokers/redis.html#broker-redis),  
 [MongoDB](http://docs.celeryproject.org/en/latest/getting-started/brokers/mongodb.html#broker-mongodb) (实验性), ZeroMQ (实验性)  
 [CouchDB](http://docs.celeryproject.org/en/latest/getting-started/brokers/couchdb.html#broker-couchdb) (实验性), [SQLAlchemy](http://docs.celeryproject.org/en/latest/getting-started/brokers/sqlalchemy.html#broker-sqlalchemy) (实验性)  
 [Django ORM](http://docs.celeryproject.org/en/latest/getting-started/brokers/django.html#broker-django) (实验性), [Amazon SQS](http://docs.celeryproject.org/en/latest/getting-started/brokers/sqs.html#broker-sqs), (实验性)  
 更多...

* 并发

 prefork (多进程),  
 [Eventlet](http://eventlet.net/), [gevent](http://gevent.org/)  
 多线程/单线程

* 结果存储

 AMQP, Redis 
 memcached, MongoDB  
 SQLAlchemy, Django ORM  
 Apache Cassandra  

* 序列化

 pickle, json, yaml, msgpack  
 zlib, bzip2 压缩  
 密码学消息签名  


# 特性

* 监视

 整条流水线的监视时间由职程发出，并用于内建或外部的工具告知你集群的工作状况——而且是实时的。

 深入了解….

* 工作流

 一系列功能强大的称为“Canvas”的原语（Primitive）用于构建或简单、或复杂的工作流。包括分组、连锁、分割等等。

 深入了解….

* 时间和速率限制

 你可以控制每秒/分钟/小时执行的任务数，或任务的最长运行时间， 并且可以为特定职程或不同类型的任务设置为默认值。

 深入了解….

* 计划任务

 你可以指定任务在若干秒后或在 datetime 运行，或你可以基于单纯的时间间隔或支持分钟、小时、每周的第几天、每月的第几天以及每年的第几月的 crontab 表达式来使用周期任务来重现事件。

 深入了解….
 
* 自动重载入

 在开发中，职程可以配置为在源码修改时自动重载入，包含对 Linux 上的 inotify(7) 支持。

 深入了解….

* 自动扩展

 根据负载自动重调职程池的大小或用户指定的测量值，用于限制共享主机/云环境的内存使用，或是保证给定的服务质量。

 深入了解….
 
* 资源泄露保护

 --maxtasksperchild 选项用于控制用户任务泄露的诸如内存或文件描述符这些易超出掌控的资源。

 深入了解….
* 用户组件

 每个职程组件都可以自定义，并且额外组件可以由用户定义。职程是用 “bootsteps” 构建的——一个允许细粒度控制职程内构件的依赖图。