# Timezone

All times and dates, internally and in messages uses the UTC timezone.

When the worker receives a message, for example with a countdown set it converts that UTC time to local time. If you wish to use a different timezone than the system timezone then you must configure that using the [CELERY_TIMEZONE](http://docs.celeryproject.org/en/latest/configuration.html#std:setting-CELERY_TIMEZONE) setting:

```
app.conf.CELERY_TIMEZONE = 'Europe/London'
```