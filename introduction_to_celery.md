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