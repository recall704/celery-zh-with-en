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