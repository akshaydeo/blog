---
title: Installing Sentry on ubuntu with nginx
author: Deo Akshay
layout: post
permalink: /installing-sentry-on-ubuntu-with-nginx/
dsq_thread_id:
  - 1840972547
categories:
  - Django
  - Web Apps
---
For the past few weeks I am finalizing my web tech-stack for [Appsurfer][1]. My current stack is Ubuntu, Django, Nginx, Gunicorn,Fabric and Sentry. Till now I was using my own custom logging framework for logging and error tracking in web app. But as my app started growing it became really difficult to hit on some issues quickly, and it was not affordable to invest time in building a full fledged logging system and error tracking system.

There are a few paid options for this like [new-relic][2] but their free version is really not that useful for tracking error in web app. So I searched a bit and came across [Sentry][3]. A neat and clean system as per my requirements.Setting up sentry is too easy ( there is documentation but I and many people faced some issues while installing it in server-client mode ).

  1. Install Senry
<pre>easy_install -U sentry</pre>

  2. Initialise config file
<pre>sentry init /home/akshay/CodeBase/sentry/sentry.conf.py</pre>

  3. Modify config file
There are some issues while performing migrations using mysql so I preferred using postgresql

<pre>import os.path

CONF_ROOT = os.path.dirname(__file__)

DATABASES = {
    'default': {
        # You can swap out the engine for MySQL easily by changing this value
        # to ``django.db.backends.mysql`` or to PostgreSQL with
        # ``django.db.backends.postgresql_psycopg2``
        'ENGINE'   : 'django.db.backends.postgresql_psycopg2',
        'NAME'     : 'database_name',
        'USER'     : 'user_name',
        'PASSWORD' : 'password',
        'HOST'     : 'host',
        'PORT'     : '',
    }
}

SENTRY_KEY = 'ZB5PJkwJWaEcSOrD3eXC05OYa18aP6DRJxO7M+L8Af+tCnnjBP/s0A=='

# Set this to false to require authentication
SENTRY_PUBLIC = True

# You should configure the absolute URI to Sentry. It will attempt to guess it if you don't
# but proxies may interfere with this.
# SENTRY_URL_PREFIX = 'http://sentry.example.com'  # No trailing slash!

SENTRY_WEB_HOST = '0.0.0.0'
SENTRY_WEB_PORT = 9000
SENTRY_WEB_OPTIONS = {
    'workers': 3,  # the number of gunicorn workers
    # 'worker_class': 'gevent',
}

# Mail server configuration

# For more information check Django's documentation:
#  https://docs.djangoproject.com/en/1.3/topics/email/?from=olddocs#e-mail-backends

EMAIL_BACKEND = 'django.core.mail.backends.smtp.EmailBackend'

EMAIL_HOST = 'localhost'
EMAIL_HOST_PASSWORD = ''
EMAIL_HOST_USER = ''
EMAIL_PORT = 25
EMAIL_USE_TLS = False

</pre>

  4. Then perform upgrade to syncup db
<pre>sentry --config=sentry.conf.py upgrade</pre>

  5. Sentry by default runs on port 9000. Nginx conf for that
<pre>server {
    listen 80;
    server_name local.sentry.com;
    # no security problem here, since / is alway passed to upstream
    
    location /admin/media/ {
    # this changes depending on your python version
	root /usr/lib/python2.7/site-packages/django/contrib;
    }
    location / {
        proxy_pass_header Server;
        proxy_set_header Host $http_host;
        proxy_redirect off;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Scheme $scheme;
        proxy_connect_timeout 50;
        proxy_read_timeout 50;
        proxy_pass http://localhost:9000/;
    }
}
</pre>

  6. Restart nginx
<pre>/etc/init.d/nginx restart </pre>

  7. start sentry
<pre>sentry --config=sentry.conf.py start </pre>

  8. Here it is 
[<img src="/images/Sentry-1024x528.png" alt="" title="Sentry" width="1024" height="528" class="aligncenter size-large wp-image-107" />][4] </ol> 

Please feel free to contact me if something is missing or misleading :).

 [1]: http://appsurfer.com
 [2]: https://newrelic.com/
 [3]: https://github.com/dcramer/sentry
 [4]: http://blog.akshaydeo.me/wp-content/uploads/2012/05/Sentry.png