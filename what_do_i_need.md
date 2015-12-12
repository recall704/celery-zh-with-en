# What do I need?

Celery requires a message transport to send and receive messages. The RabbitMQ and Redis broker transports are feature complete, but there’s also support for a myriad of other experimental solutions, including using SQLite for local development.   

Celery can run on a single machine, on multiple machines, or even across data centers.    


  Version Requirements
  Celery version 3.0 runs on  
  * Python ❨2.5, 2.6, 2.7, 3.2, 3.3❩  
  * PyPy ❨1.8, 1.9❩  
  * Jython ❨2.5, 2.7❩

  This is the last version to support Python 2.5, and from the next version Python 2.6 or newer is required. The last version to support Python 2.4 was Celery series 2.2.  
