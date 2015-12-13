# Calling Tasks

You can call a task using the `delay()` method:

```
>>> add.delay(2, 2)
```

This method is actually a star-argument shortcut to another method called `apply_async()`:

```
>>> add.apply_async((2, 2))
```

The latter enables you to specify execution options like the time to run (countdown), the queue it should be sent to and so on:

```
>>> add.apply_async((2, 2), queue='lopri', countdown=10)
```

In the above example the task will be sent to a queue named `lopri` and the task will execute, at the earliest, 10 seconds after the message was sent.

Applying the task directly will execute the task in the current process, so that no message is sent:

```
>>> add(2, 2)
4
```

These three methods `- delay()`, `apply_async()`, and applying (`__call__`), represents the Celery calling API, which are also used for subtasks.

A more detailed overview of the Calling API can be found in the [Calling User Guide](http://docs.celeryproject.org/en/latest/userguide/calling.html#guide-calling).

Every task invocation will be given a unique identifier (an UUID), this is the task id.

The `delay` and `apply_async` methods return an [AsyncResult](http://docs.celeryproject.org/en/latest/reference/celery.result.html#celery.result.AsyncResult) instance, which can be used to keep track of the tasks execution state. But for this you need to enable a [result backend](http://docs.celeryproject.org/en/latest/userguide/tasks.html#task-result-backends) so that the state can be stored somewhere.

Results are disabled by default because of the fact that there is no result backend that suits every application, so to choose one you need to consider the drawbacks of each individual backend. For many tasks keeping the return value isn’t even very useful, so it’s a sensible default to have. Also note that result backends are not used for monitoring tasks and workers, for that Celery uses dedicated event messages (see [Monitoring and Management Guide](http://docs.celeryproject.org/en/latest/userguide/monitoring.html#guide-monitoring)).

If you have a result backend configured you can retrieve the return value of a task:

```
>>> res = add.delay(2, 2)
>>> res.get(timeout=1)
4
```

You can find the task’s id by looking at the id attribute:

```
>>> res.id
d6b3aea2-fb9b-4ebc-8da4-848818db9114
```

You can also inspect the exception and traceback if the task raised an exception, in fact `result.get()` will propagate any errors by default:

>>> res = add.delay(2)
>>> res.get(timeout=1)
Traceback (most recent call last):
File "<stdin>", line 1, in <module>
File "/opt/devel/celery/celery/result.py", line 113, in get
    interval=interval)
File "/opt/devel/celery/celery/backends/amqp.py", line 138, in wait_for
    raise meta['result']
TypeError: add() takes exactly 2 arguments (1 given)
If you don’t wish for the errors to propagate then you can disable that by passing the propagate argument:

>>> res.get(propagate=False)
TypeError('add() takes exactly 2 arguments (1 given)',)
In this case it will return the exception instance raised instead, and so to check whether the task succeeded or failed you will have to use the corresponding methods on the result instance:

>>> res.failed()
True

>>> res.successful()
False
So how does it know if the task has failed or not? It can find out by looking at the tasks state:

>>> res.state
'FAILURE'
A task can only be in a single state, but it can progress through several states. The stages of a typical task can be:

PENDING -> STARTED -> SUCCESS
The started state is a special state that is only recorded if the CELERY_TRACK_STARTED setting is enabled, or if the @task(track_started=True) option is set for the task.

The pending state is actually not a recorded state, but rather the default state for any task id that is unknown, which you can see from this example:

>>> from proj.celery import app

>>> res = app.AsyncResult('this-id-does-not-exist')
>>> res.state
'PENDING'
If the task is retried the stages can become even more complex, e.g, for a task that is retried two times the stages would be:

PENDING -> STARTED -> RETRY -> STARTED -> RETRY -> STARTED -> SUCCESS
To read more about task states you should see the States section in the tasks user guide.

Calling tasks is described in detail in the Calling Guide.