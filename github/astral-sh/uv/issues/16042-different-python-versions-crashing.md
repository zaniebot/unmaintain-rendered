---
number: 16042
title: Different python versions crashing
type: issue
state: open
author: danielpassy
labels:
  - needs-mre
assignees: []
created_at: 2025-09-26T17:53:25Z
updated_at: 2025-12-18T15:29:44Z
url: https://github.com/astral-sh/uv/issues/16042
synced_at: 2026-01-07T13:12:19-06:00
---

# Different python versions crashing

---

_Issue opened by @danielpassy on 2025-09-26 17:53_

### Summary

I have a repo with multiple projects.
I has a pyproject.toml as it's root.

```
[project]
name = "daniel-passy-scripts-random"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.10"
dependencies = []
``` 


Inside ./projects there are many projects, one with python 3.12, the rest with 3.10

I used `uv venv env -p 3.10` to create the env and use `uv pip install` to install dependencies.

From time to time, I don't know how, I run into this issue:
```
 ⎿  Error: Traceback (most recent call last):
       File "/home/daniel/projects/eva/projects/eden-foundation/manage.py", line 22, in <module>
         main()
       File "/home/daniel/projects/eva/projects/eden-foundation/manage.py", line 18, in main
         execute_from_command_line(sys.argv)
       File "/home/daniel/projects/eva/projects/eden-foundation/env/lib/python3.12/site-packages/django/core/management/__init__.py", line 442, in 
     execute_from_command_line
         utility.execute()
       File "/home/daniel/projects/eva/projects/eden-foundation/env/lib/python3.12/site-packages/django/core/management/__init__.py", line 436, in 
     execute
         self.fetch_command(subcommand).run_from_argv(self.argv)
       File "/home/daniel/projects/eva/projects/eden-foundation/env/lib/python3.12/site-packages/django/core/management/commands/test.py", line 24, in 
     run_from_argv
         super().run_from_argv(argv)
       File "/home/daniel/projects/eva/projects/eden-foundation/env/lib/python3.12/site-packages/django/core/management/base.py", line 406, in 
     run_from_argv
         parser = self.create_parser(argv[0], argv[1])
                  ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
       File "/home/daniel/projects/eva/projects/eden-foundation/env/lib/python3.12/site-packages/django/core/management/base.py", line 370, in 
     create_parser
         self.add_arguments(parser)
       File "/home/daniel/projects/eva/projects/eden-foundation/env/lib/python3.12/site-packages/django/core/management/commands/test.py", line 49, in 
     add_arguments
         test_runner_class = get_runner(settings, self.test_runner)
                             ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
       File "/home/daniel/projects/eva/projects/eden-foundation/env/lib/python3.12/site-packages/django/test/utils.py", line 382, in get_runner
         test_runner_class = test_runner_class or settings.TEST_RUNNER
                                                  ^^^^^^^^^^^^^^^^^^^^
       File "/home/daniel/projects/eva/projects/eden-foundation/env/lib/python3.12/site-packages/django/conf/__init__.py", line 81, in __getattr__
         self._setup(name)
       File "/home/daniel/projects/eva/projects/eden-foundation/env/lib/python3.12/site-packages/django/conf/__init__.py", line 68, in _setup
         self._wrapped = Settings(settings_module)
                         ^^^^^^^^^^^^^^^^^^^^^^^^^
       File "/home/daniel/projects/eva/projects/eden-foundation/env/lib/python3.12/site-packages/django/conf/__init__.py", line 166, in __init__
         mod = importlib.import_module(self.SETTINGS_MODULE)
               ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
       File "/home/daniel/.local/share/uv/python/cpython-3.12.10-linux-x86_64-gnu/lib/python3.12/importlib/__init__.py", line 90, in import_module
         return _bootstrap._gcd_import(name[level:], package, level)
                ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
       File "<frozen importlib._bootstrap>", line 1387, in _gcd_import
       File "<frozen importlib._bootstrap>", line 1360, in _find_and_load
       File "<frozen importlib._bootstrap>", line 1331, in _find_and_load_unlocked
       File "<frozen importlib._bootstrap>", line 935, in _load_unlocked
       File "<frozen importlib._bootstrap_external>", line 999, in exec_module
       File "<frozen importlib._bootstrap>", line 488, in _call_with_frames_removed
       File "/home/daniel/projects/eva/projects/eden-foundation/app_manager/settings.py", line 18, in <module>
         from app_manager import env
       File "/home/daniel/projects/eva/projects/eden-foundation/app_manager/env.py", line 3, in <module>
         from app_manager.utils import get_cpus_per_task
       File "/home/daniel/projects/eva/projects/eden-foundation/app_manager/utils.py", line 5, in <module>
         import requests
       File "/home/daniel/projects/eva/projects/eden-foundation/env/lib/python3.12/site-packages/requests/__init__.py", line 45, in <module>
         from .exceptions import RequestsDependencyWarning
       File "/home/daniel/projects/eva/projects/eden-foundation/env/lib/python3.12/site-packages/requests/exceptions.py", line 9, in <module>
         from .compat import JSONDecodeError as CompatJSONDecodeError
     ImportError: cannot import name 'JSONDecodeError' from 'requests.compat' 
```

