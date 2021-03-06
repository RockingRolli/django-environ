Django-environ
==============

Django-environ allows you to utilize 12factor inspired environment variables to configure your Django application.
This is your `settings.py` file before you have installed **django-envronment**::

    import os
    SITE_ROOT = os.path.dirname(os.path.dirname(os.path.dirname(os.path.realpath(__file__))))

    DEBUG = True
    TEMPLATE_DEBUG = DEBUG

    DATABASES = {
        'default': {
            'ENGINE': 'django.db.backends.postgresql_psycopg2',
            'NAME': 'database',
            'USER': 'user',
            'PASSWORD': 'githubbedpassword',
            'HOST': '123.123.123.123',
            'PORT': '8458',
        }
        'extra': {
            'ENGINE': 'django.db.backends.sqlite3',
            'NAME': os.path.join(SITE_ROOT, 'database.sqlite')
        }
    }

    MEDIA_ROOT = os.path.join(SITE_ROOT, 'assets')
    MEDIA_URL = 'media/'
    STATIC_ROOT = os.path.join(SITE_ROOT, 'static')
    STATIC_URL = 'static/'

    SECRET_KEY = '...im incredibly still here...'

After::

    import environ
    root = environ.Path(__file__) - 3 # three folder back (/a/b/c/ - 3 = /)
    env = environ.Env(DEBUG=(bool, False),) # set default values and casting

    SITE_ROOT = root()

    DEBUG = env('DEBUG') # False if not in os.environ
    TEMPLATE_DEBUG = DEBUG

    DATABASES = {
        'default': env.db(), # Raises ImproperlyConfigured exception if DATABASE_URL not in os.environ
        'extra': env.db('SQLITE_DB_URL', default=root('database.sqlite'))
    }

    MEDIA_ROOT = root('assets')
    MEDIA_URL = 'media/'
    STATIC_ROOT = root('static')
    STATIC_URL = 'static/'

    SECRET_KEY = env('SECRET_KEY') # Raises ImproperlyConfigured exception if SECRET_KEY not in os.environ

How to install
--------------

::

    $ pip install django-environ

Supported Databases
-------------------

-  PostgreSQL: postgres:// or postgresql://
-  PostGIS: postgis://
-  MySQL: mysql://
-  MySQL for GeoDjango: mysqlgis://
-  SQLITE: sqlite://

DevMode
-------

Put your environment variables definition in a `.env` file::

    $ cat >.env <<EOM
    DEBUG=on
    SECRET_KEY=1110110010111101111011101111
    DATABASE_URL=postgres://uf07k1:wegauwhg@compute-1.amazonaws.com:5431/d8r827
    SQLITE_URL=sqlite:////var/db/myapp.db
    EOM

Then in your `settings.py` or `settings/dev.py`::

    import environ
    root = environ.Path(__file__) - 3
    env = environ.Env()

    env.read_env(root('.env'))

    ...

Tests
-----

::

    $ git clone git@github.com:joke2k/django-environ.git
    $ cd django-environ/
    $ python setup.py test


Changelog
---------

=== 0.2.1 (2013-04-19) ===

  * environ/environ.py: Env.__call__ now uses Env.get_value instance method

=== 0.2 (2013-04-16) ===

  * environ/environ.py, environ/test.py, environ/test_env.txt: add advanced
    float parsing (comma and dot symbols to separate thousands and decimals)

  * README.rst, docs/index.rst: fix TYPO in documentation

=== 0.1 (2013-04-02) ===

  * initial release

Credits
-------

- `12factor`_
- `12factor-django`_
- `Two Scoops of Django`_
- `rconradharris`_ / `envparse`_
- `kennethreitz`_ / `dj-database-url`_
- `nickstenning`_ / `honcho`_
- `envparse`_
- `Distribute`_
- `modern-package-template`_

.. _rconradharris: https://github.com/rconradharris
.. _envparse: https://github.com/rconradharris/envparse

.. _kennethreitz: https://github.com/kennethreitz
.. _dj-database-url: https://github.com/kennethreitz/dj-database-url

.. _nickstenning: https://github.com/nickstenning
.. _honcho: https://github.com/nickstenning/honcho

.. _12factor: http://www.12factor.net/
.. _12factor-django: http://www.wellfireinteractive.com/blog/easier-12-factor-django/
.. _`Two Scoops of Django`: https://django.2scoops.org (book)


.. _Distribute: http://pypi.python.org/pypi/distribute
.. _`modern-package-template`: http://pypi.python.org/pypi/modern-package-template
