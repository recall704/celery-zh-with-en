# Features

* **Monitoring**  
 A stream of monitoring events is emitted by workers and is used by built-in and external tools to tell you what your cluster is doing – in real-time.  
 
 [Read more….](http://docs.celeryproject.org/en/latest/userguide/monitoring.html#guide-monitoring)  

* **Workflows**  

 Simple and complex workflows can be composed using a set of powerful primitives we call the “canvas”, including grouping, chaining, chunking and more.

 [Read more…](http://docs.celeryproject.org/en/latest/userguide/canvas.html#guide-canvas).

* **Time & Rate Limits**

 You can control how many tasks can be executed per second/minute/hour, or how long a task can be allowed to run, and this can be set as a default, for a specific worker or individually for each task type.

 [Read more…](http://docs.celeryproject.org/en/latest/userguide/workers.html#worker-time-limits).
 
* **Scheduling**

 You can specify the time to run a task in seconds or a [datetime](http://docs.python.org/dev/library/datetime.html#datetime.datetime), or or you can use periodic tasks for recurring events based on a simple interval, or crontab expressions supporting minute, hour, day of week, day of month, and month of year.

 [Read more…](http://docs.celeryproject.org/en/latest/userguide/periodic-tasks.html#guide-beat).
 
* **Autoreloading**

 In development workers can be configured to automatically reload source code as it changes, including *inotify(7)* support on Linux.

 [Read more…](http://docs.celeryproject.org/en/latest/userguide/workers.html#worker-autoreloading).
 
* **Autoscaling**

 Dynamically resizing the worker pool depending on load, or custom metrics specified by the user, used to limit memory usage in shared hosting/cloud environments or to enforce a given quality of service.

 [Read more…](http://docs.celeryproject.org/en/latest/userguide/workers.html#worker-autoscaling).
 
* **Resource Leak Protection**

 The --maxtasksperchild option is used for user tasks leaking resources, like memory or file descriptors, that are simply out of your control.

 [Read more…](http://docs.celeryproject.org/en/latest/userguide/workers.html#worker-maxtasksperchild).
 
* **User Components**

 Each worker component can be customized, and additional components can be defined by the user. The worker is built up using “bootsteps” — a dependency graph enabling fine grained control of the worker’s internals.  