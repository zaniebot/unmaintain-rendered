---
number: 2130
title: "The \"standard\" installation use `pyproject.toml` in UV rather than dynamic dependencies via build hooks (comparing to PIP)"
type: issue
state: closed
author: potiuk
labels:
  - bug
  - compatibility
assignees: []
created_at: 2024-03-02T14:38:30Z
updated_at: 2024-03-25T22:26:52Z
url: https://github.com/astral-sh/uv/issues/2130
synced_at: 2026-01-10T01:23:13Z
---

# The "standard" installation use `pyproject.toml` in UV rather than dynamic dependencies via build hooks (comparing to PIP)

---

_Issue opened by @potiuk on 2024-03-02 14:38_

When you install packages using remote url and specify extras, the `--editable` version of extras are used, rather than the dependencies used in wheel. While I don't think it's very well specified which dependencies should be used 

```
uv pip install 'apache-airflow[aiobotocore,amazon,async,celery,cncf-kubernetes,common-io, \
docker,elasticsearch,ftp,google,google-auth,graphviz,grpc,hashicorp,http,ldap,microsoft-azure, \
mysql,odbc,openlineage,pandas,postgres,redis,sendgrid,sftp,slack,snowflake, \
ssh,statsd,uv,virtualenv] @ https://github.com/apache/airflow/archive/main.tar.gz'
```

<details>
  <summary>Here is the output of `uv pip install` output</summary>

> Resolved 359 packages in 2.94s

> Downloaded 154 packages in 2.44s
> Installed 257 packages in 700ms
>  + adal==1.2.7
>  + adlfs==2024.2.0
>  + aiobotocore==2.12.0
>  + aiofiles==23.2.1
>  + aiohttp==3.9.3
>  + aioitertools==0.11.0
>  + aiosignal==1.3.1
>  + amqp==5.2.0
>  + annotated-types==0.6.0
>  + apispec==6.5.0
>  + asn1crypto==1.5.1
>  + async-timeout==4.0.3
>  + asyncssh==2.14.2
>  + authlib==1.3.0
>  + aws-sam-translator==1.85.0
>  + aws-xray-sdk==2.12.1
>  + azure-batch==14.1.0
>  + azure-common==1.1.28
>  + azure-core==1.30.1
>  + azure-cosmos==4.5.1
>  + azure-datalake-store==0.0.53
>  + azure-identity==1.15.0
>  + azure-keyvault-secrets==4.8.0
>  + azure-kusto-data==4.3.1
>  + azure-mgmt-containerinstance==10.1.0
>  + azure-mgmt-containerregistry==10.3.0
>  + azure-mgmt-core==1.4.0
>  + azure-mgmt-cosmosdb==9.4.0
>  + azure-mgmt-datafactory==5.0.0
>  + azure-mgmt-datalake-nspkg==3.0.1
>  + azure-mgmt-datalake-store==0.5.0
>  + azure-mgmt-nspkg==3.0.2
>  + azure-mgmt-resource==23.0.1
>  + azure-mgmt-storage==21.1.0
>  + azure-nspkg==3.0.2
>  + azure-servicebus==7.11.4
>  + azure-storage-blob==12.19.0
>  + azure-storage-file-datalake==12.14.0
>  + azure-storage-file-share==12.15.0
>  + azure-synapse-artifacts==0.18.0
>  + azure-synapse-spark==0.7.0
>  + babel==2.14.0
>  + bcrypt==4.1.2
>  + beautifulsoup4==4.12.3
>  + billiard==4.2.0
>  + boto3==1.34.51
>  + botocore==1.34.51
>  + cachetools==5.3.3
>  + cattrs==23.2.3
>  + celery==5.3.6
>  + cfn-lint==0.85.3
>  + chardet==5.2.0
>  + click-didyoumean==0.3.0
>  + click-plugins==1.1.1
>  + click-repl==0.3.0
>  + colorama==0.4.6
>  - cryptography==42.0.5
>  + cryptography==41.0.7
>  + db-dtypes==1.2.0
>  + decorator==5.1.1
>  + distlib==0.3.8
>  + dnspython==2.6.1
>  + docker==7.0.0
>  + elastic-transport==8.12.0
>  + elasticsearch==8.12.1
>  + email-validator==1.3.1
>  + eventlet==0.35.2
>  + filelock==3.13.1
>  + flask-appbuilder==4.3.11
>  + flask-babel==2.0.0
>  + flask-jwt-extended==4.6.0
>  + flask-limiter==3.5.1
>  + flask-login==0.6.3
>  + flask-sqlalchemy==2.5.1
>  + flower==2.0.1
>  + frozenlist==1.4.1
>  + gcloud-aio-auth==4.2.3
>  + gcloud-aio-bigquery==7.1.0
>  + gcloud-aio-storage==9.2.0
>  + gcsfs==2024.2.0
>  + gevent==24.2.1
>  + google-ads==23.1.0
>  + google-analytics-admin==0.22.6
>  + google-api-core==2.17.1
>  + google-api-python-client==2.120.0
>  + google-auth==2.28.1
>  + google-auth-httplib2==0.2.0
>  + google-auth-oauthlib==1.2.0
>  + google-cloud-aiplatform==1.43.0
>  + google-cloud-appengine-logging==1.4.2
>  + google-cloud-audit-log==0.2.5
>  + google-cloud-automl==2.13.2
>  + google-cloud-batch==0.17.12
>  + google-cloud-bigquery==3.17.2
>  + google-cloud-bigquery-datatransfer==3.15.0
>  + google-cloud-bigquery-storage==2.24.0
>  + google-cloud-bigtable==2.23.0
>  + google-cloud-build==3.23.2
>  + google-cloud-compute==1.17.0
>  + google-cloud-container==2.41.0
>  + google-cloud-core==2.4.1
>  + google-cloud-datacatalog==3.18.2
>  + google-cloud-dataflow-client==0.8.9
>  + google-cloud-dataform==0.5.8
>  + google-cloud-dataplex==1.12.2
>  + google-cloud-dataproc==5.9.2
>  + google-cloud-dataproc-metastore==1.15.2
>  + google-cloud-dlp==3.15.2
>  + google-cloud-kms==2.21.2
>  + google-cloud-language==2.13.2
>  + google-cloud-logging==3.9.0
>  + google-cloud-memcache==1.9.2
>  + google-cloud-monitoring==2.19.2
>  + google-cloud-orchestration-airflow==1.12.0
>  + google-cloud-os-login==2.14.2
>  + google-cloud-pubsub==2.19.7
>  + google-cloud-redis==2.15.2
>  + google-cloud-resource-manager==1.12.2
>  + google-cloud-run==0.10.4
>  + google-cloud-secret-manager==2.18.2
>  + google-cloud-spanner==3.42.0
>  + google-cloud-speech==2.25.0
>  + google-cloud-storage==2.14.0
>  + google-cloud-storage-transfer==1.11.2
>  + google-cloud-tasks==2.16.2
>  + google-cloud-texttospeech==2.16.2
>  + google-cloud-translate==3.15.2
>  + google-cloud-videointelligence==2.13.2
>  + google-cloud-vision==3.7.1
>  + google-cloud-workflows==1.14.2
>  + google-crc32c==1.5.0
>  + google-resumable-media==2.7.0
>  + graphql-core==3.2.3
>  + graphviz==0.20.1
>  + greenlet==3.0.3
>  + grpc-google-iam-v1==0.13.0
>  + grpc-interceptor==0.15.4
>  + grpcio-gcp==0.2.2
>  + grpcio-status==1.62.0
>  + httplib2==0.22.0
>  + humanize==4.9.0
>  + hvac==2.1.0
>  + ijson==3.2.3
>  + isodate==0.6.1
>  + jmespath==1.0.1
>  + joserfc==0.9.0
>  + jschema-to-python==1.2.3
>  + json-merge-patch==0.2
>  + jsondiff==2.0.0
>  + jsonpatch==1.33
>  + jsonpath-ng==1.6.1
>  + jsonpickle==3.0.3
>  + jsonpointer==2.4
>  + jsonschema-path==0.3.2
>  + junit-xml==1.9
>  + kombu==5.3.5
>  + kubernetes==29.0.0
>  + kubernetes-asyncio==29.0.0
>  + ldap3==2.9.1
>  + limits==3.9.0
>  + looker-sdk==24.2.0
>  + lxml==5.1.0
>  + marshmallow-sqlalchemy==0.26.1
>  + more-itertools==10.2.0
>  + moto==5.0.2
>  + mpmath==1.3.0
>  + msal==1.27.0
>  + msal-extensions==1.1.0
>  + msrest==0.7.1
>  + msrestazure==0.6.4
>  + multidict==6.0.5
>  + mypy-boto3-appflow==1.34.0
>  + mypy-boto3-rds==1.34.50
>  + mypy-boto3-redshift-data==1.34.0
>  + mypy-boto3-s3==1.34.14
>  + mysql-connector-python==8.3.0
>  + mysqlclient==2.2.4
>  + networkx==3.1
>  + numpy==1.24.4
>  + oauthlib==3.2.2
>  + openapi-schema-validator==0.6.2
>  + openapi-spec-validator==0.7.1
>  + openlineage-integration-common==1.9.1
>  + openlineage-python==1.9.1
>  + openlineage-sql==1.9.1
>  + ordered-set==4.1.0
>  + pandas==2.0.3
>  + pandas-gbq==0.21.0
>  + paramiko==3.4.0
>  + pathable==0.4.3
>  + pbr==6.0.0
>  + platformdirs==3.11.0
>  + ply==3.11
>  + portalocker==2.8.2
>  + prison==0.2.1
>  + prometheus-client==0.20.0
>  + prompt-toolkit==3.0.43
>  + proto-plus==1.23.0
>  + psycopg2-binary==2.9.9
>  + py-partiql-parser==0.5.1
>  + pyarrow==15.0.0
>  + pyasn1==0.5.1
>  + pyasn1-modules==0.3.0
>  + pyathena==3.3.0
>  + pydantic==2.6.3
>  + pydantic-core==2.16.3
>  + pydata-google-auth==1.8.2
>  + pynacl==1.5.0
>  + pyodbc==5.1.0
>  + pyopenssl==24.0.0
>  + pyparsing==3.1.1
>  + pyspnego==0.10.2
>  + python-dotenv==1.0.1
>  + python-http-client==3.3.7
>  + python-ldap==3.4.4
>  + pywinrm==0.4.3
>  + redis==4.6.0
>  + redshift-connector==2.1.0
>  - referencing==0.33.0
>  + referencing==0.31.1
>  + regex==2023.12.25
>  + requests-ntlm==1.2.0
>  + requests-oauthlib==1.3.1
>  + requests-toolbelt==1.0.0
>  + responses==0.25.0
>  + rsa==4.9
>  + s3fs==2024.2.0
>  + s3transfer==0.10.0
>  + sarif-om==1.0.4
>  + scramp==1.4.4
>  + sendgrid==6.11.0
>  + shapely==2.0.3
>  + slack-sdk==3.27.1
>  + snowflake-connector-python==3.7.1
>  + snowflake-sqlalchemy==1.5.1
>  + sortedcontainers==2.4.0
>  + soupsieve==2.5
>  + sqlalchemy-bigquery==1.10.0
>  + sqlalchemy-redshift==0.8.14
>  + sqlalchemy-spanner==1.6.2
>  + sqlalchemy-utils==0.41.1
>  + sqlparse==0.4.4
>  + sshtunnel==0.4.0
>  + starkbank-ecdsa==2.2.0
>  + statsd==4.0.1
>  + sympy==1.12
>  + tomlkit==0.12.4
>  + tornado==6.4
>  + uritemplate==4.1.1
>  - urllib3==2.2.1
>  + urllib3==1.26.18
>  + vine==5.1.0
>  + virtualenv==20.25.1
>  + watchtower==3.0.1
>  + wcwidth==0.2.13
>  + websocket-client==1.7.0
>  + xmltodict==0.13.0
>  + yarl==1.9.4
>  + zope-event==5.0
>  + zope-interface==6.2


