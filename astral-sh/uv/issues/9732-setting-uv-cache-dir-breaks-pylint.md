```yaml
number: 9732
title: Setting UV_CACHE_DIR breaks pylint
type: issue
state: closed
author: max-wittig
labels: []
assignees: []
created_at: 2024-12-09T09:26:51Z
updated_at: 2024-12-09T10:10:46Z
url: https://github.com/astral-sh/uv/issues/9732
synced_at: 2026-01-12T15:59:57Z
```

# Setting UV_CACHE_DIR breaks pylint

---

_@max-wittig_

Hi,

I'm not sure that this is 100% related to `uv`, but it seems like it.

When setting `UV_CACHE_DIR` in `Gitlab-CI` seems to break pylint and its ability to resolve modules.

```yml
backend:lint:
  stage: checks
  extends: .backend-defaults
  variables:
    UV_CACHE_DIR: $CI_PROJECT_DIR/.cache
  script:
     - uv sync
     - ...
     - ...
     - uv run poe pylint --output-format=junit --extra='--output=pylint_results.xml'
```

See the following crash that only happens, if `UV_CACHE_DIR` is set and only in CI:

```sh
$ uv sync
Using CPython 3.12.0
Creating virtual environment at: .venv
Resolved 237 packages in 2ms
Prepared 233 packages in 6.23s
Installed 233 packages in 368ms
 + aiohappyeyeballs==2.4.4
 + aiohttp==3.11.10
 + aiosignal==1.3.1
 + amqp==5.3.1
 + annotated-types==0.7.0
 + anybadge==1.14.0
 + anyio==4.7.0
 + apscheduler==3.11.0
 + argcomplete==3.5.2
 + asgiref==3.8.1
 + astroid==3.3.5
 + attrs==24.2.0
 + beautifulsoup4==4.12.3
 + billiard==4.2.1
 + celery==5.4.0
 + celery-once==3.0.1
 + celery-stubs==0.1.3
 + celery-types==0.22.0
 + certifi==2024.8.30
 + cffi==1.17.1
 + chardet==5.2.0
 + charset-normalizer==3.4.0
 + click==8.1.7
 + click-didyoumean==0.3.1
 + click-plugins==1.1.1
 + click-repl==0.3.0
 + colorama==0.4.6
 + commitizen==4.1.0
 + coreapi==2.3.3
 + coreschema==0.0.4
 + coverage==7.6.9
 + cryptography==43.0.3
 + cssbeautifier==1.15.1
 + cyclonedx-bom==3.11.7
 + cyclonedx-python-lib==3.1.5
 + dataclasses-json==0.6.7
 + dataproperty==1.0.1
 + decli==0.6.2
 + dill==0.3.9
 + directory-client==2.0.0
 + distro==1.9.0
 + django==5.1.3
 + django-allauth==65.3.0
 + django-cachalot==2.7.0
 + django-cors-headers==4.6.0
 + django-csp==3.8
 + django-extensions==3.2.3
 + django-filter==23.5
 + django-filter-stubs==0.1.3
 + django-hijack==3.7.0
 + django-ipware==7.0.1
 + django-oauth-toolkit==3.0.1
 + django-prometheus==2.3.1
 + django-redis==5.4.0
 + django-rest-knox==5.0.2
 + django-revproxy==0.13.0
 + django-simple-history==3.7.0
 + django-structlog==9.0.0
 + django-stubs==5.1.1
 + django-stubs-ext==5.1.1
 + django-test-migrations==1.4.0
 + djangorestframework==3.15.2
 + djangorestframework-stubs==3.15.1
 + djlint==1.36.3
 + drf-excel==2.5.1
 + drf-nested-routers==0.94.1
 + drf-spectacular==0.28.0
 + drf-spectacular-sidecar==2024.12.1
 + editorconfig==0.12.4
 + esprima==4.0.1
 + et-xmlfile==2.0.0
 + factory-boy==3.3.1
 + faker==33.1.0
 + fcache==0.6.0
 + flower==2.0.1
 + freezegun==1.5.1
 + frozenlist==1.5.0
 + future==1.0.0
 + gherkin-official==29.0.0
 + gitdb==4.0.11
 + gitpython==3.1.43
 + greenlet==3.1.1
 + gunicorn==23.0.0
 + h11==0.14.0
 + httpcore==1.0.7
 + httpx==0.28.1
 + httpx-sse==0.4.0
 + humanize==4.11.0
 + idna==3.10
 + importlib-metadata==8.5.0
 + inflection==0.5.1
 + iniconfig==2.0.0
 + iso8601==2.1.0
 + isort==5.13.2
 + itypes==1.2.0
 + jinja2==3.1.4
 + jiter==0.8.0
 + jsbeautifier==1.15.1
 + json5==0.10.0
 + jsonpatch==1.33
 + jsonpointer==3.0.0
 + jsonschema==4.23.0
 + jsonschema-specifications==2024.10.1
 + junit-xml-2==1.9
 + jwcrypto==1.5.6
 + kombu==5.4.2
 + langchain==0.3.10
 + langchain-community==0.3.10
 + langchain-core==0.3.22
 + langchain-openai==0.2.11
 + langchain-postgres==0.0.12
 + langchain-text-splitters==0.3.2
 + langsmith==0.1.147
 + loguru==0.7.3
 + mako==1.3.8
 + markdown==3.7
 + markdown-it-py==3.0.0
 + markupsafe==3.0.2
 + marshmallow==3.23.1
 + mbstrdecoder==1.1.3
 + mccabe==0.7.0
 + mdurl==0.1.2
 + mmh3==5.0.1
 + multidict==6.1.0
 + mypy==1.13.0
 + mypy-extensions==1.0.0
 + numpy==1.26.4
 + oauthlib==3.2.2
 + openai==1.57.0
 + openpyxl==3.1.5
 + openpyxl-stubs==0.1.25
 + orjson==3.10.12
 + packageurl-python==0.16.0
 + packaging==24.2
 + parse==1.20.2
 + parse-type==0.6.4
 + pastel==0.2.1
 + path==16.16.0
 + pathspec==0.12.1
 + pathvalidate==3.2.1
 + pgvector==0.2.5
 + pip-requirements-parser==32.0.1
 + platformdirs==3.11.0
 + pluggy==1.5.0
 + poethepoet==0.31.1
 + pprintpp==0.4.0
 + prometheus-client==0.21.1
 + prompt-toolkit==3.0.36
 + propcache==0.2.1
 + psycopg==3.2.3
 + psycopg-binary==3.2.3
 + psycopg-pool==3.2.4
 + pycountry==24.6.1
 + pycparser==2.22
 + pydantic==2.10.3
 + pydantic-core==2.27.1
 + pydantic-settings==2.6.1
 + pydotplus==2.0.2
 + pygments==2.18.0
 + pyjwt==2.10.1
 + pylint==3.3.2
 + pylint-django==2.6.1
 + pylint-junit==0.3.4
 + pylint-plugin-utils==0.8.2
 + pyparsing==3.2.0
 + pytablereader==0.31.4
 + pytest==8.3.4
 + pytest-bdd==8.1.0
 + pytest-clarity==1.0.1
 + pytest-cov==6.0.0
 + pytest-deadfixtures==2.2.1
 + pytest-django==4.9.0
 + pytest-freezegun==0.4.2
 + pytest-unordered==0.6.1
 + python-dateutil==2.9.0.post0
 + python-dotenv==1.0.1
 + python-gitlab==5.1.0
 + python-ipware==3.0.0
 + python-json-logger==2.0.7
 + pytz==2024.2
 + pyyaml==6.0.2
 + questionary==2.0.1
 + redis==5.2.1
 + referencing==0.35.1
 + regex==2024.11.6
 + requests==2.32.3
 + requests-oauthlib==2.0.0
 + requests-toolbelt==1.0.0
 + responses==0.25.3
 + rich==13.9.4
 + rpds-py==0.22.3
 + ruamel-yaml==0.18.6
 + ruamel-yaml-clib==0.2.12
 + ruff==0.8.2
 + semver==3.0.2
 + sentry-sdk==2.19.2
 + setuptools==75.6.0
 + six==1.17.0
 + smime-email==1.0.0
 + smmap==5.0.1
 + sniffio==1.3.1
 + sortedcontainers==2.4.0
 + soupsieve==2.6
 + sqlalchemy==2.0.36
 + sqlparse==0.5.2
 + structlog==24.4.0
 + tabledata==1.3.3
 + tenacity==9.0.0
 + termcolor==2.5.0
 + tiktoken==0.8.0
 + toml==0.10.2
 + tomlkit==0.13.2
 + tornado==6.4.2
 + tqdm==4.67.1
 + tree-sitter==0.23.2
 + tree-sitter-languages==1.10.2
 + typepy==1.3.2
 + types-jsonschema==4.23.0.20241208
 + types-pyyaml==6.0.12.20240917
 + types-requests==2.32.0.20241016
 + typing-extensions==4.12.2
 + typing-inspect==0.9.0
 + tzdata==2024.2
 + tzlocal==5.2
 + unleash-django-util==0.4.4
 + unleashclient==6.0.1
 + uritemplate==4.1.1
 + urllib3==2.2.3
 + vine==5.1.0
 + wcwidth==0.2.13
 + yarl==1.18.3
 + yggdrasil-engine==0.1.4
 + zipp==3.21.0
$ uv run poe pylint --output-format=junit --extra='--output=pylint_results.xml'
Poe => pylint -j 0 --recursive=y --output-format=${output_format} ${extra} .
concurrent.futures.process._RemoteTraceback: 
"""
Traceback (most recent call last):
  File "/root/.local/share/uv/python/cpython-3.12.0-linux-x86_64-gnu/lib/python3.12/concurrent/futures/process.py", line 258, in _process_worker
    r = call_item.fn(*call_item.args, **call_item.kwargs)
        ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/root/.local/share/uv/python/cpython-3.12.0-linux-x86_64-gnu/lib/python3.12/concurrent/futures/process.py", line 207, in _process_chunk
    return [fn(*args) for args in chunk]
            ^^^^^^^^^
  File "/builds/test_project/.venv/lib/python3.12/site-packages/pylint/lint/parallel.py", line 79, in _worker_check_single_file
    _worker_linter.check_single_file_item(file_item)
  File "/builds/test_project/.venv/lib/python3.12/site-packages/pylint/lint/pylinter.py", line 739, in check_single_file_item
    with self._astroid_module_checker() as check_astroid_module:
  File "/root/.local/share/uv/python/cpython-3.12.0-linux-x86_64-gnu/lib/python3.12/contextlib.py", line 137, in __enter__
    return next(self.gen)
           ^^^^^^^^^^^^^^
  File "/builds/test_project/.venv/lib/python3.12/site-packages/pylint/lint/pylinter.py", line 947, in _astroid_module_checker
    checker.open()
  File "/builds/test_project/.venv/lib/python3.12/site-packages/pylint_django/checkers/foreign_key_strings.py", line 85, in open
    django.setup()
  File "/builds/test_project/.venv/lib/python3.12/site-packages/django/__init__.py", line 19, in setup
    configure_logging(settings.LOGGING_CONFIG, settings.LOGGING)
                      ^^^^^^^^^^^^^^^^^^^^^^^
  File "/builds/test_project/.venv/lib/python3.12/site-packages/django/conf/__init__.py", line 81, in __getattr__
    self._setup(name)
  File "/builds/test_project/.venv/lib/python3.12/site-packages/django/conf/__init__.py", line 68, in _setup
    self._wrapped = Settings(settings_module)
                    ^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/builds/test_project/.venv/lib/python3.12/site-packages/django/conf/__init__.py", line 166, in __init__
    mod = importlib.import_module(self.SETTINGS_MODULE)
          ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/root/.local/share/uv/python/cpython-3.12.0-linux-x86_64-gnu/lib/python3.12/importlib/__init__.py", line 90, in import_module
    return _bootstrap._gcd_import(name[level:], package, level)
           ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "<frozen importlib._bootstrap>", line 1381, in _gcd_import
  File "<frozen importlib._bootstrap>", line 1354, in _find_and_load
  File "<frozen importlib._bootstrap>", line 1304, in _find_and_load_unlocked
  File "<frozen importlib._bootstrap>", line 488, in _call_with_frames_removed
  File "<frozen importlib._bootstrap>", line 1381, in _gcd_import
  File "<frozen importlib._bootstrap>", line 1354, in _find_and_load
  File "<frozen importlib._bootstrap>", line 1304, in _find_and_load_unlocked
  File "<frozen importlib._bootstrap>", line 488, in _call_with_frames_removed
  File "<frozen importlib._bootstrap>", line 1381, in _gcd_import
  File "<frozen importlib._bootstrap>", line 1354, in _find_and_load
  File "<frozen importlib._bootstrap>", line 1325, in _find_and_load_unlocked
  File "<frozen importlib._bootstrap>", line 929, in _load_unlocked
  File "<frozen importlib._bootstrap_external>", line 994, in exec_module
  File "<frozen importlib._bootstrap>", line 488, in _call_with_frames_removed
  File "/builds/test_project/__init__.py", line 3, in <module>
    from .celery import app as celery_app
  File "/builds/test_project/celery.py", line 126, in <module>
    django.setup()
  File "/builds/test_project/.venv/lib/python3.12/site-packages/django/__init__.py", line 24, in setup
    apps.populate(settings.INSTALLED_APPS)
  File "/builds/test_project/.venv/lib/python3.12/site-packages/django/apps/registry.py", line 91, in populate
    app_config = AppConfig.create(entry)
                 ^^^^^^^^^^^^^^^^^^^^^^^
  File "/builds/test_project/.venv/lib/python3.12/site-packages/django/apps/config.py", line 123, in create
    mod = import_module(mod_path)
          ^^^^^^^^^^^^^^^^^^^^^^^
  File "/root/.local/share/uv/python/cpython-3.12.0-linux-x86_64-gnu/lib/python3.12/importlib/__init__.py", line 90, in import_module
    return _bootstrap._gcd_import(name[level:], package, level)
           ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/builds/test_project/llm/apps.py", line 4, in <module>
    from llm.helpers import setup_llm, setup_vectorstore
  File "/builds/test_project/llm/helpers.py", line 6, in <module>
    from langchain_postgres import PGVector
  File "/builds/test_project/.cache/archive-v0/joQ1RGialewx9X5bOAU-B/langchain_postgres/__init__.py", line 5, in <module>
    from langchain_postgres.vectorstores import PGVector
  File "/builds/test_project/.cache/archive-v0/joQ1RGialewx9X5bOAU-B/langchain_postgres/vectorstores.py", line 27, in <module>
    import sqlalchemy
  File "/builds/test_project/.cache/archive-v0/086KCgNnm6Rn_iiyQAYiH/pgvector/sqlalchemy/__init__.py", line 1, in <module>
    from sqlalchemy.dialects.postgresql.base import ischema_names
ModuleNotFoundError: No module named 'sqlalchemy.dialects'
"""
The above exception was the direct cause of the following exception:
Traceback (most recent call last):
  File "/builds/test_project/.venv/bin/pylint", line 8, in <module>
    sys.exit(run_pylint())
             ^^^^^^^^^^^^
  File "/builds/test_project/.venv/lib/python3.12/site-packages/pylint/__init__.py", line 34, in run_pylint
    PylintRun(argv or sys.argv[1:])
  File "/builds/test_project/.venv/lib/python3.12/site-packages/pylint/lint/run.py", line 209, in __init__
    linter.check(args)
  File "/builds/test_project/.venv/lib/python3.12/site-packages/pylint/lint/pylinter.py", line 679, in check
    check_parallel(
  File "/builds/test_project/.venv/lib/python3.12/site-packages/pylint/lint/parallel.py", line 153, in check_parallel
    for (
  File "/root/.local/share/uv/python/cpython-3.12.0-linux-x86_64-gnu/lib/python3.12/concurrent/futures/process.py", line 608, in _chain_from_iterable_of_lists
    for element in iterable:
  File "/root/.local/share/uv/python/cpython-3.12.0-linux-x86_64-gnu/lib/python3.12/concurrent/futures/_base.py", line 619, in result_iterator
    yield _result_or_cancel(fs.pop())
          ^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/root/.local/share/uv/python/cpython-3.12.0-linux-x86_64-gnu/lib/python3.12/concurrent/futures/_base.py", line 317, in _result_or_cancel
    return fut.result(timeout)
           ^^^^^^^^^^^^^^^^^^^
  File "/root/.local/share/uv/python/cpython-3.12.0-linux-x86_64-gnu/lib/python3.12/concurrent/futures/_base.py", line 456, in result
    return self.__get_result()
           ^^^^^^^^^^^^^^^^^^^
  File "/root/.local/share/uv/python/cpython-3.12.0-linux-x86_64-gnu/lib/python3.12/concurrent/futures/_base.py", line 401, in __get_result
    raise self._exception
ModuleNotFoundError: No module named 'sqlalchemy.dialects'
Uploading artifacts for failed job
00:01
Uploading artifacts...
pylint_results.xml: found 1 matching artifact files and directories 
Uploading artifacts as "junit" to coordinator... [201](https://example.gitlab.com/test_project/-/jobs/230576139#L201) Created  id=230576139 responseStatus=201 Created token=glcbt-64
Cleaning up project directory and file based variables
00:00
ERROR: Job failed: exit code 1
```

