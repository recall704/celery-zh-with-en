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