</details>

Compare it with the equivalent `pip` result:


```
uv pip install 'apache-airflow[aiobotocore,amazon,async,celery,cncf-kubernetes,common-io, \
docker,elasticsearch,ftp,google,google-auth,graphviz,grpc,hashicorp,http,ldap,microsoft-azure, \
mysql,odbc,openlineage,pandas,postgres,redis,sendgrid,sftp,slack,snowflake, \
ssh,statsd,uv,virtualenv] @ https://github.com/apache/airflow/archive/main.tar.gz'
```

<details>
   <summary>Result of `pip install`</summary>

> Installing collected packages: wcwidth, unicodecsv, text-unidecode, statsd, starkbank-ecdsa, sortedcontainers, pytz, ply, lockfile, json-merge-patch, ijson, distlib, cron-descriptor, colorlog, azure-nspkg, azure-common, asn1crypto, zope.interface, zope.event, zipp, wrapt, websocket-client, vine, urllib3, uritemplate, uc-micro-py, tzdata, typing-extensions, tornado, tomlkit, termcolor, tenacity, tabulate, sqlparse, sqlalchemy, soupsieve, sniffio, slack_sdk, six, setproctitle, scramp, rpds-py, PyYAML, python-slugify, python-http-client, python-dotenv, pyparsing, pyodbc, pyjwt, pygments, pycparser, pyasn1, psycopg2-binary, psutil, protobuf, prompt-toolkit, prometheus-client, portalocker, pluggy, platformdirs, pkgutil-resolve-name, pathspec, packaging, ordered-set, opentelemetry-semantic-conventions, openlineage-sql, oauthlib, numpy, mysqlclient, mysql-connector-python, multidict, more-itertools, mdurl, markupsafe, lxml, lazy-object-proxy, jsonpath_ng, jmespath, itsdangerous, inflection, idna, humanize, h11, grpcio, greenlet, graphviz, google-re2, google-crc32c, fsspec, frozenlist, filelock, exceptiongroup, docutils, dnspython, dill, decorator, configupdater, colorama, click, charset-normalizer, chardet, certifi, cachetools, cachelib, blinker, billiard, bcrypt, backports.zoneinfo, backoff, Babel, azure-mgmt-nspkg, attrs, async-timeout, argcomplete, aiofiles, yarl, wtforms, werkzeug, virtualenv, universal-pathlib, sqlalchemy-utils, sqlalchemy_redshift, sqlalchemy-jsonfield, shapely, sendgrid, rsa, rfc3339-validator, requests, referencing, redis, python-dateutil, python-daemon, pyasn1-modules, pyarrow, proto-plus, prison, opentelemetry-proto, marshmallow, markdown-it-py, Mako, linkify-it-py, ldap3, jinja2, isodate, importlib-resources, importlib-metadata, httplib2, httpcore, gunicorn, grpcio-gcp, grpc-interceptor, googleapis-common-protos, google-resumable-media, gevent, eventlet, email-validator, elastic-transport, deprecated, clickclick, click-repl, click-plugins, click-didyoumean, cffi, cattrs, beautifulsoup4, azure-mgmt-datalake-nspkg, asgiref, apispec, anyio, amqp, aiosignal, aioitertools, time-machine, rich, requests_toolbelt, requests-oauthlib, python-nvd3, python-ldap, pynacl, pandas, opentelemetry-exporter-otlp-proto-common, opentelemetry-api, openlineage-python, mdit-py-plugins, marshmallow-sqlalchemy, marshmallow-oneofschema, looker-sdk, limits, kombu, jsonschema-specifications, hvac, httpx, grpcio-status, google-cloud-audit-log, google-auth, flask, elasticsearch, docker, cryptography, croniter, botocore, azure-core, alembic, aiohttp, s3transfer, rich-argparse, PyOpenSSL, pendulum, paramiko, opentelemetry-sdk, openlineage-integration-common, msrest, kubernetes_asyncio, kubernetes, jsonschema, grpc-google-iam-v1, google-auth-oauthlib, google-auth-httplib2, google-api-core, gcloud-aio-auth, flask-wtf, Flask-SQLAlchemy, flask-session, flask-login, Flask-Limiter, Flask-JWT-Extended, flask-caching, Flask-Babel, db-dtypes, celery, azure-storage-file-share, azure-storage-blob, azure-servicebus, azure-mgmt-core, azure-keyvault-secrets, azure-cosmos, authlib, asyncssh, aiobotocore, adal, sshtunnel, snowflake-connector-python, pydata-google-auth, opentelemetry-exporter-otlp-proto-http, opentelemetry-exporter-otlp-proto-grpc, msrestazure, msal, google-cloud-core, google-api-python-client, google-ads, gcloud-aio-storage, gcloud-aio-bigquery, flower, flask-appbuilder, connexion, boto3, azure-synapse-spark, azure-synapse-artifacts, azure-storage-file-datalake, azure-mgmt-storage, azure-mgmt-resource, azure-mgmt-datafactory, azure-mgmt-cosmosdb, azure-mgmt-containerregistry, azure-mgmt-containerinstance, watchtower, snowflake-sqlalchemy, redshift_connector, PyAthena, opentelemetry-exporter-otlp, msal-extensions, google-cloud-workflows, google-cloud-vision, google-cloud-videointelligence, google-cloud-translate, google-cloud-texttospeech, google-cloud-tasks, google-cloud-storage-transfer, google-cloud-storage, google-cloud-speech, google-cloud-spanner, google-cloud-secret-manager, google-cloud-run, google-cloud-resource-manager, google-cloud-redis, google-cloud-pubsub, google-cloud-os-login, google-cloud-orchestration-airflow, google-cloud-monitoring, google-cloud-memcache, google-cloud-language, google-cloud-kms, google-cloud-dlp, google-cloud-dataproc-metastore, google-cloud-dataproc, google-cloud-dataplex, google-cloud-dataform, google-cloud-dataflow-client, google-cloud-datacatalog, google-cloud-container, google-cloud-compute, google-cloud-build, google-cloud-bigtable, google-cloud-bigquery-storage, google-cloud-bigquery-datatransfer, google-cloud-bigquery, google-cloud-batch, google-cloud-automl, google-cloud-appengine-logging, google-analytics-admin, azure-mgmt-datalake-store, azure-datalake-store, azure-batch, sqlalchemy-spanner, sqlalchemy-bigquery, pandas-gbq, google-cloud-logging, google-cloud-aiplatform, gcsfs, azure-identity, azure-kusto-data, adlfs, apache-airflow-providers-smtp, apache-airflow-providers-imap, apache-airflow-providers-http, apache-airflow-providers-ftp, apache-airflow-providers-fab, apache-airflow-providers-common-sql, apache-airflow-providers-common-io, apache-airflow-providers-sqlite, apache-airflow-providers-ssh, apache-airflow-providers-snowflake, apache-airflow-providers-slack, apache-airflow-providers-sftp, apache-airflow-providers-sendgrid, apache-airflow-providers-redis, apache-airflow-providers-postgres, apache-airflow-providers-openlineage, apache-airflow-providers-odbc, apache-airflow-providers-mysql, apache-airflow-providers-microsoft-azure, apache-airflow-providers-hashicorp, apache-airflow-providers-grpc, apache-airflow-providers-google, apache-airflow-providers-elasticsearch, apache-airflow-providers-docker, apache-airflow-providers-cncf-kubernetes, apache-airflow-providers-celery, apache-airflow-providers-amazon

