# Next Steps

The [First Steps with Celery](http://docs.celeryproject.org/en/latest/getting-started/first-steps-with-celery.html#first-steps) guide is intentionally minimal. In this guide I will demonstrate what Celery offers in more detail, including how to add Celery support for your application and library.

This document does not document all of Celery’s features and best practices, so it’s recommended that you also read the [User Guide](http://docs.celeryproject.org/en/latest/userguide/index.html#guide)

* Using Celery in your Application  
* Calling Tasks  
* Canvas: Designing Workflows  
* Routing  
* Remote Control  
* Timezone  
* Optimization  
* What to do now?  


## Using Celery in your Application

### Our Project

Project layout:

```
proj/__init__.py
    /celery.py
    /tasks.py
```
proj/celery.py
```
from __future__ import absolute_import

from celery import Celery

app = Celery('proj',
             broker='amqp://',
             backend='amqp://',
             include=['proj.tasks'])

# Optional configuration, see the application user guide.
app.conf.update(
    CELERY_TASK_RESULT_EXPIRES=3600,
)

if __name__ == '__main__':
    app.start()
```
In this module you created our [Celery](http://docs.celeryproject.org/en/latest/reference/celery.html#celery.Celery) instance (sometimes referred to as the app). To use Celery within your project you simply import this instance.

* The `broker` argument specifies the URL of the broker to use.

 See [Choosing a Broker](http://docs.celeryproject.org/en/latest/getting-started/first-steps-with-celery.html#celerytut-broker) for more information.

* The `backend` argument specifies the result backend to use,

 It’s used to keep track of task state and results. While results are disabled by default I use the amqp result backend here because I demonstrate how retrieving results work later, you may want to use a different backend for your application. They all have different strengths and weaknesses. If you don’t need results it’s better to disable them. Results can also be disabled for individual tasks by setting the `@task(ignore_result=True)` option.

 See [Keeping Results](http://docs.celeryproject.org/en/latest/getting-started/first-steps-with-celery.html#celerytut-keeping-results) for more information.

* The `include` argument is a list of modules to import when the worker starts. You need to add our tasks module here so that the worker is able to find our tasks.

proj/tasks.py
```
from __future__ import absolute_import

from proj.celery import app


@app.task
def add(x, y):
    return x + y


@app.task
def mul(x, y):
    return x * y


@app.task
def xsum(numbers):
    return sum(numbers)
```


## Starting the worker

The **celery** program can be used to start the worker (you need to run the worker in the directory above proj):

```
$ celery -A proj worker -l info
```
When the worker starts you should see a banner and some messages:

```
-------------- celery@halcyon.local v3.1 (Cipater)
---- **** -----
--- * ***  * -- [Configuration]
-- * - **** --- . broker:      amqp://guest@localhost:5672//
- ** ---------- . app:         __main__:0x1012d8590
- ** ---------- . concurrency: 8 (processes)
- ** ---------- . events:      OFF (enable -E to monitor this worker)
- ** ----------
- *** --- * --- [Queues]
-- ******* ---- . celery:      exchange:celery(direct) binding:celery
--- ***** -----

[2012-06-08 16:23:51,078: WARNING/MainProcess] celery@halcyon.local has started.
```
– The *broker* is the URL you specifed in the broker argument in our `celery` module, you can also specify a different broker on the command-line by using the `-b` option.

– *Concurrency* is the number of prefork worker process used to process your tasks concurrently, when all of these are busy doing work new tasks will have to wait for one of the tasks to finish before it can be processed.

The default concurrency number is the number of CPU’s on that machine (including cores), you can specify a custom number using -c option. There is no recommended value, as the optimal number depends on a number of factors, but if your tasks are mostly I/O-bound then you can try to increase it, experimentation has shown that adding more than twice the number of CPU’s is rarely effective, and likely to degrade performance instead.

Including the default prefork pool, Celery also supports using Eventlet, Gevent, and threads (see [Concurrency](http://docs.celeryproject.org/en/latest/userguide/concurrency/index.html#concurrency)).

– *Events* is an option that when enabled causes Celery to send monitoring messages (events) for actions occurring in the worker. These can be used by monitor programs like `celery events`, and Flower - the real-time Celery monitor, which you can read about in the [Monitoring and Management guide](http://docs.celeryproject.org/en/latest/userguide/monitoring.html#guide-monitoring).

– *Queues* is the list of queues that the worker will consume tasks from. The worker can be told to consume from several queues at once, and this is used to route messages to specific workers as a means for Quality of Service, separation of concerns, and emulating priorities, all described in the [Routing Guide](http://docs.celeryproject.org/en/latest/userguide/routing.html#guide-routing).

You can get a complete list of command-line arguments by passing in the *`–help`* flag:

```
$ celery worker --help
```
These options are described in more detailed in the [Workers Guide](http://docs.celeryproject.org/en/latest/userguide/workers.html#guide-workers).


#### Stopping the worker   
To stop the worker simply hit Ctrl+C. A list of signals supported by the worker is detailed in the [Workers Guide](http://docs.celeryproject.org/en/latest/userguide/workers.html#guide-workers).

#### In the background  

In production you will want to run the worker in the background, this is described in detail in the [daemonization tutorial](http://docs.celeryproject.org/en/latest/tutorials/daemonizing.html#daemonizing).

The daemonization scripts uses the **celery multi** command to start one or more workers in the background:

```
$ celery multi start w1 -A proj -l info
celery multi v3.1.1 (Cipater)
> Starting nodes...
    > w1.halcyon.local: OK
```

You can restart it too:

```
$ celery  multi restart w1 -A proj -l info
celery multi v3.1.1 (Cipater)
> Stopping nodes...
    > w1.halcyon.local: TERM -> 64024
> Waiting for 1 node.....
    > w1.halcyon.local: OK
> Restarting node w1.halcyon.local: OK
celery multi v3.1.1 (Cipater)
> Stopping nodes...
    > w1.halcyon.local: TERM -> 64052
```

or stop it:

```
$ celery multi stop w1 -A proj -l info
```

The `stop` command is asynchronous so it will not wait for the worker to shutdown. You will probably want to use the `stopwait` command instead which will ensure all currently executing tasks is completed:

```
$ celery multi stopwait w1 -A proj -l info
```

> Note  
**celery multi** doesn’t store information about workers so you need to use the same command-line arguments when restarting. Only the same pidfile and logfile arguments must be used when stopping.  

By default it will create pid and log files in the current directory, to protect against multiple workers launching on top of each other you are encouraged to put these in a dedicated directory:

```
$ mkdir -p /var/run/celery
$ mkdir -p /var/log/celery
$ celery multi start w1 -A proj -l info --pidfile=/var/run/celery/%n.pid \
                                        --logfile=/var/log/celery/%n%I.log
```

