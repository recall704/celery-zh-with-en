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


# 框架集成

Celery 易于与 Web 框架集成，其中的一些甚至已经有了集成包：

| Django | django-celery |
| -- | -- |
| Pyramid | pyramid_celery |
| Pylons | celery-pylons |
| Flask | not needed |
| web2py | web2py-celery |
| Tornado | tornado-celery |

集成包并非是严格必要的，但它们让开发更简便，并且有时它们在 fork(2) 上添加了比如关闭数据库连接这样的重要回调.