</details>

Note - all the `apache-airflow-providers-*` packages missing in case of `uv pip install`.

The problem is likely that the installation uses directly `pyproject.toml` to install dependencies, however for such remote installation (and without `--editable` install at that - but even if it would be specified, `--editable` makes no sense for remote install) the dependencies should be the same as in packaged .whl file and it makes the installation of `uv` in this case non-compliant with PEP 517.

A bit more context: Airlfow uses hatchling build backend, and utilzes PEP 517 compliant build_hooks (https://peps.python.org/pep-0517/#build-wheel) to modify the `--editable`  extras into `wheel` extras on the flight. So for example `[celery]` requirement in pyproject.toml ( https://github.com/apache/airflow/blob/main/pyproject.toml#L641) is this:

```
celery = [ # source: airflow/providers/celery/provider.yaml
  "celery>=5.3.0,<6,!=5.3.3,!=5.3.2",
  "flower>=1.0.0",
  "google-re2>=1.0",
]
```

However the hatchling [build hook of ours](https://github.com/apache/airflow/blob/main/hatch_build.py), when preparing `wheel` package, replaces this extra with:
```
"apache-airflow-providers-celery"
```

This is the way how we are dealing with our monorepo where `--editable` "extra" just installs dependencies of our providers, while the "wheel" extra install actual provider (and transitively dependencies of that provider).

I **believe** that PEP-517 compliant way of installing a package from remote URL should actually build the wheel file first using the build backend the project has defined in `pyproject.toml` and only then install such a wheel file (this is exactly what `pip` does under the hood when installing package from remote url - treating it the same way as installind an sdist package (which the remote URL is equivalent of).






---

_Label `bug` added by @charliermarsh on 2024-03-02 22:54_

---

_Label `compatibility` added by @charliermarsh on 2024-03-02 22:54_

---

_Renamed from "Installing from remote URL uses `--editable` version of dependencies in UV (comparing to PIP)" to "Installing from remote URL uses `pyproject.toml` version of dependencies in UV (comparing to PIP)" by @potiuk on 2024-03-24 22:00_

---

_Renamed from "Installing from remote URL uses `pyproject.toml` version of dependencies in UV (comparing to PIP)" to "Installing from remote URL uses `pyproject.toml`  in UV rather than dynamic deypendencies via build hooks (comparing to PIP)" by @potiuk on 2024-03-24 22:09_

---

_Renamed from "Installing from remote URL uses `pyproject.toml`  in UV rather than dynamic deypendencies via build hooks (comparing to PIP)" to "Installing from sources uses `pyproject.toml`  in UV rather than dynamic deypendencies via build hooks (comparing to PIP)" by @potiuk on 2024-03-24 22:22_

---

_Renamed from "Installing from sources uses `pyproject.toml`  in UV rather than dynamic deypendencies via build hooks (comparing to PIP)" to "The "standard" installation use `pyproject.toml`  in UV rather than dynamic deypendencies via build hooks (comparing to PIP)" by @potiuk on 2024-03-24 22:32_

---

_Renamed from "The "standard" installation use `pyproject.toml`  in UV rather than dynamic deypendencies via build hooks (comparing to PIP)" to "The "standard" installation use `pyproject.toml` in UV rather than dynamic deypendencies via build hooks (comparing to PIP)" by @potiuk on 2024-03-24 22:32_

---

_Comment by @potiuk on 2024-03-24 22:50_

I have an update. 

I realized that the problem could be, because we were not really PEP 621 compliant with Airflow becaused we mixed "dynamic" and "static" project properties in Airflow. The `pyproject.toml` contained  the "dependencies" and "optional-dependencies" but also build hooks modified them.

However as of today we fixed it in `apache-airflow'  in https://github.com/apache/airflow/pull/38439 and the problem remains.

Both "dependencies" and "optional-dependencies" are set as dynamic in `pyproject.toml`: https://github.com/apache/airflow/blob/main/pyproject.toml#L64 :

```
dynamic = ["version", "optional-dependencies", "dependencies"]
```

It looks like `uv` - unlike `pip` does not use build hooks to determine `standard` dynamic properties as defined in `pyproject.toml` - only uses the hooks to determine `editable` dependencies

## PIP case

With `pip` - they are both properly set dynamically by PEP 517 compliant build hooks (via hatchling) - https://github.com/apache/airflow/blob/main/hatch_build.py - you can check it by checking out the repo and running those commands (I separated out the relevant installed packages to show the difference)

1) `pip install "apache-airflow[aws] @ file:///Users/jarek/code/airflow"` -> correctly installs "wheel" version of airflow with wheel `[aws]` variant of extra - including `apache-airflow-providers-amazon` - but not including devel deps like `moto`.

<details>
  <summary>Output</summary>

> Successfully installed Babel-2.14.0 Flask-Babel-2.0.0 Flask-JWT-Extended-4.6.0 Flask-Limiter-3.5.1 Flask-SQLAlchemy-2.5.1 Mako-1.3.2 PyAthena-3.5.1 PyYAML-6.0.1 WTForms-3.1.2 aiohttp-3.9.3 aiosignal-1.3.1 alembic-1.13.1 anyio-4.3.0 apache-airflow-2.9.0.dev0 

> **apache-airflow-providers-amazon-8.19.0** 

> apache-airflow-providers-common-io-1.3.0 apache-airflow-providers-common-sql-1.11.1 apache-airflow-providers-fab-1.0.2.dev2 apache-airflow-providers-ftp-3.7.0 apache-airflow-providers-http-4.10.0 apache-airflow-providers-imap-3.5.0 apache-airflow-providers-smtp-1.6.1 apache-airflow-providers-sqlite-3.7.1 apispec-6.6.0 argcomplete-3.2.3 asgiref-3.8.1 asn1crypto-1.5.1 attrs-23.2.0 beautifulsoup4-4.12.3 blinker-1.7.0 boto3-1.34.69 botocore-1.34.69 cachelib-0.9.0 certifi-2024.2.2 cffi-1.16.0 charset-normalizer-3.3.2 click-8.1.7 clickclick-20.10.2 colorama-0.4.6 colorlog-4.8.0 configupdater-3.2 connexion-2.14.2 cron-descriptor-1.4.3 croniter-2.0.3 cryptography-42.0.5 deprecated-1.2.14 dill-0.3.8 dnspython-2.6.1 docutils-0.20.1 email-validator-2.1.1 flask-2.2.5 flask-appbuilder-4.4.1 flask-caching-2.1.0 flask-login-0.6.3 flask-session-0.5.0 flask-wtf-1.2.1 frozenlist-1.4.1 fsspec-2024.3.1 google-re2-1.1 googleapis-common-protos-1.63.0 grpcio-1.62.1 gunicorn-21.2.0 h11-0.14.0 httpcore-1.0.4 httpx-0.27.0 idna-3.6 importlib-metadata-6.11.0 importlib-resources-6.4.0 inflection-0.5.1 itsdangerous-2.1.2 jinja2-3.1.3 jmespath-1.0.1 jsonpath_ng-1.6.1 jsonschema-4.21.1 jsonschema-specifications-2023.12.1 lazy-object-proxy-1.10.0 limits-3.10.1 linkify-it-py-2.0.3 lockfile-0.12.2 lxml-5.1.0 markdown-it-py-3.0.0 markupsafe-2.1.5 marshmallow-3.21.1 marshmallow-oneofschema-3.1.1 marshmallow-sqlalchemy-0.28.2 mdit-py-plugins-0.4.0 mdurl-0.1.2 more-itertools-10.2.0 multidict-6.0.5 opentelemetry-api-1.23.0 opentelemetry-exporter-otlp-1.23.0 opentelemetry-exporter-otlp-proto-common-1.23.0 opentelemetry-exporter-otlp-proto-grpc-1.23.0 opentelemetry-exporter-otlp-proto-http-1.23.0 opentelemetry-proto-1.23.0 opentelemetry-sdk-1.23.0 opentelemetry-semantic-conventions-0.44b0 ordered-set-4.1.0 packaging-24.0 pathspec-0.12.1 pendulum-3.0.0 pluggy-1.4.0 ply-3.11 prison-0.2.1 protobuf-4.25.3 psutil-5.9.8 pycparser-2.21 pygments-2.17.2 pyjwt-2.8.0 python-daemon-3.0.1 python-dateutil-2.9.0.post0 python-nvd3-0.15.0 python-slugify-8.0.4 pytz-2024.1 redshift_connector-2.1.0 referencing-0.34.0 requests-2.31.0 requests_toolbelt-1.0.0 rfc3339-validator-0.1.4 rich-13.7.1 rich-argparse-1.4.0 rpds-py-0.18.0 s3transfer-0.10.1 scramp-1.4.4 setproctitle-1.3.3 six-1.16.0 sniffio-1.3.1 soupsieve-2.5 sqlalchemy-1.4.52 sqlalchemy-jsonfield-1.0.2 sqlalchemy-utils-0.41.2 sqlalchemy_redshift-0.8.14 sqlparse-0.4.4 tabulate-0.9.0 tenacity-8.2.3 termcolor-2.4.0 text-unidecode-1.3 time-machine-2.14.1 typing-extensions-4.10.0 tzdata-2024.1 uc-micro-py-1.0.3 unicodecsv-0.14.1 universal-pathlib-0.2.2 urllib3-2.2.1 watchtower-3.1.0 werkzeug-2.2.3 wrapt-1.16.0 yarl-1.9.4 zipp-3.18.1

</details>

   2) `pip install -e "/Users/jarek/code/airflow[aws]"` -> correctly install "editable" version of airflow with editable version of `[aws]` variant of extra - not includoing `apaxhe-airflow-providers-amazon` 

<details>
  <summary>Output</summary>

> Successfully installed Mako-1.3.2 PyAthena-3.5.1 PyYAML-6.0.1 aiobotocore-2.12.1 aiohttp-3.9.3 aioitertools-0.11.0 aiosignal-1.3.1 alembic-1.13.1 annotated-types-0.6.0 anyio-4.3.0 apache-airflow-2.9.0.dev0 argcomplete-3.2.3 asgiref-3.8.1 asn1crypto-1.5.1 attrs-23.2.0 aws-sam-translator-1.86.0 aws_xray_sdk-2.13.0 beautifulsoup4-4.12.3 blinker-1.7.0 boto3-1.34.51 botocore-1.34.51 cachelib-0.9.0 certifi-2024.2.2 cffi-1.16.0 cfn-lint-0.86.1 charset-normalizer-3.3.2 click-8.1.7 clickclick-20.10.2 colorlog-4.8.0 configupdater-3.2 connexion-2.14.2 cron-descriptor-1.4.3 croniter-2.0.3 cryptography-42.0.5 deprecated-1.2.14 dill-0.3.8 docker-7.0.0 docutils-0.20.1 flask-2.2.5 flask-caching-2.1.0 flask-session-0.5.0 flask-wtf-1.2.1 frozenlist-1.4.1 fsspec-2024.3.1 google-re2-1.1 googleapis-common-protos-1.63.0 graphql-core-3.2.3 grpcio-1.62.1 gunicorn-21.2.0 h11-0.14.0 httpcore-1.0.4 httpx-0.27.0 idna-3.6 importlib-metadata-6.11.0 inflection-0.5.1 itsdangerous-2.1.2 jinja2-3.1.3 jmespath-1.0.1 joserfc-0.9.0 jschema-to-python-1.2.3 jsondiff-2.0.0 jsonpatch-1.33 jsonpath_ng-1.6.1 jsonpickle-3.0.3 jsonpointer-2.4 jsonschema-4.21.1 jsonschema-path-0.3.2 jsonschema-specifications-2023.12.1 junit-xml-1.9 lazy-object-proxy-1.10.0 linkify-it-py-2.0.3 lockfile-0.12.2 lxml-5.1.0 markdown-it-py-3.0.0 markupsafe-2.1.5 marshmallow-3.21.1 marshmallow-oneofschema-3.1.1 mdit-py-plugins-0.4.0 mdurl-0.1.2 more-itertools-10.2.0 

> **moto-5.0.3** 

> mpmath-1.3.0 multidict-6.0.5 mypy-boto3-appflow-1.34.0 mypy-boto3-rds-1.34.65 mypy-boto3-redshift-data-1.34.0 mypy-boto3-s3-1.34.65 networkx-3.2.1 openapi-schema-validator-0.6.2 openapi-spec-validator-0.7.1 opentelemetry-api-1.23.0 opentelemetry-exporter-otlp-1.23.0 opentelemetry-exporter-otlp-proto-common-1.23.0 opentelemetry-exporter-otlp-proto-grpc-1.23.0 opentelemetry-exporter-otlp-proto-http-1.23.0 opentelemetry-proto-1.23.0 opentelemetry-sdk-1.23.0 opentelemetry-semantic-conventions-0.44b0 packaging-24.0 pathable-0.4.3 pathspec-0.12.1 pbr-6.0.0 pendulum-3.0.0 pluggy-1.4.0 ply-3.11 protobuf-4.25.3 psutil-5.9.8 py-partiql-parser-0.5.1 pycparser-2.21 pydantic-2.6.4 pydantic-core-2.16.3 pygments-2.17.2 pyjwt-2.8.0 pyparsing-3.1.2 python-daemon-3.0.1 python-dateutil-2.9.0.post0 python-nvd3-0.15.0 python-slugify-8.0.4 pytz-2024.1 redshift_connector-2.1.0 referencing-0.31.1 regex-2023.12.25 requests-2.31.0 requests_toolbelt-1.0.0 responses-0.25.0 rfc3339-validator-0.1.4 rich-13.7.1 rich-argparse-1.4.0 rpds-py-0.18.0 s3fs-2024.3.1 s3transfer-0.10.1 sarif-om-1.0.4 scramp-1.4.4 setproctitle-1.3.3 six-1.16.0 sniffio-1.3.1 soupsieve-2.5 sqlalchemy-1.4.52 sqlalchemy-jsonfield-1.0.2 sqlalchemy_redshift-0.8.14 sqlparse-0.4.4 sympy-1.12 tabulate-0.9.0 tenacity-8.2.3 termcolor-2.4.0 text-unidecode-1.3 time-machine-2.14.1 typing-extensions-4.10.0 tzdata-2024.1 uc-micro-py-1.0.3 unicodecsv-0.14.1 universal-pathlib-0.2.2 urllib3-2.0.7 watchtower-3.1.0 werkzeug-2.2.3 wrapt-1.16.0 wtforms-3.1.2 xmltodict-0.13.0 yarl-1.9.4 zipp-3.18.1
</details>

3) Similarly - remote installtion via URL uses "wheel" dependency versions: `pip install "apache-airflow[aws] @ https://github.com/apache/airflow/archive/main.tar.gz"`:

<details>
  <summary>Ouput</summary>

> Successfully installed Babel-2.14.0 Flask-Babel-2.0.0 Flask-JWT-Extended-4.6.0 Flask-Limiter-3.5.1 Flask-SQLAlchemy-2.5.1 Mako-1.3.2 PyAthena-3.5.1 PyYAML-6.0.1 WTForms-3.1.2 aiohttp-3.9.3 aiosignal-1.3.1 alembic-1.13.1 anyio-4.3.0 

> **apache-airflow-providers-amazon-8.19.0**

> apache-airflow-providers-common-io-1.3.0 apache-airflow-providers-common-sql-1.11.1 apache-airflow-providers-fab-1.0.2.dev2 apache-airflow-providers-ftp-3.7.0 apache-airflow-providers-http-4.10.0 apache-airflow-providers-imap-3.5.0 apache-airflow-providers-smtp-1.6.1 apache-airflow-providers-sqlite-3.7.1 apispec-6.6.0 argcomplete-3.2.3 asgiref-3.8.1 asn1crypto-1.5.1 attrs-23.2.0 beautifulsoup4-4.12.3 blinker-1.7.0 boto3-1.34.69 botocore-1.34.69 cachelib-0.9.0 certifi-2024.2.2 cffi-1.16.0 charset-normalizer-3.3.2 click-8.1.7 clickclick-20.10.2 colorama-0.4.6 colorlog-4.8.0 configupdater-3.2 connexion-2.14.2 cron-descriptor-1.4.3 croniter-2.0.3 cryptography-42.0.5 deprecated-1.2.14 dill-0.3.8 dnspython-2.6.1 docutils-0.20.1 email-validator-2.1.1 flask-2.2.5 flask-appbuilder-4.4.1 flask-caching-2.1.0 flask-login-0.6.3 flask-session-0.5.0 flask-wtf-1.2.1 frozenlist-1.4.1 fsspec-2024.3.1 google-re2-1.1 googleapis-common-protos-1.63.0 grpcio-1.62.1 gunicorn-21.2.0 h11-0.14.0 httpcore-1.0.4 httpx-0.27.0 idna-3.6 importlib-metadata-6.11.0 importlib-resources-6.4.0 inflection-0.5.1 itsdangerous-2.1.2 jinja2-3.1.3 jmespath-1.0.1 jsonpath_ng-1.6.1 jsonschema-4.21.1 jsonschema-specifications-2023.12.1 lazy-object-proxy-1.10.0 limits-3.10.1 linkify-it-py-2.0.3 lockfile-0.12.2 lxml-5.1.0 markdown-it-py-3.0.0 markupsafe-2.1.5 marshmallow-3.21.1 marshmallow-oneofschema-3.1.1 marshmallow-sqlalchemy-0.28.2 mdit-py-plugins-0.4.0 mdurl-0.1.2 more-itertools-10.2.0 multidict-6.0.5 opentelemetry-api-1.23.0 opentelemetry-exporter-otlp-1.23.0 opentelemetry-exporter-otlp-proto-common-1.23.0 opentelemetry-exporter-otlp-proto-grpc-1.23.0 opentelemetry-exporter-otlp-proto-http-1.23.0 opentelemetry-proto-1.23.0 opentelemetry-sdk-1.23.0 opentelemetry-semantic-conventions-0.44b0 ordered-set-4.1.0 packaging-24.0 pathspec-0.12.1 pendulum-3.0.0 pluggy-1.4.0 ply-3.11 prison-0.2.1 protobuf-4.25.3 psutil-5.9.8 pycparser-2.21 pygments-2.17.2 pyjwt-2.8.0 python-daemon-3.0.1 python-dateutil-2.9.0.post0 python-nvd3-0.15.0 python-slugify-8.0.4 pytz-2024.1 redshift_connector-2.1.0 referencing-0.34.0 requests-2.31.0 requests_toolbelt-1.0.0 rfc3339-validator-0.1.4 rich-13.7.1 rich-argparse-1.4.0 rpds-py-0.18.0 s3transfer-0.10.1 scramp-1.4.4 setproctitle-1.3.3 six-1.16.0 sniffio-1.3.1 soupsieve-2.5 sqlalchemy-1.4.52 sqlalchemy-jsonfield-1.0.2 sqlalchemy-utils-0.41.2 sqlalchemy_redshift-0.8.14 sqlparse-0.4.4 tabulate-0.9.0 tenacity-8.2.3 termcolor-2.4.0 text-unidecode-1.3 time-machine-2.14.1 typing-extensions-4.10.0 tzdata-2024.1 uc-micro-py-1.0.3 unicodecsv-0.14.1 universal-pathlib-0.2.2 urllib3-2.2.1 watchtower-3.1.0 werkzeug-2.2.3 wrapt-1.16.0 yarl-1.9.4 zipp-3.18.1

</details>

## UV case

The same exercise with `uv` produced very different (incorrect) results:

I run it with the latest as of today `uv` version:  0.1.24.

1) `uv pip install "apache-airflow[aws] @ file:///Users/jarek/code/airflow"` 

This wrongly installs just airflow package and misses both - dynamic "dependenciess" and dynamic "optional-dependencies" that should be derived from running the build hook.

<details>
  <summary>Output</summary>

> Resolved 1 package in 0.42ms
>    Built apache-airflow @ file:///Users/jarek/code/airflow                                                                                                                                                                                                                                          > Downloaded 1 package in 5.89s
> Installed 1 package in 10ms
>  + apache-airflow==2.9.0.dev0 (from file:///Users/jarek/code/airflow)

</details>

2) `uv pip install -e "/Users/jarek/code/airflow[aws]"`

This correctly installs dynamically derived both "dependencies" and "optional-dependencies". See  (expected) lack of "apache-airflow-providers-amazon" and presence of "moto" - both indicate that `aws` extra was correctly calculated with the build hook.

<details>
  <summary>Output</summary>

> uv pip install -e "/Users/jarek/code/airflow[aws]"
>   Built file:///Users/jarek/code/airflow                                                                                                                                                                                                                                                           > Built 1 editable in 4.90s
> Resolved 161 packages in 6.94s
>   Built python-nvd3==0.15.0
>    Built lazy-object-proxy==1.10.0
>    Built unicodecsv==0.14.1                                                                                                                                                                                                                                                                         > Downloaded 156 packages in 14.87s
> Installed 160 packages in 219ms
> + aiobotocore==2.12.1
> + aiohttp==3.9.3
> + aioitertools==0.11.0
> + aiosignal==1.3.1
> + alembic==1.13.1
> + annotated-types==0.6.0
> + anyio==4.3.0
> + apache-airflow==2.9.0.dev0 (from file:///Users/jarek/code/airflow)
> + argcomplete==3.2.3
> + asgiref==3.8.1
> + asn1crypto==1.5.1
> + attrs==23.2.0
> + aws-sam-translator==1.86.0
> + aws-xray-sdk==2.13.0
> + beautifulsoup4==4.12.3
> + blinker==1.7.0
> + boto3==1.34.51
> + botocore==1.34.51
> + cachelib==0.9.0
> + certifi==2024.2.2
> + cffi==1.16.0
> + cfn-lint==0.86.1
> + charset-normalizer==3.3.2
> + click==8.1.7
> + clickclick==20.10.2
> + colorlog==4.8.0
> + configupdater==3.2
> + connexion==2.14.2
> + cron-descriptor==1.4.3
> + croniter==2.0.3
> + cryptography==42.0.5
> + deprecated==1.2.14
> + dill==0.3.8
> + docker==7.0.0
> + docutils==0.20.1
> + flask==2.2.5
> + flask-caching==2.1.0
> + flask-session==0.5.0
> + flask-wtf==1.2.1
> + frozenlist==1.4.1
> + fsspec==2024.3.1
> + google-re2==1.1
> + googleapis-common-protos==1.63.0
> + graphql-core==3.2.3
> + grpcio==1.62.1
> + gunicorn==21.2.0
> + h11==0.14.0
> + httpcore==1.0.4
> + httpx==0.27.0
> + idna==3.6
> + importlib-metadata==6.11.0
> + inflection==0.5.1
> + itsdangerous==2.1.2
> + jinja2==3.1.3
> + jmespath==1.0.1
> + joserfc==0.9.0
> + jschema-to-python==1.2.3
> + jsondiff==2.0.0
> + jsonpatch==1.33
> + jsonpath-ng==1.6.1
> + jsonpickle==3.0.3
> + jsonpointer==2.4
v + jsonschema==4.21.1
> + jsonschema-path==0.3.2
> + jsonschema-specifications==2023.12.1
> + junit-xml==1.9
> + lazy-object-proxy==1.10.0
> + linkify-it-py==2.0.3
> + lockfile==0.12.2
> + lxml==5.1.0
> + mako==1.3.2
> + markdown-it-py==3.0.0
> + markupsafe==2.1.5
> + marshmallow==3.21.1
> + marshmallow-oneofschema==3.1.1
> + mdit-py-plugins==0.4.0
> + mdurl==0.1.2
> + more-itertools==10.2.0

> **+ moto==5.0.3**

> + mpmath==1.3.0
> + multidict==6.0.5
> + mypy-boto3-appflow==1.34.0
> + mypy-boto3-rds==1.34.65
> + mypy-boto3-redshift-data==1.34.0
> + mypy-boto3-s3==1.34.65
> + networkx==3.2.1
> + openapi-schema-validator==0.6.2
> + openapi-spec-validator==0.7.1
> + opentelemetry-api==1.23.0
> + opentelemetry-exporter-otlp==1.23.0
> + opentelemetry-exporter-otlp-proto-common==1.23.0
> + opentelemetry-exporter-otlp-proto-grpc==1.23.0
> + opentelemetry-exporter-otlp-proto-http==1.23.0
> + opentelemetry-proto==1.23.0
> + opentelemetry-sdk==1.23.0
> + opentelemetry-semantic-conventions==0.44b0
> + packaging==24.0
> + pathable==0.4.3
> + pathspec==0.12.1
> + pbr==6.0.0
> + pendulum==3.0.0
> + pluggy==1.4.0
> + ply==3.11
> + protobuf==4.25.3
> + psutil==5.9.8
> + py-partiql-parser==0.5.1
> + pyathena==3.5.1
> + pycparser==2.21
> + pydantic==2.6.4
> + pydantic-core==2.16.3
> + pygments==2.17.2
> + pyjwt==2.8.0
> + pyparsing==3.1.2
> + python-daemon==3.0.1
> + python-dateutil==2.9.0.post0
> + python-nvd3==0.15.0
> + python-slugify==8.0.4
> + pytz==2024.1
> + pyyaml==6.0.1
> + redshift-connector==2.1.0
> + referencing==0.31.1
> + regex==2023.12.25
> + requests==2.31.0
> + requests-toolbelt==1.0.0
> + responses==0.25.0
> + rfc3339-validator==0.1.4
> + rich==13.7.1
> + rich-argparse==1.4.0
> + rpds-py==0.18.0
> + s3fs==2024.3.1
> + s3transfer==0.10.1
> + sarif-om==1.0.4
> + scramp==1.4.4
> + setproctitle==1.3.3
> + six==1.16.0
> + sniffio==1.3.1
> + soupsieve==2.5
> + sqlalchemy==1.4.52
> + sqlalchemy-jsonfield==1.0.2
> + sqlalchemy-redshift==0.8.14
> + sqlparse==0.4.4
> + sympy==1.12
> + tabulate==0.9.0
> + tenacity==8.2.3
> + termcolor==2.4.0
> + text-unidecode==1.3
> + time-machine==2.14.1
> + typing-extensions==4.10.0
> + tzdata==2024.1
> + uc-micro-py==1.0.3
> + unicodecsv==0.14.1
> + universal-pathlib==0.2.2
> + urllib3==2.0.7
> + watchtower==3.1.0
> + werkzeug==2.2.3
> + wrapt==1.16.0
> + wtforms==3.1.2
> + xmltodict==0.13.0
> + yarl==1.9.4
> + zipp==3.18.1

</details>

3) Finally installing from URL: `uv pip install "apache-airflow[aws] @ https://github.com/apache/airflow/archive/main.tar.gz"`

<details>
  <summary>Output</summary>

> Resolved 1 package in 589ms
>   Built apache-airflow @ https://github.com/apache/airflow/archive/main.tar.gz                                                                                                                                                                                                                     > Downloaded 1 package in 1.06s
> Installed 1 package in 9ms
> - apache-airflow==2.9.0.dev0 (from file:///Users/jarek/code/airflow)
>  + apache-airflow==2.9.0.dev0 (from https://github.com/apache/airflow/archive/main.tar.gz)

</details>



---

_Referenced in [astral-sh/uv#2633](../../astral-sh/uv/pulls/2633.md) on 2024-03-24 23:34_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-03-24 23:55_

---

_Comment by @charliermarsh on 2024-03-24 23:56_

There's something happening here but I don't quite understand what yet.

---

_Comment by @charliermarsh on 2024-03-25 00:00_

(We do run the PEP 517 build hooks in all cases here.)

---

_Comment by @potiuk on 2024-03-25 00:05_

Just to add maybe a hypothesis/lead:

It could be that this is somehow interacting with the way how the "hatch_build.py" interacts with hatchling backeng build. I had a bit hard time to guess how to properly update both dependencies and optional-dependencies in the hook. And I settled with this way (which works as I described above + it nicely generates the .whl packages):

So what i settled with:

* Updating "dependencies" is as easy as setting "dependencies" tnery in the "build_data" dict passed to the initialize  https://github.com/apache/airflow/blob/20cb9f1770c819f31e48748fc9fb86527f739d6e/hatch_build.py#L748 

* modifying protected `_optional_dependencies"  dictionary  in the hook, as I could not find a better way (documentation is a bit sparse and looking at the source code did not help me to understand how else I can do it).
https://github.com/apache/airflow/blob/20cb9f1770c819f31e48748fc9fb86527f739d6e/hatch_build.py#L754

While the "optional-dependencies" might be somewhat hacky, the "dependencies" seem to be perfectly justified way. And they also nicely work for "editable" case.

Hypothesis - maybe the version is not set to "standard" when you run the hook ?  That could cause the behaviour observd.



---

_Comment by @charliermarsh on 2024-03-25 00:08_

So, this only installs airflow:

```
rm -rf foo && cargo run venv && cargo run pip install "apache-airflow[aws] @ https://github.com/apache/airflow/archive/main.tar.gz" --pre --cache-dir foo
```

But if I run again without clearing the cache...

```
cargo run venv && cargo run pip install "apache-airflow[aws] @ https://github.com/apache/airflow/archive/main.tar.gz" --cache-dir foo --pre
```

Then I get all 150 dependencies.

---

_Comment by @potiuk on 2024-03-25 00:11_

Yes. the build hook is run properly. Just run it with a little exception just before existing `inistialization` to debug 

in `hatch_build.py`:

```python
        # with hatchling, we can modify dependencies dynamically by modifying the build_data
        build_data["dependencies"] = self._dependencies
        raise Exception(build_data) <-- added this
```


and it seems all good - at least for the  clean run of:

```
uv pip install "apache-airflow[aws] @ file:///Users/jarek/code/airflow"
```

result:

```
[jarek:~/code/airflow] [test-3.11] main(+1/-0)+ 2 ± uv pip install "apache-airflow[aws] @ file:///Users/jarek/code/airflow"
Resolved 1 package in 0.48ms
error: Failed to download distributions
  Caused by: Failed to fetch wheel: apache-airflow @ file:///Users/jarek/code/airflow
  Caused by: Failed to build: apache-airflow @ file:///Users/jarek/code/airflow
  Caused by: Build backend failed to build wheel through `build_wheel()` with exit status: 1
--- stdout:

--- stderr:
Traceback (most recent call last):
  File "<string>", line 11, in <module>
  File "/Users/jarek/Library/Caches/uv/.tmpaYi2WU/.venv/lib/python3.11/site-packages/hatchling/build.py", line 58, in build_wheel
    return os.path.basename(next(builder.build(directory=wheel_directory, versions=['standard'])))
                            ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/Users/jarek/Library/Caches/uv/.tmpaYi2WU/.venv/lib/python3.11/site-packages/hatchling/builders/plugin/interface.py", line 147, in build
    build_hook.initialize(version, build_data)
  File "/Users/jarek/IdeaProjects/airflow/hatch_build.py", line 750, in initialize
    raise Exception(build_data)
Exception: {'infer_tag': False, 'pure_python': True, 'dependencies': ['alembic>=1.13.1, <2.0', 'argcomplete>=1.10', 'asgiref', 'attrs>=22.1.0', 'blinker>=1.6.2', 'colorlog>=4.0.2, <5.0', 'configupdater>=3.1.1', 'connexion[flask]>=2.10.0,<3.0', 'cron-descriptor>=1.2.24', 'croniter>=2.0.2', 'cryptography>=39.0.0', 'deprecated>=1.2.13', 'dill>=0.2.2', 'flask-caching>=1.5.0', 'flask-session>=0.4.0,<0.6', 'flask-wtf>=0.15', 'flask>=2.2,<2.3', 'fsspec>=2023.10.0', 'google-re2>=1.0', 'gunicorn>=20.1.0', 'httpx', 'importlib_metadata>=1.7;python_version<"3.9"', 'importlib_resources>=5.2,!=6.2.0,!=6.3.0,!=6.3.1;python_version<"3.9"', 'itsdangerous>=2.0', 'jinja2>=3.0.0', 'jsonschema>=4.18.0', 'lazy-object-proxy', 'linkify-it-py>=2.0.0', 'lockfile>=0.12.2', 'markdown-it-py>=2.1.0', 'markupsafe>=1.1.1', 'marshmallow-oneofschema>=2.0.1', 'mdit-py-plugins>=0.3.0', 'opentelemetry-api>=1.15.0', 'opentelemetry-exporter-otlp', 'packaging>=14.0', 'pathspec>=0.9.0', 'pendulum>=2.1.2,<4.0', 'pluggy>=1.0', 'psutil>=4.2.0', 'pygments>=2.0.1', 'pyjwt>=2.0.0', 'python-daemon>=3.0.0', 'python-dateutil>=2.3', 'python-nvd3>=0.15.0', 'python-slugify>=5.0', 'requests>=2.27.0,<3', 'rfc3339-validator>=0.1.4', 'rich-argparse>=1.0.0', 'rich>=12.4.4', 'setproctitle>=1.1.8', 'sqlalchemy>=1.4.36,<2.0', 'sqlalchemy-jsonfield>=1.0', 'tabulate>=0.7.5', 'tenacity>=6.2.0,!=8.2.0', 'termcolor>=1.1.0', 'unicodecsv>=0.14.1', 'universal-pathlib>=0.2.2', 'werkzeug>=2.0,<3', 'apache-airflow-providers-common-io', 'apache-airflow-providers-common-sql', 'apache-airflow-providers-fab>=1.0.2dev0', 'apache-airflow-providers-ftp', 'apache-airflow-providers-http', 'apache-airflow-providers-imap', 'apache-airflow-providers-smtp', 'apache-airflow-providers-sqlite'], 'force_include_editable': {}, 'extra_metadata': {}, 'artifacts': [], 'force_include': {}, 'build_hooks': ('custom',)}

```

Still, when I remove the exception:

```
Resolved 1 package in 0.45ms
   Built apache-airflow @ file:///Users/jarek/code/airflow                                                                                                                                                                                                                                          Downloaded 1 package in 5.84s
Installed 1 package in 10ms
 + apache-airflow==2.9.0.dev0 (from file:///Users/jarek/code/airflow)
```


---

_Comment by @charliermarsh on 2024-03-25 00:13_

Hmm, it seems related to our use of `prepare_metadata_for_build_wheel`. The metadata returned by that hook isn't including any requirements.

---

_Comment by @potiuk on 2024-03-25 00:16_

Maybe I should set it differently in `hatchling`, but:

a) no idea how and at least "dependencies" seem to be the right way of doing it
b) `pip` works correctly with both `dependencies` and `optional-dependencies`
c) it seems to work fine with `--editable` build even in `uv` 

