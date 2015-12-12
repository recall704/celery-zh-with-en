# What do I need?

Celery requires a message transport to send and receive messages. The RabbitMQ and Redis broker transports are feature complete, but there’s also support for a myriad of other experimental solutions, including using SQLite for local development.   

Celery can run on a single machine, on multiple machines, or even across data centers.    


>  Version Requirements
  Celery version 3.0 runs on  
  * Python ❨2.5, 2.6, 2.7, 3.2, 3.3❩  
  * PyPy ❨1.8, 1.9❩  
  * Jython ❨2.5, 2.7❩

>  This is the last version to support Python 2.5, and from the next version Python 2.6 or newer is required. The last version to support Python 2.4 was Celery series 2.2.  



# 运行条件

Celery 需要一个发送和接受消息的传输者。RabbitMQ 和 Redis 中间人的消息传输支持所有特性，但也提供大量其他实验性方案的支持，包括用 SQLite 进行本地开发.

Celery 可以单机运行，也可以在多台机器上运行，甚至可以跨越数据中心运行.

> 版本需求

 Celery 的 3.0 版本可运行在

 * Python ❨2.5, 2.6, 2.7, 3.2, 3.3❩  
 * PyPy ❨1.8, 1.9❩  
 * Jython ❨2.5, 2.7❩.  
 这是最后一个支持 Python 2.5 的版本，也即从下个版本需要 Python 2.6 或更新版本的 Python。最后一个支持 Python 2.4 的版本为 Celery 2.2 系列。