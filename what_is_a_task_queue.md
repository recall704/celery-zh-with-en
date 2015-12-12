# What is a Task Queue?

Task queues are used as a mechanism to distribute work across threads or machines.  

A task queue’s input is a unit of work called a task. Dedicated worker processes constantly monitor task queues for new work to perform.  

Celery communicates via messages, usually using a broker to mediate between clients and workers. To initiate a task, a client adds a message to the queue, which the broker then delivers to a worker.  

A Celery system can consist of multiple workers and brokers, giving way to high availability and horizontal scaling.  

Celery is written in Python, but the protocol can be implemented in any language. So far there’s [RCelery](http://leapfrogdevelopment.github.com/rcelery/) for the Ruby programming language, [node-celery](https://github.com/mher/node-celery) for Node.js and a [PHP client](https://github.com/gjedeer/celery-php). Language interoperability can also be achieved by [using webhooks](http://docs.celeryproject.org/en/latest/userguide/remote-tasks.html#guide-webhooks).  


# 什么是任务队列?

任务队列是一种在线程或机器间分发任务的机制。

消息队列的输入是工作的一个单元，称为任务，独立的职程（Worker）进程持续监视队列中是否有需要处理的新任务。

Celery 用消息通信，通常使用中间人（Broker）在客户端和职程间斡旋。这个过程从客户端向队列添加消息开始，之后中间人把消息派送给职程。

Celery 系统可包含多个职程和中间人，以此获得高可用性和横向扩展能力。

Celery 是用 Python 编写的，但协议可以用任何语言实现。迄今，已有 Ruby 实现的 RCelery 、node.js 实现的 node-celery 以及一个 PHP 客户端 ，语言互通也可以通过 using webhooks 实现。