So it looks like something on the `uv` side :( ?

---

_Comment by @potiuk on 2024-03-25 00:19_

Editable with exception:

```
uv pip install -e "/Users/jarek/code/airflow[aws]"
error: Failed to build editables
  Caused by: Failed to build editable: file:///Users/jarek/code/airflow
  Caused by: Build backend failed to build wheel through `build_editable()` with exit status: 1
--- stdout:

--- stderr:
Traceback (most recent call last):
  File "<string>", line 11, in <module>
  File "/Users/jarek/Library/Caches/uv/.tmpFU2vBA/.venv/lib/python3.11/site-packages/hatchling/build.py", line 83, in build_editable
    return os.path.basename(next(builder.build(directory=wheel_directory, versions=['editable'])))
                            ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/Users/jarek/Library/Caches/uv/.tmpFU2vBA/.venv/lib/python3.11/site-packages/hatchling/builders/plugin/interface.py", line 147, in build
    build_hook.initialize(version, build_data)
  File "/Users/jarek/IdeaProjects/airflow/hatch_build.py", line 750, in initialize
    raise Exception(build_data)
Exception: {'infer_tag': False, 'pure_python': True, 'dependencies': ['alembic>=1.13.1, <2.0', 'argcomplete>=1.10', 'asgiref', 'attrs>=22.1.0', 'blinker>=1.6.2', 'colorlog>=4.0.2, <5.0', 'configupdater>=3.1.1', 'connexion[flask]>=2.10.0,<3.0', 'cron-descriptor>=1.2.24', 'croniter>=2.0.2', 'cryptography>=39.0.0', 'deprecated>=1.2.13', 'dill>=0.2.2', 'flask-caching>=1.5.0', 'flask-session>=0.4.0,<0.6', 'flask-wtf>=0.15', 'flask>=2.2,<2.3', 'fsspec>=2023.10.0', 'google-re2>=1.0', 'gunicorn>=20.1.0', 'httpx', 'importlib_metadata>=1.7;python_version<"3.9"', 'importlib_resources>=5.2,!=6.2.0,!=6.3.0,!=6.3.1;python_version<"3.9"', 'itsdangerous>=2.0', 'jinja2>=3.0.0', 'jsonschema>=4.18.0', 'lazy-object-proxy', 'linkify-it-py>=2.0.0', 'lockfile>=0.12.2', 'markdown-it-py>=2.1.0', 'markupsafe>=1.1.1', 'marshmallow-oneofschema>=2.0.1', 'mdit-py-plugins>=0.3.0', 'opentelemetry-api>=1.15.0', 'opentelemetry-exporter-otlp', 'packaging>=14.0', 'pathspec>=0.9.0', 'pendulum>=2.1.2,<4.0', 'pluggy>=1.0', 'psutil>=4.2.0', 'pygments>=2.0.1', 'pyjwt>=2.0.0', 'python-daemon>=3.0.0', 'python-dateutil>=2.3', 'python-nvd3>=0.15.0', 'python-slugify>=5.0', 'requests>=2.27.0,<3', 'rfc3339-validator>=0.1.4', 'rich-argparse>=1.0.0', 'rich>=12.4.4', 'setproctitle>=1.1.8', 'sqlalchemy>=1.4.36,<2.0', 'sqlalchemy-jsonfield>=1.0', 'tabulate>=0.7.5', 'tenacity>=6.2.0,!=8.2.0', 'termcolor>=1.1.0', 'unicodecsv>=0.14.1', 'universal-pathlib>=0.2.2', 'werkzeug>=2.0,<3'], 'force_include_editable': {}, 'extra_metadata': {}, 'artifacts': [], 'force_include': {}, 'build_hooks': ('custom',)}
```

After removing the exception:

```
[jarek:~/code/airflow] [test-3.11] main(+1/-1)+ 2 ± uv pip install -e "/Users/jarek/code/airflow[aws]"
   Built file:///Users/jarek/code/airflow                                                                                                                                                                                                                                                           Built 1 editable in 4.63s
Resolved 161 packages in 5.82s
   Built python-nvd3==0.15.0
   Built lazy-object-proxy==1.10.0
   Built unicodecsv==0.14.1                                                                                                                                                                                                                                                                         Downloaded 151 packages in 1.82s
Installed 154 packages in 254ms
 + aiobotocore==2.12.1
 + aiohttp==3.9.3
 + aioitertools==0.11.0
 + aiosignal==1.3.1
 + alembic==1.13.1
 + annotated-types==0.6.0
 + anyio==4.3.0
 + apache-airflow==2.9.0.dev0 (from file:///Users/jarek/code/airflow)
 + argcomplete==3.2.3
 + asgiref==3.8.1
 + asn1crypto==1.5.1
 + attrs==23.2.0
 + aws-sam-translator==1.86.0
.....  and son ....
```


---

_Comment by @charliermarsh on 2024-03-25 00:20_

Perhaps we can ask Ofek if he has any ideas why the generated metadata is different here.

---

_Comment by @potiuk on 2024-03-25 00:21_

cc: @ofek ? 

---

_Renamed from "The "standard" installation use `pyproject.toml` in UV rather than dynamic deypendencies via build hooks (comparing to PIP)" to "The "standard" installation use `pyproject.toml` in UV rather than dynamic dependencies via build hooks (comparing to PIP)" by @potiuk on 2024-03-25 00:22_

---

_Comment by @charliermarsh on 2024-03-25 00:34_

(I _think_ the reason it works the second invocation that the wheel is built, and we read the metadata from the wheel if it's available. But in the first invocation, during resolution, we do `prepare_metadata_for_build_wheel`, since the wheel isn't available yet.)

---

_Comment by @potiuk on 2024-03-25 00:41_

> (I think the reason it works the second invocation that the wheel is built, and we read the metadata from the wheel if it's available. But in the first invocation, during resolution, we do prepare_metadata_for_build_wheel, since the wheel isn't available yet.)

I believe this is what `pip` is always doing - preparing the  wheel first and only then installing it.

---

_Comment by @potiuk on 2024-03-25 00:45_

And that might also explain why editable works - because you are doing it in PEP-660 compatible way (which also builds an "editable" wheel as an intermediary medium of metadaa)  ?

---

_Comment by @charliermarsh on 2024-03-25 00:49_

I'm pretty sure that the metadata returned by `prepare_metadata_for_build_wheel` should be the same as that returned by building the wheel though.

---

_Comment by @potiuk on 2024-03-25 01:02_

Hmmm. Then we need @ofek to chime-in :). 