This happened because this requests is from python 3.10 and is in the project that's using 3.12

I even can delete the env, recreate it, and somehow it still install the wrong request.

```
"""
requests.compat
~~~~~~~~~~~~~~~

This module previously handled import compatibility issues
between Python 2 and Python 3. It remains for backwards
compatibility until the next major version.req  


env/lib/python3.12/site-packages/requests/compat.py
"""

try:
    pass
except ImportError:
    pass

import sys

# -------
# Pythons
# -------

# Syntax sugar.
_ver = sys.version_info

#: Python 2.x?
is_py2 = _ver[0] == 2

#: Python 3.x?
is_py3 = _ver[0] == 3

# json/simplejson module import resolution
has_simplejson = False
try:
    pass

    has_simplejson = True
except ImportError:
    pass

if has_simplejson:
    pass
else:
    pass

# Keep OrderedDict for backwards compatibility.

# --------------
# Legacy Imports
# --------------

builtin_str = str
str = str
bytes = bytes
basestring = (str, bytes)
numeric_types = (int, float)
integer_types = (int,)
```

FOr me to solve, delete env, run `uv cache clean`, than recreate the env
"""
requests.compat
~~~~~~~~~~~~~~~

This module previously handled import compatibility issues
between Python 2 and Python 3. It remains for backwards
compatibility until the next major version.
"""

try:
    import chardet
except ImportError:
    import charset_normalizer as chardet

import sys

# -------
# Pythons
# -------

# Syntax sugar.
_ver = sys.version_info

#: Python 2.x?
is_py2 = _ver[0] == 2

#: Python 3.x?
is_py3 = _ver[0] == 3

# json/simplejson module import resolution
has_simplejson = False
try:
    import simplejson as json

    has_simplejson = True
except ImportError:
    import json

if has_simplejson:
    from simplejson import JSONDecodeError
else:
    from json import JSONDecodeError

# Keep OrderedDict for backwards compatibility.
from collections import OrderedDict
from collections.abc import Callable, Mapping, MutableMapping
from http import cookiejar as cookielib
from http.cookies import Morsel
from io import StringIO

# --------------
# Legacy Imports
# --------------
from urllib.parse import (
    quote,
    quote_plus,
    unquote,
    unquote_plus,
    urldefrag,
    urlencode,
    urljoin,
    urlparse,
    urlsplit,
    urlunparse,
)
from urllib.request import (
    getproxies,
    getproxies_environment,
    parse_http_list,
    proxy_bypass,
    proxy_bypass_environment,
)

