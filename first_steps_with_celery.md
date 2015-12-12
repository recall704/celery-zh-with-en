# First Steps with Celery

Celery is a task queue with batteries included. It is easy to use so that you can get started without learning the full complexities of the problem it solves. It is designed around best practices so that your product can scale and integrate with other languages, and it comes with the tools and support you need to run such a system in production.

In this tutorial you will learn the absolute basics of using Celery. You will learn about;

* Choosing and installing a message transport (broker).  
* Installing Celery and creating your first task.  
* Starting the worker and calling tasks.  
* Keeping track of tasks as they transition through different states, and inspecting return values.  

Celery may seem daunting at first - but don’t worry - this tutorial will get you started in no time. It is deliberately kept simple, so to not confuse you with advanced features. After you have finished this tutorial it’s a good idea to browse the rest of the documentation, for example the [Next Steps](http://docs.celeryproject.org/en/latest/getting-started/next-steps.html#next-steps) tutorial, which will showcase Celery’s capabilities.

* Choosing a Broker  
 * RabbitMQ  
 * Redis  
 * Using a database  
 * Other brokers  
* Installing Celery  
* Application  
* Running the celery worker server  
* Calling the task  
* Keeping Results  
* Configuration  
* Where to go from here  
* Troubleshooting  
 * Worker does not start: Permission Error  
 * Result backend does not work or tasks are always in PENDING state.  

Choosing a Broker
Celery requires a solution to send and receive messages; usually this comes in the form of a separate service called a message broker.

There are several choices available, including:

RabbitMQ
RabbitMQ is feature-complete, stable, durable and easy to install. It’s an excellent choice for a production environment. Detailed information about using RabbitMQ with Celery:

Using RabbitMQ
If you are using Ubuntu or Debian install RabbitMQ by executing this command:

$ sudo apt-get install rabbitmq-server
When the command completes the broker is already running in the background, ready to move messages for you: Starting rabbitmq-server: SUCCESS.

And don’t worry if you’re not running Ubuntu or Debian, you can go to this website to find similarly simple installation instructions for other platforms, including Microsoft Windows:

http://www.rabbitmq.com/download.html
Redis
Redis is also feature-complete, but is more susceptible to data loss in the event of abrupt termination or power failures. Detailed information about using Redis:

Using Redis
Using a database
Using a database as a message queue is not recommended, but can be sufficient for very small installations. Your options include:

Using SQLAlchemy
Using the Django Database
If you’re already using a Django database for example, using it as your message broker can be convenient while developing even if you use a more robust system in production.

Other brokers
In addition to the above, there are other experimental transport implementations to choose from, including Amazon SQS, Using MongoDB and IronMQ.

See Broker Overview for a full list.

Installing Celery
Celery is on the Python Package Index (PyPI), so it can be installed with standard Python tools like pip or easy_install:

$ pip install celery
Application
The first thing you need is a Celery instance, which is called the celery application or just “app” for short. Since this instance is used as the entry-point for everything you want to do in Celery, like creating tasks and managing workers, it must be possible for other modules to import it.

In this tutorial you will keep everything contained in a single module, but for larger projects you want to create a dedicated module.

Let’s create the file tasks.py:

from celery import Celery

app = Celery('tasks', broker='amqp://guest@localhost//')

@app.task
def add(x, y):
    return x + y
The first argument to Celery is the name of the current module, this is needed so that names can be automatically generated, the second argument is the broker keyword argument which specifies the URL of the message broker you want to use, using RabbitMQ here, which is already the default option. See Choosing a Broker above for more choices, e.g. for RabbitMQ you can use amqp://localhost, or for Redis you can use redis://localhost.

You defined a single task, called add, which returns the sum of two numbers.

Running the celery worker server
You now run the worker by executing our program with the worker argument:

$ celery -A tasks worker --loglevel=info
Note
See the Troubleshooting section if the worker does not start.
In production you will want to run the worker in the background as a daemon. To do this you need to use the tools provided by your platform, or something like supervisord (see Running the worker as a daemon for more information).

For a complete listing of the command-line options available, do:

$  celery worker --help
There are also several other commands available, and help is also available:

$ celery help
Calling the task
To call our task you can use the delay() method.

This is a handy shortcut to the apply_async() method which gives greater control of the task execution (see Calling Tasks):

>>> from tasks import add
>>> add.delay(4, 4)
The task has now been processed by the worker you started earlier, and you can verify that by looking at the workers console output.

Calling a task returns an AsyncResult instance, which can be used to check the state of the task, wait for the task to finish or get its return value (or if the task failed, the exception and traceback). But this isn’t enabled by default, and you have to configure Celery to use a result backend, which is detailed in the next section.

Keeping Results
If you want to keep track of the tasks’ states, Celery needs to store or send the states somewhere. There are several built-in result backends to choose from: SQLAlchemy/Django ORM, Memcached, Redis, AMQP (RabbitMQ), and MongoDB – or you can define your own.

For this example you will use the rpc result backend, which sends states back as transient messages. The backend is specified via the backend argument to Celery, (or via the CELERY_RESULT_BACKEND setting if you choose to use a configuration module):

app = Celery('tasks', backend='rpc://', broker='amqp://')
Or if you want to use Redis as the result backend, but still use RabbitMQ as the message broker (a popular combination):

app = Celery('tasks', backend='redis://localhost', broker='amqp://')
To read more about result backends please see Result Backends.

Now with the result backend configured, let’s call the task again. This time you’ll hold on to the AsyncResult instance returned when you call a task:

>>> result = add.delay(4, 4)
The ready() method returns whether the task has finished processing or not:

>>> result.ready()
False
You can wait for the result to complete, but this is rarely used since it turns the asynchronous call into a synchronous one:

>>> result.get(timeout=1)
8
In case the task raised an exception, get() will re-raise the exception, but you can override this by specifying the propagate argument:

>>> result.get(propagate=False)
If the task raised an exception you can also gain access to the original traceback:

>>> result.traceback
…
See celery.result for the complete result object reference.

Configuration
Celery, like a consumer appliance, doesn’t need much to be operated. It has an input and an output, where you must connect the input to a broker and maybe the output to a result backend if so wanted. But if you look closely at the back there’s a lid revealing loads of sliders, dials and buttons: this is the configuration.

The default configuration should be good enough for most uses, but there are many things to tweak so Celery works just the way you want it to. Reading about the options available is a good idea to get familiar with what can be configured. You can read about the options in the Configuration and defaults reference.

The configuration can be set on the app directly or by using a dedicated configuration module. As an example you can configure the default serializer used for serializing task payloads by changing the CELERY_TASK_SERIALIZER setting:

app.conf.CELERY_TASK_SERIALIZER = 'json'
If you are configuring many settings at once you can use update:

app.conf.update(
    CELERY_TASK_SERIALIZER='json',
    CELERY_ACCEPT_CONTENT=['json'],  # Ignore other content
    CELERY_RESULT_SERIALIZER='json',
    CELERY_TIMEZONE='Europe/Oslo',
    CELERY_ENABLE_UTC=True,
)
For larger projects using a dedicated configuration module is useful, in fact you are discouraged from hard coding periodic task intervals and task routing options, as it is much better to keep this in a centralized location, and especially for libraries it makes it possible for users to control how they want your tasks to behave, you can also imagine your SysAdmin making simple changes to the configuration in the event of system trouble.

You can tell your Celery instance to use a configuration module, by calling the app.config_from_object() method:

app.config_from_object('celeryconfig')
This module is often called “celeryconfig”, but you can use any module name.

A module named celeryconfig.py must then be available to load from the current directory or on the Python path, it could look like this:

celeryconfig.py:

BROKER_URL = 'amqp://'
CELERY_RESULT_BACKEND = 'rpc://'

CELERY_TASK_SERIALIZER = 'json'
CELERY_RESULT_SERIALIZER = 'json'
CELERY_ACCEPT_CONTENT=['json']
CELERY_TIMEZONE = 'Europe/Oslo'
CELERY_ENABLE_UTC = True
To verify that your configuration file works properly, and doesn’t contain any syntax errors, you can try to import it:

$ python -m celeryconfig
For a complete reference of configuration options, see Configuration and defaults.

To demonstrate the power of configuration files, this is how you would route a misbehaving task to a dedicated queue:

celeryconfig.py:

CELERY_ROUTES = {
    'tasks.add': 'low-priority',
}
Or instead of routing it you could rate limit the task instead, so that only 10 tasks of this type can be processed in a minute (10/m):

celeryconfig.py:

CELERY_ANNOTATIONS = {
    'tasks.add': {'rate_limit': '10/m'}
}
If you are using RabbitMQ or Redis as the broker then you can also direct the workers to set a new rate limit for the task at runtime:

$ celery -A tasks control rate_limit tasks.add 10/m
worker@example.com: OK
    new rate limit set successfully
See Routing Tasks to read more about task routing, and the CELERY_ANNOTATIONS setting for more about annotations, or Monitoring and Management Guide for more about remote control commands, and how to monitor what your workers are doing.

Where to go from here
If you want to learn more you should continue to the Next Steps tutorial, and after that you can study the User Guide.

Troubleshooting
There’s also a troubleshooting section in the Frequently Asked Questions.

Worker does not start: Permission Error
If you’re using Debian, Ubuntu or other Debian-based distributions:

Debian recently renamed the /dev/shm special file to /run/shm.

A simple workaround is to create a symbolic link:

# ln -s /run/shm /dev/shm
Others:

If you provide any of the --pidfile, --logfile or --statedb arguments, then you must make sure that they point to a file/directory that is writable and readable by the user starting the worker.

Result backend does not work or tasks are always in PENDING state.
All tasks are PENDING by default, so the state would have been better named “unknown”. Celery does not update any state when a task is sent, and any task with no history is assumed to be pending (you know the task id after all).

Make sure that the task does not have ignore_result enabled.

Enabling this option will force the worker to skip updating states.

Make sure the CELERY_IGNORE_RESULT setting is not enabled.

Make sure that you do not have any old workers still running.

It’s easy to start multiple workers by accident, so make sure that the previous worker is properly shutdown before you start a new one.

An old worker that is not configured with the expected result backend may be running and is hijacking the tasks.

The –pidfile argument can be set to an absolute path to make sure this doesn’t happen.

Make sure the client is configured with the right backend.

If for some reason the client is configured to use a different backend than the worker, you will not be able to receive the result, so make sure the backend is correct by inspecting it:

>>> result = task.delay(…)
>>> print(result.backend)