---

_Comment by @FishAlchemist on 2024-12-09 09:48_

https://pylint.readthedocs.io/en/stable/faq.html#where-is-the-persistent-data-stored-to-compare-between-successive-runs
Linux: ``~/.cache/pylint``
I'm not sure about the contents of $CI_PROJECT_DIR, but is it possible that you've put both the pylint and uv caches in the same directory?

---

_Comment by @max-wittig on 2024-12-09 10:10_

Changing the `UV_CACHE_DIR` to `.uv_cache` causes the same error message:

```sh
$ uv run poe pylint --output-format=junit --extra='--output=pylint_results.xml'
Poe => pylint -j 0 --recursive=y --output-format=${output_format} ${extra} .
concurrent.futures.process._RemoteTraceback: 
"""
Traceback (most recent call last):
  File "/root/.local/share/uv/python/cpython-3.12.0-linux-x86_64-gnu/lib/python3.12/concurrent/futures/process.py", line 258, in _process_worker
    r = call_item.fn(*call_item.args, **call_item.kwargs)
        ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/root/.local/share/uv/python/cpython-3.12.0-linux-x86_64-gnu/lib/python3.12/concurrent/futures/process.py", line 207, in _process_chunk
    return [fn(*args) for args in chunk]
            ^^^^^^^^^
  File "/builds/code-ops/code-apps/.venv/lib/python3.12/site-packages/pylint/lint/parallel.py", line 79, in _worker_check_single_file
    _worker_linter.check_single_file_item(file_item)
  File "/builds/code-ops/code-apps/.venv/lib/python3.12/site-packages/pylint/lint/pylinter.py", line 739, in check_single_file_item
    with self._astroid_module_checker() as check_astroid_module:
  File "/root/.local/share/uv/python/cpython-3.12.0-linux-x86_64-gnu/lib/python3.12/contextlib.py", line 137, in __enter__
    return next(self.gen)
           ^^^^^^^^^^^^^^
  File "/builds/code-ops/code-apps/.venv/lib/python3.12/site-packages/pylint/lint/pylinter.py", line 947, in _astroid_module_checker
    checker.open()
  File "/builds/code-ops/code-apps/.venv/lib/python3.12/site-packages/pylint_django/checkers/foreign_key_strings.py", line 85, in open
    django.setup()
  File "/builds/code-ops/code-apps/.venv/lib/python3.12/site-packages/django/__init__.py", line 19, in setup
    configure_logging(settings.LOGGING_CONFIG, settings.LOGGING)
                      ^^^^^^^^^^^^^^^^^^^^^^^
  File "/builds/code-ops/code-apps/.venv/lib/python3.12/site-packages/django/conf/__init__.py", line 81, in __getattr__
    self._setup(name)
  File "/builds/code-ops/code-apps/.venv/lib/python3.12/site-packages/django/conf/__init__.py", line 68, in _setup
    self._wrapped = Settings(settings_module)
                    ^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/builds/code-ops/code-apps/.venv/lib/python3.12/site-packages/django/conf/__init__.py", line 166, in __init__
    mod = importlib.import_module(self.SETTINGS_MODULE)
          ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/root/.local/share/uv/python/cpython-3.12.0-linux-x86_64-gnu/lib/python3.12/importlib/__init__.py", line 90, in import_module
    return _bootstrap._gcd_import(name[level:], package, level)
           ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "<frozen importlib._bootstrap>", line 1381, in _gcd_import
  File "<frozen importlib._bootstrap>", line 1354, in _find_and_load
  File "<frozen importlib._bootstrap>", line 1304, in _find_and_load_unlocked
  File "<frozen importlib._bootstrap>", line 488, in _call_with_frames_removed
  File "<frozen importlib._bootstrap>", line 1381, in _gcd_import
  File "<frozen importlib._bootstrap>", line 1354, in _find_and_load
  File "<frozen importlib._bootstrap>", line 1304, in _find_and_load_unlocked
  File "<frozen importlib._bootstrap>", line 488, in _call_with_frames_removed
  File "<frozen importlib._bootstrap>", line 1381, in _gcd_import
  File "<frozen importlib._bootstrap>", line 1354, in _find_and_load
  File "<frozen importlib._bootstrap>", line 1325, in _find_and_load_unlocked
  File "<frozen importlib._bootstrap>", line 929, in _load_unlocked
  File "<frozen importlib._bootstrap_external>", line 994, in exec_module
  File "<frozen importlib._bootstrap>", line 488, in _call_with_frames_removed
  File "/builds/code-ops/code-apps/codeapps/__init__.py", line 3, in <module>
    from .celery import app as celery_app
  File "/builds/code-ops/code-apps/codeapps/celery.py", line 126, in <module>
    django.setup()
  File "/builds/code-ops/code-apps/.venv/lib/python3.12/site-packages/django/__init__.py", line 24, in setup
    apps.populate(settings.INSTALLED_APPS)
  File "/builds/code-ops/code-apps/.venv/lib/python3.12/site-packages/django/apps/registry.py", line 91, in populate
    app_config = AppConfig.create(entry)
                 ^^^^^^^^^^^^^^^^^^^^^^^
  File "/builds/code-ops/code-apps/.venv/lib/python3.12/site-packages/django/apps/config.py", line 123, in create
    mod = import_module(mod_path)
          ^^^^^^^^^^^^^^^^^^^^^^^
  File "/root/.local/share/uv/python/cpython-3.12.0-linux-x86_64-gnu/lib/python3.12/importlib/__init__.py", line 90, in import_module
    return _bootstrap._gcd_import(name[level:], package, level)
           ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/builds/code-ops/code-apps/llm/apps.py", line 4, in <module>
    from llm.helpers import setup_llm, setup_vectorstore
  File "/builds/code-ops/code-apps/llm/helpers.py", line 6, in <module>
    from langchain_postgres import PGVector
  File "/builds/code-ops/code-apps/.uv-cache/archive-v0/xaY_NEVyToDYooPaCW0b5/langchain_postgres/__init__.py", line 5, in <module>
    from langchain_postgres.vectorstores import PGVector
  File "/builds/code-ops/code-apps/.uv-cache/archive-v0/xaY_NEVyToDYooPaCW0b5/langchain_postgres/vectorstores.py", line 27, in <module>
    import sqlalchemy
  File "/builds/code-ops/code-apps/.uv-cache/archive-v0/Go2jH0TIqWqDamfOQnUI5/pgvector/sqlalchemy/__init__.py", line 1, in <module>
    from sqlalchemy.dialects.postgresql.base import ischema_names
ModuleNotFoundError: No module named 'sqlalchemy.dialects'
"""
The above exception was the direct cause of the following exception:
Traceback (most recent call last):
  File "/builds/code-ops/code-apps/.venv/bin/pylint", line 8, in <module>
    sys.exit(run_pylint())
             ^^^^^^^^^^^^
  File "/builds/code-ops/code-apps/.venv/lib/python3.12/site-packages/pylint/__init__.py", line 34, in run_pylint
    PylintRun(argv or sys.argv[1:])
  File "/builds/code-ops/code-apps/.venv/lib/python3.12/site-packages/pylint/lint/run.py", line 209, in __init__
    linter.check(args)
  File "/builds/code-ops/code-apps/.venv/lib/python3.12/site-packages/pylint/lint/pylinter.py", line 679, in check
    check_parallel(
  File "/builds/code-ops/code-apps/.venv/lib/python3.12/site-packages/pylint/lint/parallel.py", line 153, in check_parallel
    for (
  File "/root/.local/share/uv/python/cpython-3.12.0-linux-x86_64-gnu/lib/python3.12/concurrent/futures/process.py", line 608, in _chain_from_iterable_of_lists
    for element in iterable:
  File "/root/.local/share/uv/python/cpython-3.12.0-linux-x86_64-gnu/lib/python3.12/concurrent/futures/_base.py", line 619, in result_iterator
    yield _result_or_cancel(fs.pop())
          ^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/root/.local/share/uv/python/cpython-3.12.0-linux-x86_64-gnu/lib/python3.12/concurrent/futures/_base.py", line 317, in _result_or_cancel
    return fut.result(timeout)
           ^^^^^^^^^^^^^^^^^^^
  File "/root/.local/share/uv/python/cpython-3.12.0-linux-x86_64-gnu/lib/python3.12/concurrent/futures/_base.py", line [456](https://code.siemens.com/code-ops/code-apps/-/jobs/230737950#L456), in result
    return self.__get_result()
           ^^^^^^^^^^^^^^^^^^^
  File "/root/.local/share/uv/python/cpython-3.12.0-linux-x86_64-gnu/lib/python3.12/concurrent/futures/_base.py", line 401, in __get_result
    raise self._exception
ModuleNotFoundError: No module named 'sqlalchemy.dialects'
```

Setting `UV_CACHE_DIR` outside of `$CI_PROJECT_DIR`(where the project files are stored, fixes it) so it must be a `pylint` issue.

Closing...

---

_Closed by @max-wittig on 2024-12-09 10:10_

---