builtin_str = str
str = str
bytes = bytes
basestring = (str, bytes)
numeric_types = (int, float)
integer_types = (int,)
```
 + requests==2.31.0



Unfornately, the venv from other projects now is broken and I have to delete and uv cache clean 




### Platform

Ubuntu 22.04.5 LTS

### Version

uv 0.8.17

### Python version

Python 3.10.15 and Python 3.12.10

---

_Label `bug` added by @danielpassy on 2025-09-26 17:53_

---

_Comment by @charliermarsh on 2025-09-27 18:13_

Can you clarify which requests version is being installed that shouldn't be, and which version you expect to get instead?

---

_Label `bug` removed by @charliermarsh on 2025-09-27 18:13_

---

_Label `needs-mre` added by @charliermarsh on 2025-09-27 18:13_

---

_Comment by @danielpassy on 2025-09-29 14:37_

As soon as it happens again, I'll verify.

---

_Comment by @danielpassy on 2025-11-05 23:15_

Ok, I just happened again.
I created an env python 3.12 with `uv venv env -p 3.12`, use it, then I opened another shell into another project, and..
```
Traceback (most recent call last):
  File "/home/daniel/projects/eva/projects/eden-middleware/env/lib/python3.10/site-packages/django/core/management/base.py", line 412, in run_from_argv
    self.execute(*args, **cmd_options)
  File "/home/daniel/projects/eva/projects/eden-middleware/env/lib/python3.10/site-packages/django/core/management/commands/runserver.py", line 74, in execute
    super().execute(*args, **options)
  File "/home/daniel/projects/eva/projects/eden-middleware/env/lib/python3.10/site-packages/django/core/management/base.py", line 458, in execute
    output = self.handle(*args, **options)
  File "/home/daniel/projects/eva/projects/eden-middleware/env/lib/python3.10/site-packages/django/core/management/commands/runserver.py", line 81, in handle
    if not settings.DEBUG and not settings.ALLOWED_HOSTS:
  File "/home/daniel/projects/eva/projects/eden-middleware/env/lib/python3.10/site-packages/django/conf/__init__.py", line 102, in __getattr__
    self._setup(name)
  File "/home/daniel/projects/eva/projects/eden-middleware/env/lib/python3.10/site-packages/django/conf/__init__.py", line 89, in _setup
    self._wrapped = Settings(settings_module)
  File "/home/daniel/projects/eva/projects/eden-middleware/env/lib/python3.10/site-packages/django/conf/__init__.py", line 217, in __init__
    mod = importlib.import_module(self.SETTINGS_MODULE)
  File "/home/daniel/.local/share/uv/python/cpython-3.10.15-linux-x86_64-gnu/lib/python3.10/importlib/__init__.py", line 126, in import_module
    return _bootstrap._gcd_import(name[level:], package, level)
  File "<frozen importlib._bootstrap>", line 1050, in _gcd_import
  File "<frozen importlib._bootstrap>", line 1027, in _find_and_load
  File "<frozen importlib._bootstrap>", line 1006, in _find_and_load_unlocked
  File "<frozen importlib._bootstrap>", line 688, in _load_unlocked
  File "<frozen importlib._bootstrap_external>", line 883, in exec_module
  File "<frozen importlib._bootstrap>", line 241, in _call_with_frames_removed
  File "/home/daniel/projects/eva/projects/eden-middleware/middleware/settings.py", line 11, in <module>
    from middleware import env
  File "/home/daniel/projects/eva/projects/eden-middleware/middleware/env.py", line 3, in <module>
    from environ import Env
  File "/home/daniel/projects/eva/projects/eden-middleware/env/lib/python3.10/site-packages/environ/__init__.py", line 18, in <module>
    from .environ import *
  File "/home/daniel/projects/eva/projects/eden-middleware/env/lib/python3.10/site-packages/environ/environ.py", line 31, in <module>
    from .compat import (
ImportError: cannot import name 'json' from 'environ.compat' (/home/daniel/projects/eva/projects/eden-middleware/env/lib/python3.10/site-packages/environ/compat.py)

During handling of the above exception, another exception occurred:

Traceback (most recent call last):
  File "/home/daniel/projects/eva/projects/eden-middleware/manage.py", line 22, in <module>
    main()
  File "/home/daniel/projects/eva/projects/eden-middleware/manage.py", line 18, in main
    execute_from_command_line(sys.argv)
  File "/home/daniel/projects/eva/projects/eden-middleware/env/lib/python3.10/site-packages/django/core/management/__init__.py", line 442, in execute_from_command_line
    utility.execute()
  File "/home/daniel/projects/eva/projects/eden-middleware/env/lib/python3.10/site-packages/django/core/management/__init__.py", line 436, in execute
    self.fetch_command(subcommand).run_from_argv(self.argv)
  File "/home/daniel/projects/eva/projects/eden-middleware/env/lib/python3.10/site-packages/django/core/management/base.py", line 425, in run_from_argv
    connections.close_all()
  File "/home/daniel/projects/eva/projects/eden-middleware/env/lib/python3.10/site-packages/django/utils/connection.py", line 84, in close_all
    for conn in self.all(initialized_only=True):
  File "/home/daniel/projects/eva/projects/eden-middleware/env/lib/python3.10/site-packages/django/utils/connection.py", line 76, in all
    return [
  File "/home/daniel/projects/eva/projects/eden-middleware/env/lib/python3.10/site-packages/django/utils/connection.py", line 73, in __iter__
    return iter(self.settings)
  File "/home/daniel/projects/eva/projects/eden-middleware/env/lib/python3.10/site-packages/django/utils/functional.py", line 57, in __get__
    res = instance.__dict__[self.name] = self.func(instance)
  File "/home/daniel/projects/eva/projects/eden-middleware/env/lib/python3.10/site-packages/django/utils/connection.py", line 45, in settings
    self._settings = self.configure_settings(self._settings)
  File "/home/daniel/projects/eva/projects/eden-middleware/env/lib/python3.10/site-packages/django/db/utils.py", line 148, in configure_settings
    databases = super().configure_settings(databases)
  File "/home/daniel/projects/eva/projects/eden-middleware/env/lib/python3.10/site-packages/django/utils/connection.py", line 50, in configure_settings
    settings = getattr(django_settings, self.settings_name)
  File "/home/daniel/projects/eva/projects/eden-middleware/env/lib/python3.10/site-packages/django/conf/__init__.py", line 102, in __getattr__
    self._setup(name)
  File "/home/daniel/projects/eva/projects/eden-middleware/env/lib/python3.10/site-packages/django/conf/__init__.py", line 89, in _setup
    self._wrapped = Settings(settings_module)
  File "/home/daniel/projects/eva/projects/eden-middleware/env/lib/python3.10/site-packages/django/conf/__init__.py", line 217, in __init__
    mod = importlib.import_module(self.SETTINGS_MODULE)
  File "/home/daniel/.local/share/uv/python/cpython-3.10.15-linux-x86_64-gnu/lib/python3.10/importlib/__init__.py", line 126, in import_module
    return _bootstrap._gcd_import(name[level:], package, level)
  File "<frozen importlib._bootstrap>", line 1050, in _gcd_import
  File "<frozen importlib._bootstrap>", line 1027, in _find_and_load
  File "<frozen importlib._bootstrap>", line 1006, in _find_and_load_unlocked
  File "<frozen importlib._bootstrap>", line 688, in _load_unlocked
  File "<frozen importlib._bootstrap_external>", line 883, in exec_module
  File "<frozen importlib._bootstrap>", line 241, in _call_with_frames_removed
  File "/home/daniel/projects/eva/projects/eden-middleware/middleware/settings.py", line 11, in <module>
    from middleware import env
  File "/home/daniel/projects/eva/projects/eden-middleware/middleware/env.py", line 3, in <module>
    from environ import Env
  File "/home/daniel/projects/eva/projects/eden-middleware/env/lib/python3.10/site-packages/environ/__init__.py", line 18, in <module>
    from .environ import *
  File "/home/daniel/projects/eva/projects/eden-middleware/env/lib/python3.10/site-packages/environ/environ.py", line 31, in <module>
    from .compat import (
ImportError: cannot import name 'json' from 'environ.compat' (/home/daniel/projects/eva/projects/eden-middleware/env/lib/python3.10/site-packages/environ/compat.py)
```


the content of compat.py:
```
# This file is part of the django-environ.
#
# Copyright (c) 2021-2022, Serghei Iakovlev <egrep@protonmail.ch>
# Copyright (c) 2013-2021, Daniele Faraglia <daniele.faraglia@gmail.com>
#
# For the full copyright and license information, please view
# the LICENSE.txt file that was distributed with this source code.

"""
Django-environ allows you to utilize 12factor inspired environment
variables to configure your Django application.
"""

import ast
import itertools
import logging
import os
import re
import sys
import warnings
from urllib.parse import (
    parse_qs,
    ParseResult,
    quote,
    unquote,
    unquote_plus,
    urlparse,
    urlunparse,
)

from .compat import (
    DJANGO_POSTGRES,
    ImproperlyConfigured,
    json,
    PYMEMCACHE_DRIVER,
    REDIS_DRIVER,
)
```
the content of .compat

```
# This file is part of the django-environ.
#
# Copyright (c) 2021-2022, Serghei Iakovlev <egrep@protonmail.ch>
# Copyright (c) 2013-2021, Daniele Faraglia <daniele.faraglia@gmail.com>
#
# For the full copyright and license information, please view
# the LICENSE.txt file that was distributed with this source code.

"""This module handles import compatibility issues."""

from importlib.util import find_spec

if find_spec('simplejson'):
    pass
else:
    pass

if find_spec('django'):
    from django import VERSION as DJANGO_VERSION
    from django.core.exceptions import ImproperlyConfigured
else:
    DJANGO_VERSION = None

    class ImproperlyConfigured(Exception):
        """Django is somehow improperly configured"""


def choose_rediscache_driver():
    """Backward compatibility for RedisCache driver."""

    # django-redis library takes precedence
    if find_spec('django_redis'):
        return 'django_redis.cache.RedisCache'

    # use built-in support if Django 4+
    if DJANGO_VERSION is not None and DJANGO_VERSION >= (4, 0):
        return 'django.core.cache.backends.redis.RedisCache'

    # back compatibility with redis_cache package
    return 'redis_cache.RedisCache'


def choose_postgres_driver():
    """Backward compatibility for postgresql driver."""
    old_django = DJANGO_VERSION is not None and DJANGO_VERSION < (2, 0)
    if old_django:
        return 'django.db.backends.postgresql_psycopg2'
    return 'django.db.backends.postgresql'


def choose_pymemcache_driver():
    """Backward compatibility for pymemcache."""
    old_django = DJANGO_VERSION is not None and DJANGO_VERSION < (3, 2)
    if old_django or not find_spec('pymemcache'):
        # The original backend choice for the 'pymemcache' scheme is
        # unfortunately 'pylibmc'.
        return 'django.core.cache.backends.memcached.PyLibMCCache'
    return 'django.core.cache.backends.memcached.PyMemcacheCache'


REDIS_DRIVER = choose_rediscache_driver()
"""The name of the RedisCache driver."""

DJANGO_POSTGRES = choose_postgres_driver()
"""The name of the PostgreSQL driver."""

PYMEMCACHE_DRIVER = choose_pymemcache_driver()
"""The name of the Pymemcache driver."""

```


then:
```
(env) daniel@daniel-Precision-3591:~/projects/eva/projects/eden-middleware$ uv cache clean
Clearing cache at: /home/daniel/.cache/uv
Removed 54556 files (1.4GiB)
(env) daniel@daniel-Precision-3591:~/projects/eva/projects/eden-middleware$ sudo rm -rf env
[sudo] password for daniel: 
(env) daniel@daniel-Precision-3591:~/projects/eva/projects/eden-middleware$ uv venv env -p 3.10
Using CPython 3.10.15
Creating virtual environment at: env
Activate with: source env/bin/activate
(env) daniel@daniel-Precision-3591:~/projects/eva/projects/eden-middleware$ source env/bin/activate
(env) daniel@daniel-Precision-3591:~/projects/eva/projects/eden-middleware$ uv pip install -r requirements/prod.txt 
``` 

suddenly my compat.py

```
# This file is part of the django-environ.
#
# Copyright (c) 2021-2022, Serghei Iakovlev <egrep@protonmail.ch>
# Copyright (c) 2013-2021, Daniele Faraglia <daniele.faraglia@gmail.com>
#
# For the full copyright and license information, please view
# the LICENSE.txt file that was distributed with this source code.

"""This module handles import compatibility issues."""

from importlib.util import find_spec

if find_spec('simplejson'):
    import simplejson as json
else:
    import json

if find_spec('django'):
    from django import VERSION as DJANGO_VERSION
    from django.core.exceptions import ImproperlyConfigured
else:
    DJANGO_VERSION = None

    class ImproperlyConfigured(Exception):
        """Django is somehow improperly configured"""


def choose_rediscache_driver():
    """Backward compatibility for RedisCache driver."""

    # django-redis library takes precedence
    if find_spec('django_redis'):
        return 'django_redis.cache.RedisCache'

    # use built-in support if Django 4+
    if DJANGO_VERSION is not None and DJANGO_VERSION >= (4, 0):
        return 'django.core.cache.backends.redis.RedisCache'

    # back compatibility with redis_cache package
    return 'redis_cache.RedisCache'


def choose_postgres_driver():
    """Backward compatibility for postgresql driver."""
    old_django = DJANGO_VERSION is not None and DJANGO_VERSION < (2, 0)
    if old_django:
        return 'django.db.backends.postgresql_psycopg2'
    return 'django.db.backends.postgresql'


def choose_pymemcache_driver():
    """Backward compatibility for pymemcache."""
    old_django = DJANGO_VERSION is not None and DJANGO_VERSION < (3, 2)
    if old_django or not find_spec('pymemcache'):
        # The original backend choice for the 'pymemcache' scheme is
        # unfortunately 'pylibmc'.
        return 'django.core.cache.backends.memcached.PyLibMCCache'
    return 'django.core.cache.backends.memcached.PyMemcacheCache'


REDIS_DRIVER = choose_rediscache_driver()
"""The name of the RedisCache driver."""

DJANGO_POSTGRES = choose_postgres_driver()
"""The name of the PostgreSQL driver."""

PYMEMCACHE_DRIVER = choose_pymemcache_driver()
"""The name of the Pymemcache driver."""
```




I actually did the same for python 3.12... and it got the same compact.py as in the python 3.10, I have no idea what was that weird compact.py, nor where it came from..

---

_Comment by @konstin on 2025-12-18 15:29_

Is there maybe a tool changing those files? django-environ only has a `django_environ-0.12.0-py2.py3-none-any.whl` package, so the Python version shouldn't be the issue.

---
