# Framework Integration

Celery is easy to integrate with web frameworks, some of which even have integration packages:  

| Django | django-celery |
| -- | -- |
| Pyramid | pyramid_celery |
| Pylons | celery-pylons |
| Flask | not needed |
| web2py | web2py-celery |
| Tornado | tornado-celery |


The integration packages are not strictly necessary, but they can make development easier, and sometimes they add important hooks like closing database connections at fork(2).  