BTW. Just to add the sense of `priority` - it's not **super** important to handle quickly. 

I workarounded it in our CI image building proces. so I donwload and unpack the `https://github.com/apache/airflow/archive/main.tar.gz` for our own caching, unpack it and instal it in `--editable` mode, which is actually even better for our process, because this way we could remove the `devel-ci` extra from wheel (so it is likely going to stay like that). but it would be nice to solve it for "prime-time" of `uv` for our users who might want to also install airflow via the GitHub URL in some cases.

It works for `pip` and `pip` is for now our recommended user-installation build frontent, so this does not impact us much

---

_Comment by @ofek on 2024-03-25 01:19_

Does it work if you explicitly gate by the value of this e.g. `wheel`? https://hatch.pypa.io/latest/plugins/build-hook/reference/#hatchling.builders.hooks.plugin.interface.BuildHookInterface.target_name

Both built-in targets `wheel` and `sdist` have a version named `standard` just like your custom build target:

- https://hatch.pypa.io/latest/plugins/builder/wheel/#versions
- https://hatch.pypa.io/latest/plugins/builder/sdist/#versions

I'm just getting up to speed on the issue and don't immediately see what's happening.

---

_Comment by @ofek on 2024-03-25 01:32_

Oh, nevermind I see the issue I think: https://github.com/pypa/hatch/blob/hatchling-v1.22.4/backend/src/hatchling/build.py#L86-L99

