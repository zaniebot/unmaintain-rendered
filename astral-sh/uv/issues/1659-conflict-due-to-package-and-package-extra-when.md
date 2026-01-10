---
number: 1659
title: Conflict due to package and package-extra when installing airflow
type: issue
state: closed
author: erdembanak
labels:
  - bug
  - resolver
assignees: []
created_at: 2024-02-18T17:33:35Z
updated_at: 2024-02-22T02:51:23Z
url: https://github.com/astral-sh/uv/issues/1659
synced_at: 2026-01-10T01:23:08Z
---

# Conflict due to package and package-extra when installing airflow

---

_Issue opened by @erdembanak on 2024-02-18 17:33_

I'm trying to install airflow 2.4.2 with the constraints file provided by airflow ([file](https://raw.githubusercontent.com/invent-analytics/airflow-constraints/main/2.4.2/constraints-3.8.txt)) with the following command (I have downloaded the file):

`uv pip install apache-airflow==2.42 -c airflow-constraints.txt --verbose`

This gets an error due to conflicting swagger-ui versions. But it looks to be related to adding the package and the package with extra multiple times.

I have added the output in the end but I have noticed two things:

```
        0.027896s   2ms DEBUG uv_resolver::resolver Adding transitive dependency: connexion[flask]>=2.10.0
        0.027902s   2ms DEBUG uv_resolver::resolver Adding transitive dependency: attrs==22.1.0
        0.027908s   2ms DEBUG uv_resolver::resolver Adding transitive dependency: blinker==1.5
        0.027914s   2ms DEBUG uv_resolver::resolver Adding transitive dependency: typing-extensions==4.4.0
        0.027922s   2ms DEBUG uv_resolver::resolver Adding transitive dependency: colorlog==4.8.0
        0.027929s   2ms DEBUG uv_resolver::resolver Adding transitive dependency: tenacity==8.1.0
        0.027935s   2ms DEBUG uv_resolver::resolver Adding transitive dependency: flask-wtf==1.0.1
        0.027945s   2ms DEBUG uv_resolver::resolver Adding transitive dependency: setproctitle==1.3.2
        0.027951s   2ms DEBUG uv_resolver::resolver Adding transitive dependency: tabulate==0.9.0
        0.027956s   2ms DEBUG uv_resolver::resolver Adding transitive dependency: markdown==3.4.1
        0.027961s   2ms DEBUG uv_resolver::resolver Adding transitive dependency: alembic==1.8.1
        0.027966s   2ms DEBUG uv_resolver::resolver Adding transitive dependency: connexion==2.14.1

```

connexion dependency added as itself and then with flask (this also happens with swagger-ui). In the end tries to download connexion==3.0.6 and gets an error:

```
   uv_resolver::resolver::choose_version package=connexion[swagger-ui]
     uv_resolver::resolver::package_wait package_name=connexion
        0.153084s   0ms DEBUG uv_resolver::resolver Searching for a compatible version of connexion[swagger-ui] (>=2.10.0)
        0.153125s   0ms DEBUG uv_resolver::resolver Selecting: connexion[swagger-ui]==3.0.6 (connexion-3.0.6-py3-none-any.whl)
   uv_resolver::resolver::get_dependencies package=connexion[swagger-ui], version=3.0.6
     uv_resolver::resolver::distributions_wait package_id=connexion-3.0.6
error: There are conflicting versions for `swagger-ui-bundle`: `swagger-ui-bundle>=1.1.0` does not intersect with `swagger-ui-bundle==0.0.9`
```

With pip connexion==2.14.1 is installed:

`pip install --dry-run apache-airflow==2.4.2 -c airflow-constraints.txt`

`Would install Babel-2.10.3 ConfigUpdater-3.1.1 Deprecated-1.2.13 Flask-2.2.2 Flask-AppBuilder-4.1.4 Flask-Babel-2.0.0 Flask-Caching-2.0.1 Flask-JWT-Extended-4.4.4 Flask-Login-0.6.2 Flask-SQLAlchemy-2.5.1 Flask-Session-0.4.0 Flask-WTF-1.0.1 Jinja2-3.1.2 Mako-1.2.3 Markdown-3.4.1 MarkupSafe-2.1.1 PyJWT-2.6.0 PyYAML-6.0 Pygments-2.13.0 SQLAlchemy-1.4.27 SQLAlchemy-JSONField-1.0.0 SQLAlchemy-Utils-0.38.3 WTForms-3.0.1 Werkzeug-2.2.2 alembic-1.8.1 anyio-3.6.2 apache-airflow-2.4.2 apache-airflow-providers-common-sql-1.2.0 apache-airflow-providers-ftp-3.1.0 apache-airflow-providers-http-4.0.0 apache-airflow-providers-imap-3.0.0 apache-airflow-providers-sqlite-3.2.1 apispec-3.3.2 argcomplete-2.0.0 attrs-22.1.0 blinker-1.5 cachelib-0.9.0 cattrs-22.2.0 certifi-2022.9.24 cffi-1.15.1 charset-normalizer-2.1.1 click-8.1.3 clickclick-20.10.2 colorama-0.4.5 colorlog-4.8.0 commonmark-0.9.1 connexion-2.14.1 cron-descriptor-1.2.31 croniter-1.3.7 cryptography-36.0.2 dill-0.3.1.1 dnspython-2.2.1 docutils-0.19 email-validator-1.3.0 exceptiongroup-1.0.0rc9 graphviz-0.20.1 greenlet-1.1.3.post0 gunicorn-20.1.0 h11-0.12.0 httpcore-0.15.0 httpx-0.23.0 idna-3.4 inflection-0.5.1 itsdangerous-2.1.2 jsonschema-4.16.0 lazy-object-proxy-1.7.1 linkify-it-py-2.0.0 lockfile-0.12.2 markdown-it-py-2.1.0 marshmallow-3.18.0 marshmallow-enum-1.5.1 marshmallow-oneofschema-3.0.1 marshmallow-sqlalchemy-0.26.1 mdit-py-plugins-0.3.1 mdurl-0.1.2 pathspec-0.9.0 pendulum-2.1.2 pluggy-1.0.0 prison-0.2.1 psutil-5.9.3 pycparser-2.21 python-daemon-2.3.1 python-nvd3-0.15.0 python-slugify-6.1.2 pytz-2022.5 pytzdata-2020.1 requests-2.28.1 requests-toolbelt-0.10.0 rfc3986-1.5.0 rich-12.6.0 setproctitle-1.3.2 sniffio-1.3.0 sqlparse-0.4.3 swagger-ui-bundle-0.0.9 tabulate-0.9.0 tenacity-8.1.0 termcolor-2.0.1 text-unidecode-1.3 typing_extensions-4.4.0 uc-micro-py-1.0.1 unicodecsv-0.14.1 urllib3-1.26.12 wrapt-1.14.1
`

[uv-output.txt](https://github.com/astral-sh/uv/files/14323597/uv-output.txt)
[pip-output.txt](https://github.com/astral-sh/uv/files/14323614/pip-output.txt)




---

_Renamed from "Conflict due package and package-extra when installing airflow" to "Conflict due to package and package-extra when installing airflow" by @erdembanak on 2024-02-18 18:05_

---

_Label `bug` added by @zanieb on 2024-02-18 20:58_

---

_Label `resolver` added by @zanieb on 2024-02-18 20:58_

---

_Comment by @zanieb on 2024-02-18 20:58_

Thanks for the report!

---

_Comment by @charliermarsh on 2024-02-22 02:51_

This is solved by #1796.

---

_Closed by @charliermarsh on 2024-02-22 02:51_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-02-22 02:51_

---
