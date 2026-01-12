```yaml
number: 3115
title: isort configuration doesnt seem to respect localfolder (vs real isort) in monorepo
type: issue
state: open
author: merc1031
labels:
  - isort
assignees: []
created_at: 2023-02-22T06:09:08Z
updated_at: 2023-07-15T05:07:11Z
url: https://github.com/astral-sh/ruff/issues/3115
synced_at: 2026-01-12T15:54:43Z
```

# isort configuration doesnt seem to respect localfolder (vs real isort) in monorepo

---

_@merc1031_

ruff 0.0.249

run `ruff check pure_check/checks/docker_active_container.py`

The format that ruff isort wants doesn't agree with regular isort (which is seemingly more correct). maybe do to the default_section config on regular isort

snippet in question (works with isort config)
```
import rich_click as click

from lib.enums.general import CheckResult
from lib.pure_check import Target

from pure_check.checks.base_check import BaseCheck
from pure_check.rehabs.remove_active_docker import RemoveActiveDocker
from pure_check.rehabs.restart_node import RestartNode
```

what ruff wants me to change it to
```
import rich_click as click

from lib.enums.general import CheckResult
from lib.pure_check import Target
from pure_check.checks.base_check import BaseCheck
from pure_check.rehabs.remove_active_docker import RemoveActiveDocker
from pure_check.rehabs.restart_node import RestartNode
```

directory structure
```
lib
| __init__.py
├── pure_check.py
├── enums
│   ├── __init__.py
│   ├── general.py
pure_check
├── pure_check
│   ├── __init__.py
│   ├── checks
│   │   ├── __init__.py
│   │   ├── base_check.py
│   └── rehabs
│      ├── __init__.py
│       ├── remove_active_docker.py
│       └── restart_node.py
```

pyproject.toml
```
[tool.ruff.isort]
force-sort-within-sections = true
force-to-top = []
forced-separate = []
known-first-party = ["lib", "tests", "app"]
lines-after-imports = 2
no-lines-before = []
```

.isort.cfg  (known_third_party is seeded by seed-isort-cfg)
```
[settings]
default_section=LOCALFOLDER
force_grid_wrap=2
force_sort_within_sections=1
force_to_top=
forced_separate=
include_trailing_comma=1
indent='    '
known_first_party=lib,tests,app
known_future_library=
known_standard_library=
# This is generated from isort-seed-config DO NOT MODIFY BY HAND
known_third_party=Levenshtein,PyInstaller,alembic,anubis,attr,autotriage_spark_monitor,backpack,billiard,blinker,boto3,bs4,cachetools,cassandra,celery,cleo,click,click_default_group,consul,dateutil,dirty_equals,distro,distutils,dns,dotenv,elasticsearch,faker,flake8,flask,flask_restful,flexmock,frozendict,git,gspread,haproxy_check,inflection,influxdb,iso8601,jinja2,jira,jira_cookie_auth,jwt,kafka,kif,kombu,lazy_object_proxy,ldap,lru,marshmallow,matplotlib,methodtools,metrics_helper,mock,more_itertools,multimethod,nltk,nomad,numpy,octillion_lib,pampy,pandas,pendulum,pika,postgresql,prometheus_api_client,prometheus_client,psycopg2,pygments,pympler,pysnmp,pyspark,pytest,pytest_describe,pytoml,pytz,re2,redis,redis_lock,requests,rethinkdb,rich_click,seaborn,setuptools,sh,simplejson,six,slack_bolt,slack_sdk,slackclient,snowflake,sqlalchemy,statsd,structlog,sumtypes,tenacity,texttable,typing_extensions,unidecode,urllib3,webargs,werkzeug,wrapt,xmltodict,yaml
length_sort=0
line_length=120
lines_after_imports=2
multi_line_output=3
no_lines_before=
sections=FUTURE,STDLIB,THIRDPARTY,FIRSTPARTY,LOCALFOLDER
skip=
skip_glob=citadel/node_modules/*.*
use_parentheses=1
src_paths=*/tests,*
old_finders=true
```

Thanks for any help


---

_Comment by @charliermarsh on 2023-02-22 14:26_

Yeah we don't support `default_section` right now. I suppose you could mark `pure_check` as `known-local-folder` as a workaround?

---

_Label `isort` added by @charliermarsh on 2023-02-22 14:26_

---

_Comment by @merc1031 on 2023-02-22 19:25_

Ah ok.
This is a legacy mono repo with many many top level folders, so that would be... difficult to maintain (if someone adds a new one) without some automation like seed-isort to auto rewrite the config.
I guess i can continue running isort on the side until this works.

Thanks for the response

---

_Comment by @franneck94 on 2023-04-23 10:22_

Is it planned in the near future?

---

_Comment by @charliermarsh on 2023-04-23 15:40_

@franneck94 - just to make sure I understand, you’re asking specifically about adding the default-section configuration option, is that right?

---

_Comment by @franneck94 on 2023-04-23 15:49_

Yes correct.
I'm a tutor and we have a lot of small Python examples in a nested directory structure, where I would like to just have all "local" imports in that default section.
Currently this is a deal breaker for me to use ruff's isort.

![image](https://user-images.githubusercontent.com/20141069/233850761-1354812e-4a75-40fd-82e8-d1cd95111ed8.png)


---

_Comment by @merc1031 on 2023-05-29 22:39_

I would definitely be interested in the `default-section` configuration so i could default to `local-folder`.

Currently I am just using regular isort for now, i could also write  pre process step that auto populates `known-local-folder`, but the `default-section` config is just much more convenient

---

_Comment by @merc1031 on 2023-07-15 05:07_

@charliermarsh 
Hey just checking if this is any closer to being possible. After the newish Custom Sections feature was merged in i think this is the only thing holding back removing isort from our pre-commit pipeline.

---
