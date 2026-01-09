---
number: 1875
title: "uv pip compile doesn't output to a file by default (like pip-tools)"
type: issue
state: closed
author: ionelmc
labels: []
assignees: []
created_at: 2024-02-22T16:01:35Z
updated_at: 2024-02-22T16:41:47Z
url: https://github.com/astral-sh/uv/issues/1875
synced_at: 2026-01-07T13:12:16-06:00
---

# uv pip compile doesn't output to a file by default (like pip-tools)

---

_Issue opened by @ionelmc on 2024-02-22 16:01_


Using `uv 0.1.7`. 

Running something as simple as `uv pip compile packages.in` only outputs the result to stdout while `pip-compile packages.in` also creates a `packages.txt`.

For reference, verbose output (`packages.in` only contains a line with django):
```
 > uv pip compile packages.in --verbose
 uv::requirements::from_source source=packages.in
    0.002643s DEBUG uv_interpreter::interpreter Resolved python3 to /usr/bin/python3
    0.002708s DEBUG uv_interpreter::interpreter Using cached markers for: /usr/bin/python3
    0.002734s DEBUG uv::commands::pip_compile Using Python 3.11.5 interpreter at /usr/bin/python3 for builds
    0.003041s DEBUG uv_client::registry_client Using registry request timeout of 300s
 uv_client::flat_index::from_entries
 uv_resolver::resolver::solve
      0.004060s   0ms DEBUG uv_resolver::resolver Solving with target Python version 3.11.5
   uv_resolver::resolver::choose_version package=root
   uv_resolver::resolver::get_dependencies package=root, version=0a0.dev0
        0.004143s   0ms DEBUG uv_resolver::resolver Adding direct dependency: django*
   uv_resolver::resolver::choose_version package=django
     uv_resolver::resolver::package_wait package_name=django
 uv_resolver::resolver::process_request request=Versions django
   uv_client::registry_client::simple_api package=django
     uv_client::cached_client::get_cacheable
       uv_client::cached_client::read_and_parse_cache file=/home/ionel/.cache/uv/simple-v1/pypi/django.rkyv
 uv_resolver::resolver::process_request request=Prefetch django *
          0.004893s   0ms DEBUG uv_client::cached_client Found fresh response for: https://pypi.org/simple/django/
 uv_resolver::version_map::from_metadata
   uv_distribution::distribution_database::get_or_build_wheel_metadata dist=django==5.0.2
     uv_client::registry_client::wheel_metadata built_dist=django==5.0.2
       uv_client::cached_client::get_serde
         uv_client::cached_client::get_cacheable
           uv_client::cached_client::read_and_parse_cache file=/home/ionel/.cache/uv/wheels-v0/pypi/django/django-5.0.2-py3-none-any.msgpack
        0.005619s   1ms DEBUG uv_resolver::resolver Searching for a compatible version of django (*)
        0.005627s   1ms DEBUG uv_resolver::resolver Selecting: django==5.0.2 (Django-5.0.2-py3-none-any.whl)
   uv_resolver::resolver::get_dependencies package=django, version=5.0.2
     uv_resolver::resolver::distributions_wait package_id=django-5.0.2
              0.005692s   0ms DEBUG uv_client::cached_client Found fresh response for: https://files.pythonhosted.org/packages/50/1b/7536019fd20654919dcd81b475fee1e54f21bd71b2b4e094b2ab075478b2/Django-5.0.2-py3-none-any.whl
        0.005748s   0ms DEBUG uv_resolver::resolver Adding transitive dependency: asgiref>=3.7.0, <4
        0.005772s   0ms DEBUG uv_resolver::resolver Adding transitive dependency: sqlparse>=0.3.1
   uv_resolver::resolver::choose_version package=asgiref
     uv_resolver::resolver::package_wait package_name=asgiref
 uv_resolver::resolver::process_request request=Versions asgiref
   uv_client::registry_client::simple_api package=asgiref
     uv_client::cached_client::get_cacheable
       uv_client::cached_client::read_and_parse_cache file=/home/ionel/.cache/uv/simple-v1/pypi/asgiref.rkyv
 uv_resolver::resolver::process_request request=Versions sqlparse
   uv_client::registry_client::simple_api package=sqlparse
     uv_client::cached_client::get_cacheable
       uv_client::cached_client::read_and_parse_cache file=/home/ionel/.cache/uv/simple-v1/pypi/sqlparse.rkyv
 uv_resolver::resolver::process_request request=Prefetch sqlparse >=0.3.1
 uv_resolver::resolver::process_request request=Prefetch asgiref >=3.7.0, <4
          0.006065s   0ms DEBUG uv_client::cached_client Found fresh response for: https://pypi.org/simple/asgiref/
          0.006122s   0ms DEBUG uv_client::cached_client Found fresh response for: https://pypi.org/simple/sqlparse/
 uv_resolver::version_map::from_metadata
 uv_resolver::version_map::from_metadata
   uv_distribution::distribution_database::get_or_build_wheel_metadata dist=sqlparse==0.4.4
     uv_client::registry_client::wheel_metadata built_dist=sqlparse==0.4.4
       uv_client::cached_client::get_serde
         uv_client::cached_client::get_cacheable
           uv_client::cached_client::read_and_parse_cache file=/home/ionel/.cache/uv/wheels-v0/pypi/sqlparse/sqlparse-0.4.4-py3-none-any.msgpack
   uv_distribution::distribution_database::get_or_build_wheel_metadata dist=asgiref==3.7.2
     uv_client::registry_client::wheel_metadata built_dist=asgiref==3.7.2
       uv_client::cached_client::get_serde
         uv_client::cached_client::get_cacheable
           uv_client::cached_client::read_and_parse_cache file=/home/ionel/.cache/uv/wheels-v0/pypi/asgiref/asgiref-3.7.2-py3-none-any.msgpack
        0.006541s   0ms DEBUG uv_resolver::resolver Searching for a compatible version of asgiref (>=3.7.0, <4)
        0.006550s   0ms DEBUG uv_resolver::resolver Selecting: asgiref==3.7.2 (asgiref-3.7.2-py3-none-any.whl)
   uv_resolver::resolver::get_dependencies package=asgiref, version=3.7.2
     uv_resolver::resolver::distributions_wait package_id=asgiref-3.7.2
              0.006591s   0ms DEBUG uv_client::cached_client Found fresh response for: https://files.pythonhosted.org/packages/98/5a/66d7c9305baa9f11857f247d4ba761402cea75db6058ff850ed7128957b7/sqlparse-0.4.4-py3-none-any.whl
              0.006613s   0ms DEBUG uv_client::cached_client Found fresh response for: https://files.pythonhosted.org/packages/9b/80/b9051a4a07ad231558fcd8ffc89232711b4e618c15cb7a392a17384bbeef/asgiref-3.7.2-py3-none-any.whl
   uv_resolver::resolver::choose_version package=sqlparse
     uv_resolver::resolver::package_wait package_name=sqlparse
        0.006665s   0ms DEBUG uv_resolver::resolver Searching for a compatible version of sqlparse (>=0.3.1)
        0.006685s   0ms DEBUG uv_resolver::resolver Selecting: sqlparse==0.4.4 (sqlparse-0.4.4-py3-none-any.whl)
   uv_resolver::resolver::get_dependencies package=sqlparse, version=0.4.4
     uv_resolver::resolver::distributions_wait package_id=sqlparse-0.4.4
Resolved 3 packages in 6ms
# This file was autogenerated by uv via the following command:
#    uv pip compile packages.in --verbose
asgiref==3.7.2
    # via django
django==5.0.2
sqlparse==0.4.4
    # via django
```


---

_Comment by @charliermarsh on 2024-02-22 16:41_

Thanks -- this is a duplicate of https://github.com/astral-sh/uv/issues/1350.

---

_Closed by @charliermarsh on 2024-02-22 16:41_

---
