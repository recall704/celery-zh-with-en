# Optimization

The default configuration is not optimized for throughput by default, it tries to walk the middle way between many short tasks and fewer long tasks, a compromise between throughput and fair scheduling.

If you have strict fair scheduling requirements, or want to optimize for throughput then you should read the [Optimizing Guide](http://docs.celeryproject.org/en/latest/userguide/optimizing.html#guide-optimizing).

If you’re using RabbitMQ then you should install the **librabbitmq** module, which is an AMQP client implemented in C:

```
$ pip install librabbitmq
```