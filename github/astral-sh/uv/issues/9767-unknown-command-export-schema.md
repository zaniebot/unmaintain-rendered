---
number: 9767
title: "Unknown command: 'export_schema'"
type: issue
state: closed
author: MrMegaMango
labels:
  - needs-mre
assignees: []
created_at: 2024-12-10T11:26:38Z
updated_at: 2025-01-17T10:23:23Z
url: https://github.com/astral-sh/uv/issues/9767
synced_at: 2026-01-07T13:12:18-06:00
---

# Unknown command: 'export_schema'

---

_Issue opened by @MrMegaMango on 2024-12-10 11:26_

$ uv run python manage.py export_schema graphql_api.schema:schema --path ../../packages/graphql/src/schema.graphql
warning: No `requires-python` value found in the workspace. Defaulting to `>=3.11`.
{"message": "starting Prometheus metrics server on port 9090", "search_id": "", "context_id": "", "report_generator_id": "", "timestamp": "2024-12-10T11:22:17.751629Z", "severity": "INFO", "labels": {"searchId": "", "contextId": ""}}
Unknown command: 'export_schema'
Type 'manage.py help' for usage.
error: script "generate" exited with code 1
error: script "generate:all" exited with code 1
make[1]: *** [graphql] Error 1
make: *** [codegen] Error 2


I think this is a django/strawberry thing, running without uv directly by python is ok

---

_Comment by @charliermarsh on 2024-12-10 13:34_

In order to help, I'd like need a more complete reproduction. For example, a minimal GitHub repo to emulate the problem, with a set of commands I can run.

---

_Label `needs-mre` added by @charliermarsh on 2024-12-10 13:34_

---

_Closed by @MrMegaMango on 2024-12-10 13:35_

---

_Comment by @MrMegaMango on 2025-01-15 14:05_

The origianal question should be rephrased as:
how do I pass arguments correctly when using `uv run` to replace `python`?

> uv run manage.py export_schema graphql_api.schema:schema --path ../../packages/graphql/src/schema.graphql
Unknown command: 'export_schema'

vs
> python manage.py export_schema graphql_api.schema:schema --path ../../packages/graphql/src/schema.graphql
works

![Image](https://github.com/user-attachments/assets/048766f3-0d6d-4dfc-8870-880692c07808)
https://strawberry.rocks/docs/django/guide/export-schema

---

_Reopened by @MrMegaMango on 2025-01-15 14:05_

---

_Closed by @MrMegaMango on 2025-01-15 14:11_

---

_Comment by @MrMegaMango on 2025-01-16 23:39_

```
python manage.py help
[auth]
    changepassword
    createsuperuser

[channels]
    runworker

[contenttypes]
    remove_stale_contenttypes

[daphne]
    runserver

[django]
    check
    compilemessages
    createcachetable
    dbshell
    diffsettings
    dumpdata
    flush
    inspectdb
    loaddata
    makemessages
    makemigrations
    migrate
    optimizemigration
    sendtestemail
    shell
    showmigrations
    sqlflush
    sqlmigrate
    sqlsequencereset
    squashmigrations
    startapp
    startproject
    test
    testserver

[graphql_api]
    migrate_references
    populate_per_user_data

[sessions]
    clearsessions

[staticfiles]
    collectstatic
    findstatic

[strawberry_django]
    export_schema
```

vs

```
uv run manage.py help
Type 'manage.py help ' for help on a specific subcommand.
Available subcommands:
[django]
check
compilemessages
createcachetable
dbshell
diffsettings
dumpdata
flush
inspectdb
loaddata
makemessages
makemigrations
migrate
optimizemigration
runserver
sendtestemail
shell
showmigrations
sqlflush
sqlmigrate
sqlsequencereset
squashmigrations
startapp
startproject
test
testserver
```

---

_Reopened by @MrMegaMango on 2025-01-16 23:39_

---

_Comment by @MrMegaMango on 2025-01-16 23:43_

similar to this https://micro.webology.dev/2024/08/23/uv-run-django.html
but adding 
# /// script
# requires-python = ">=3.11"
# dependencies = [
#     "strawberry-graphql-django",
# ]
# ///
still doesn't make a difference

---

_Comment by @MrMegaMango on 2025-01-17 10:23_

it was a import error on local editable dependency hidden by django, calling django.setup() early exposes the error

---

_Closed by @MrMegaMango on 2025-01-17 10:23_

---