Charlie, let me know how you would like me to fix. I think I have to account for your code base now as well.

---

_Comment by @potiuk on 2024-03-25 01:32_

> I'm just getting up to speed on the issue and don't immediately see what's happening.

In short:

* airflow has rather complex build_hooks writen via hatchling's `hatch_build.py` https://github.com/apache/airflow/blob/main/hatch_build.py

* I just merged PR yesterday (https://github.com/apache/airflow/pull/38437)  to fix it for a bit of abuse of `PEP 517` (partially based on your discussions in https://github.com/pypa/hatch/issues/1331 and comment from pfmoore about  non-standard compliant mixing of dynamic properties  with static data in `pyproject.toml`. I turned both `dependencies` and `optional-dependencies` into a truly dynamic property in this PR and in the hook I am updating them after calculating them (though I must admit `optional-dependencies` are updated in rather hacky way as I have not found a better way via the hatchling hook (see those few lines in https://github.com/apache/airflow/blob/main/hatch_build.py#L748) 

* **the problem** - it seems all work perfectly fine in `pip` for both installing an "editable" install and also a `standard` install from airflow sources (either via "file:///" or  "http:" - with all the bells-and-whistles (i.e. extras are resolving as I wanted them to resolve). But the "standard" builds don't work with `uv`.


---

_Comment by @potiuk on 2024-03-25 01:34_

> Oh, nevermind I see the issue I think: https://github.com/pypa/hatch/blob/hatchling-v1.22.4/backend/src/hatchling/build.py#L86-L99

> Charlie, let me know how you would like me to fix. I think I have to account for your code base now as well.

AAARGH...

 If `pip` then.... 🤯 


---

_Comment by @potiuk on 2024-03-25 01:36_

BTW. @ofek -> since I got your attention... Is there a better way to dynamically set `optional-dependencies` than what I (hackishly) figured out ?

```python
        # unfortunately hatchling currently does not have a way to override optional_dependencies
        # via build_data (or so it seem) so we need to modify internal _optional_dependencies
        # field in core.metadata until this is possible
        self.metadata.core._optional_dependencies = self.optional_dependencies
```

---

_Comment by @potiuk on 2024-03-25 01:38_

>  I think I have to account for your code base now as well.

BTW. Shouldn't it work for any PEP-compliant frontend (just sayin :D ) ?

---

_Comment by @ofek on 2024-03-25 01:39_

I know that seems hacky but I swear that was only the way to satisfy everyone 😅 I honestly thought a lot about that fix.

> Is there a better way to dynamically set optional-dependencies than what I (hackishly) figured out ?

Yes there is a metadata hook https://hatch.pypa.io/latest/plugins/metadata-hook/custom/

---

_Comment by @charliermarsh on 2024-03-25 01:44_

Ah ok, so am I correct that Hatchling detects if pip is calling it, and disables the `prepare_metadata_for_build_wheel` hooks in that case?

---

_Comment by @ofek on 2024-03-25 01:50_

Yes, for precisely this issue!

---

_Comment by @potiuk on 2024-03-25 01:56_

> Yes there is a metadata hook https://hatch.pypa.io/latest/plugins/metadata-hook/custom/

Thank you 🙇‍♂️. I think after all the things I've done for Airlfow It's time to contribute back some docs to Hatchling from the "user" perspective :). Will likely do so soon. 

---

_Comment by @ofek on 2024-03-25 02:01_

That would be awesome, thanks 🙂 Here is the meta-issue: https://github.com/pypa/hatch/issues/1245

In short, please prioritize how-to documents!

---

_Comment by @potiuk on 2024-03-25 02:05_

> Yes there is a metadata hook https://hatch.pypa.io/latest/plugins/metadata-hook/custom/

Hmm. Just tried it but it does not seem to allow me to distinguish between editable/standard build. I want to **really** have quite a different set of those for editable standard build in a single `hatch_build.py` 

---

_Comment by @ofek on 2024-03-25 02:07_

Then please open a feature request to the build hook interface.

---

_Comment by @potiuk on 2024-03-25 02:08_

> Then please open a feature request to the build hook interface.

Cool. 

---

_Comment by @charliermarsh on 2024-03-25 02:12_

@ofek -- I assume there's no way for uv or for Hatch to detect when the hook can be trusted vs. not? E.g., could uv just avoid calling this when requirements are declared as "dynamic"?

---

_Comment by @ofek on 2024-03-25 02:16_

That could work also I think.

---

_Comment by @charliermarsh on 2024-03-25 02:32_

Is this unique to Hatch since it supports extensions? Would other build backends need similar gating?

---

_Comment by @ofek on 2024-03-25 02:43_

setuptools would have the same issue in theory for very dynamic builds.

edit: really any such backend that allows for code execution during builds to modify metadata, maybe PDM also

---

_Comment by @charliermarsh on 2024-03-25 02:45_

Ok, I'll add that gate in uv for now.

---

_Comment by @charliermarsh on 2024-03-25 03:31_

Do you have any advice for writing a test for this? This is what I have, but it's passing when I want it to fail based on the above: https://github.com/astral-sh/uv/compare/charlie/proj...charlie/dynamic

---

_Comment by @ofek on 2024-03-25 03:47_

Does this work?

`scripts/packages/hatch_dynamic/pyproject.toml`:

```toml
[build-system]
requires = ["hatchling"]
build-backend = "hatchling.build"

[project]
name = "hatch-dynamic"
version = "1.0.0"
dynamic = ["dependencies"]

[tool.hatch.build.targets.wheel.hooks.custom]
```

`scripts/packages/hatch_dynamic/hatch_build.py`:

```python
from hatchling.builders.hooks.plugin.interface import BuildHookInterface


class LiteraryBuildHook(BuildHookInterface):
    def initialize(self, version, build_data):
        build_data['dependencies'].append('foo')
```

---

_Comment by @charliermarsh on 2024-03-25 03:48_

Trying, thank you.

---

_Comment by @charliermarsh on 2024-03-25 03:56_

Yes, thanks.

---

_Referenced in [astral-sh/uv#2645](../../astral-sh/uv/pulls/2645.md) on 2024-03-25 03:57_

---

_Comment by @charliermarsh on 2024-03-25 04:02_

It looks like the example use-case from the Hatch issue (https://github.com/pypa/hatch/issues/532) doesn't use dynamic dependencies. I was hoping I could instead do something even more specific, like look for the presence of Hatch plugins (`[tool.hatch.build.targets.wheel.hooks]`, etc.), but that example _also_ doesn't seem to use that, it registers a custom entrypoint.

---

_Comment by @charliermarsh on 2024-03-25 04:02_

So I'm not really sure how to detect it :/

---

_Comment by @ofek on 2024-03-25 04:13_

Hmm, good point actually only metadata hooks have anything to do with `dynamic` I'm seeing. At the end of the day, build hooks can do anything to the metadata because even if they don't use the standard fields that the system might provide to change things they could also do a post hook to actually rewrite the contents of the wheel e.g. for example if there was a plugin that ran auditwheel/delocate/delvewheel to fix up extension modules.

If you want to check hooks then you would have to search for the following:

- `[tool.hatch.build.hooks.*]`
- `[tool.hatch.build.targets.*.hooks.*]`

Alternatively (or in addition to), maybe it would be nice to indicate your usage with an environment variable and then pip perhaps could start doing the same thing to standardize.

---

_Comment by @sbidoul on 2024-03-25 08:51_

I'd find it worrying if a trend develops for frontends to special case for specific backends, or backends to special case for specific frontends.

---

_Comment by @potiuk on 2024-03-25 08:57_

> I'd find it worrying if a trends develops for frontends to special case for specific backends, or backends to special case for specific frontends.

Agree. I think we have to find a solution that is generic in both directions. Maybe something to discuss in Pittsburgh :) 

---

_Referenced in [pypa/hatch#1348](../../pypa/hatch/issues/1348.md) on 2024-03-25 09:06_

---

_Comment by @ofek on 2024-03-25 13:06_

Just a sanity check, does everyone here understand why I do this? https://github.com/pypa/hatch/blob/hatchling-v1.22.4/backend/src/hatchling/build.py#L86-L99

I'm open to feedback but basically for extremely dynamic metadata you don't want frontends caching like that before dynamic stuff happens i.e. this issue.

---

_Comment by @charliermarsh on 2024-03-25 14:17_

I think I understand it, though I'm not familiar enough with Hatch to know why my first test case (using the `requirements.txt` plugin) worked without issue.

I do think it's non-standards compliant though, in two ways, at least based on what I saw in the linked issue:

1. Projects that don't mark fields as "dynamic" can still produce dynamic metadata. In theory this could be considered a user configuration error?
2. The metadata returned by `prepare_metadata_for_build_wheel` is incorrect and Hatch doesn't adhere to the contract in the standards, so it's then sniffing for callers that would be impacted by the incorrectness.

I'm honestly somewhat hesitant to add custom workarounds for Hatch here, though it also seems like a huge shame to skip `prepare_metadata_for_build_wheel` in all cases when it would be totally valid most of the time.

I know it's easier said than done, but I feel like the standards-compliant thing would be something like:

- Require "dynamic" for any field that is actually dynamic, and have Hatch enforce that after all the hooks run. (No idea if this is possible.)
- Do the detection _within Hatch_ to figure out whether `prepare_metadata_for_build_wheel` is safe (e.g., are there any build hooks?), and return `None` when it's not.


---

_Comment by @ofek on 2024-03-25 15:59_

> I think I understand it, though I'm not familiar enough with Hatch to know why my first test case (using the requirements.txt plugin) worked without issue.

Metadata resolution is the very first step and that component doesn't take into account what kind of builds are happening so it works perfectly in that case.

> Projects that don't mark fields as "dynamic" can still produce dynamic metadata. In theory this could be considered a user configuration error?

That is unrelated to what's happening here but you're correct that I should produce an error for build hooks modifying static metadata just like I do for metadata hooks. I'll work on that soon.

> The metadata returned by prepare_metadata_for_build_wheel is incorrect and Hatch doesn't adhere to the contract in the standards, so it's then sniffing for callers that would be impacted by the incorrectness.

I understand why you might think that but the issue is that backends that offer a dynamic functionality must make a trade-off. There are three options:

1. Don't implement that method at all which would preclude certain performance optimizations for some tools.
2. Only provide an implementation conditionally, deliberately excluding situations where it is undesirable. (This is the current approach.)
3. Provide a constant implementation, which would break for workloads that require dynamic things happening during builds.

> Do the detection within Hatch to figure out whether prepare_metadata_for_build_wheel is safe (e.g., are there any build hooks?), and return None when it's not.

Is that part of the standard? It appears to me that you either implement it or you don't: https://peps.python.org/pep-0517/#prepare-metadata-for-build-wheel

---

_Comment by @charliermarsh on 2024-03-25 17:04_

> That is unrelated to what's happening here but you're correct that I should produce an error for build hooks modifying static metadata just like I do for metadata hooks. I'll work on that soon.

Yeah makes sense. This more spun out of my idea to detect `dynamic` fields and limit the call to non-`dynamic` cases.

> Is that part of the standard? It appears to me that you either implement it or you don't: https://peps.python.org/pep-0517/#prepare-metadata-for-build-wheel

I think you're right. Our implementation allows the backend to return `None` but I think that's just an implementation detail on our side, I don't see that in the standard.

> Only provide an implementation conditionally, deliberately excluding situations where it is undesirable. (This is the current approach.)

This seems like a reasonable outcome. The problem is that the "conditionally" is based on detecting the build front-end. Like, this wouldn't just be a problem for `uv`, it'd be a problem for any other installer too. Though I understand why it's hard to do anything differently here given the limitations of the spec.

> I understand why you might think that but the issue is that backends that offer a dynamic functionality must make a trade-off.

Yeah, I understand. But it's still out of compliance with the standard, right? I'm not sure what to suggest other than that the standard needs to evolve in some way, but I'm not familiar enough with build backends to understand how.


---

_Comment by @ofek on 2024-03-25 20:34_

I suppose I'll use your implementation's hack and return `None`.

---

_Comment by @charliermarsh on 2024-03-25 20:40_

It's possible that breaks other frontends though, I'm not sure >.<

---

_Comment by @charliermarsh on 2024-03-25 20:53_

I have https://github.com/astral-sh/uv/pull/2645 open for now.

---

_Comment by @charliermarsh on 2024-03-25 21:00_

> I suppose I'll use your implementation's hack and return None.

My read is that this won't work for `pip`:

```python
def prepare_metadata_for_build_wheel(
        metadata_directory, config_settings, _allow_fallback):
    """Invoke optional prepare_metadata_for_build_wheel

    Implements a fallback by building a wheel if the hook isn't defined,
    unless _allow_fallback is False in which case HookMissing is raised.
    """
    backend = _build_backend()
    try:
        hook = backend.prepare_metadata_for_build_wheel
    except AttributeError:
        if not _allow_fallback:
            raise HookMissing()
    else:
        return hook(metadata_directory, config_settings)
    # fallback to build_wheel outside the try block to avoid exception chaining
    # which can be confusing to users and is not relevant
    whl_basename = backend.build_wheel(metadata_directory, config_settings)
    return _get_wheel_metadata_from_wheel(whl_basename, metadata_directory,
                                          config_settings)
```

(Though I know you omit pip anyway altogether.)

---

_Comment by @charliermarsh on 2024-03-25 21:01_

I would say just leave it for now, there's no need to make any urgent changes.

---

_Comment by @charliermarsh on 2024-03-25 22:24_

I ended up special-casing `hatchling` in https://github.com/astral-sh/uv/pull/2645. We should figure out a better solution together.

---

_Closed by @charliermarsh on 2024-03-25 22:26_

---

_Referenced in [astral-sh/uv#2703](../../astral-sh/uv/issues/2703.md) on 2024-03-28 09:22_

---

_Referenced in [prefix-dev/pixi#1121](../../prefix-dev/pixi/pulls/1121.md) on 2024-04-05 13:28_

---
