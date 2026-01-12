```yaml
number: 12359
title: Retrying in case of a broken pipe
type: issue
state: closed
author: potiuk
labels:
  - bug
assignees: []
created_at: 2025-03-21T09:23:36Z
updated_at: 2025-05-21T17:23:59Z
url: https://github.com/astral-sh/uv/issues/12359
synced_at: 2026-01-12T16:01:01Z
```

# Retrying in case of a broken pipe

---

_@potiuk_

### Summary

Recently in our CI we started to experience somewhat frequent failures due to `broken pipe`  errors when pulling big artifacts from PyPI. Particularly `pyspark==3.5.5` which is 320  MB to pull: https://pypi.org/project/pyspark/#files

While this is of course a problem of the infrastructure (either PyPI or GH actions), it should be relatively easy (I guess) to introduce a retry mechanism in case of broken pipe - which is very likely transient error.


### Example

https://github.com/apache/airflow/actions/runs/13987630354/job/39165182511?pr=47798#step:11:5191

```bash
Running command: uv pip install --no-sources -e '.[all-core]' ./airflow-core ./task-sdk --reinstall apache-airflow-providers-airbyte==5.0.1 apache-airflow-providers-alibaba==3.0.1 apache-airflow-providers-amazon==9.4.0 apache-airflow-providers-apache-beam==6.0.3 apache-airflow-providers-apache-cassandra==3.7.1 apache-airflow-providers-apache-drill==3.0.1 
  apache-airflow-providers-apache-druid==4.1.0 apache-airflow-providers-apache-flink==1.6.1 apache-airflow-providers-apache-hdfs==4.7.1 apache-airflow-providers-apache-hive==9.0.3 apache-airflow-providers-apache-iceberg==1.2.1 apache-airflow-providers-apache-impala==1.6.1 apache-airflow-providers-apache-kafka==1.7.0 apache-airflow-providers-apache-kylin==3.8.1 
  apache-airflow-providers-apache-livy==4.2.1 apache-airflow-providers-apache-pig==4.6.1 apache-airflow-providers-apache-pinot==4.7.0 apache-airflow-providers-apache-spark==5.0.1 apache-airflow-providers-apprise==2.0.1 apache-airflow-providers-arangodb==2.7.3 apache-airflow-providers-asana==2.9.1 apache-airflow-providers-atlassian-jira==3.0.1 apache-airflow-providers-celery==3.10.3 
  apache-airflow-providers-cloudant==4.1.1 apache-airflow-providers-cncf-kubernetes==10.3.1 apache-airflow-providers-cohere==1.4.3 apache-airflow-providers-common-compat==1.5.1 apache-airflow-providers-common-io==1.5.1 apache-airflow-providers-common-sql==1.24.0 apache-airflow-providers-databricks==7.2.1 apache-airflow-providers-datadog==3.8.3 apache-airflow-providers-dbt-cloud==4.2.1 
  apache-airflow-providers-dingding==3.7.3 apache-airflow-providers-discord==3.9.3 apache-airflow-providers-docker==4.2.1 apache-airflow-providers-elasticsearch==6.2.1 apache-airflow-providers-exasol==4.7.3 apache-airflow-providers-facebook==3.7.1 apache-airflow-providers-ftp==3.12.3 apache-airflow-providers-github==2.8.3 apache-airflow-providers-google==14.0.0 apache-airflow-providers-grpc==3.7.3 
  apache-airflow-providers-hashicorp==4.1.0 apache-airflow-providers-http==5.2.1 apache-airflow-providers-imap==3.8.3 apache-airflow-providers-influxdb==2.8.3 apache-airflow-providers-jdbc==5.0.1 apache-airflow-providers-jenkins==4.0.3 apache-airflow-providers-microsoft-azure==12.2.1 apache-airflow-providers-microsoft-mssql==4.2.1 apache-airflow-providers-microsoft-psrp==3.0.1 
  apache-airflow-providers-microsoft-winrm==3.9.1 apache-airflow-providers-mongo==5.0.2 apache-airflow-providers-mysql==6.2.0 apache-airflow-providers-neo4j==3.8.2 apache-airflow-providers-odbc==4.9.1 apache-airflow-providers-openai==1.5.2 apache-airflow-providers-openfaas==3.7.1 apache-airflow-providers-openlineage==2.1.1 apache-airflow-providers-opensearch==1.6.2 
  apache-airflow-providers-opsgenie==5.8.2 apache-airflow-providers-oracle==4.0.2 apache-airflow-providers-pagerduty==4.0.2 apache-airflow-providers-papermill==3.9.2 apache-airflow-providers-pgvector==1.4.1 apache-airflow-providers-pinecone==2.2.2 apache-airflow-providers-postgres==6.1.1 apache-airflow-providers-presto==5.8.2 apache-airflow-providers-qdrant==1.3.2 
  apache-airflow-providers-redis==4.0.2 apache-airflow-providers-salesforce==5.10.1 apache-airflow-providers-samba==4.9.2 apache-airflow-providers-segment==3.7.2 apache-airflow-providers-sendgrid==4.0.1 apache-airflow-providers-sftp==5.1.1 apache-airflow-providers-singularity==3.7.1 apache-airflow-providers-slack==9.0.2 apache-airflow-providers-smtp==2.0.1 apache-airflow-providers-snowflake==6.1.1 
  apache-airflow-providers-sqlite==4.0.1 apache-airflow-providers-ssh==4.0.1 apache-airflow-providers-standard==0.1.1 apache-airflow-providers-tableau==5.0.2 apache-airflow-providers-telegram==4.7.2 apache-airflow-providers-teradata==3.0.2 apache-airflow-providers-trino==6.1.0 apache-airflow-providers-vertica==4.0.1 apache-airflow-providers-weaviate==3.0.2 apache-airflow-providers-yandex==4.0.2 
  apache-airflow-providers-ydb==2.1.1 apache-airflow-providers-zendesk==4.9.1 --resolution highest
  Using Python 3.12.9 environment at: /usr/local
     Building apache-airflow @ file:///opt/airflow
        Built apache-airflow @ file:///opt/airflow
    × Failed to download and build `pyspark==3.5.5`
    ├─▶ Failed to extract archive
    ├─▶ failed to unpack
    │   `/root/.cache/uv/sdists-v9/.tmpK4m8P8/pyspark-3.5.5/deps/jars/hadoop-client-runtime-3.3.4.jar`
    ├─▶ failed to unpack
    │   `pyspark-3.5.5/deps/jars/hadoop-client-runtime-3.3.4.jar` into
    │   `/root/.cache/uv/sdists-v9/.tmpK4m8P8/pyspark-3.5.5/deps/jars/hadoop-client-runtime-3.3.4.jar`
    ├─▶ error decoding response body
    ├─▶ request or response body error
    ├─▶ error reading a body from connection
    ╰─▶ stream closed because of a broken pipe
    help: `pyspark` (v3.5.5) was included because
          `apache-airflow-providers-apache-spark` (v5.0.1) depends on
          `pyspark>=3.1.3`
  Traceback (most recent call last):
  File "/opt/airflow/scripts/in_container/run_generate_constraints.py", line 531, in <module>
    generate_constraints()
  File "/usr/local/lib/python3.12/site-packages/click/core.py", line 1161, in __call__
    return self.main(*args, **kwargs)
           ^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/usr/local/lib/python3.12/site-packages/rich_click/rich_command.py", line 166, in main
    rv = self.invoke(ctx)
         ^^^^^^^^^^^^^^^^
  File "/usr/local/lib/python3.12/site-packages/click/core.py", line 1443, in invoke
    return ctx.invoke(self.callback, **ctx.params)
           ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/usr/local/lib/python3.12/site-packages/click/core.py", line 788, in invoke
    return __callback(*args, **kwargs)
           ^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/opt/airflow/scripts/in_container/run_generate_constraints.py", line 517, in generate_constraints
    generate_constraints_pypi_providers(config_params)
  File "/opt/airflow/scripts/in_container/run_generate_constraints.py", line 396, in generate_constraints_pypi_providers
    run_command(
  File "/opt/airflow/scripts/in_container/in_container_utils.py", line 54, in run_command
    result = subprocess.run(cmd, **kwargs)
             ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/usr/local/lib/python3.12/subprocess.py", line 573, in run
    raise CalledProcessError(retcode, process.args,
```

---

_Label `enhancement` added by @potiuk on 2025-03-21 09:23_

---

_Comment by @charliermarsh on 2025-03-22 00:31_

Hmm, we should _already_ be retrying here. I guess the logs in your CI run don't include our own `--verbose` logging so there's no way to tell if it kicked in here.

---

_Comment by @potiuk on 2025-03-22 07:36_

> Hmm, we should _already_ be retrying here. I guess the logs in your CI run don't include our own `--verbose` logging so there's no way to tell if it kicked in here.

Maybe adding a retry count information when it finally fails will be able to pin-down the problem better:

```
We tried x times but finally gave up.
```

---

_Comment by @zanieb on 2025-04-01 17:12_

I think we do that generally but the special-case retries for streamed downloads doesn't.

I think we could add that in https://github.com/astral-sh/uv/blob/a1a4d820d54607613bceb4973789b67c8c1c1232/crates/uv-client/src/cached_client.rs#L600 somehow?

---

_Comment by @potiuk on 2025-04-03 11:40_

I think there is a bit more to the issue than just showing the retry. Let me explain a bit more context, maybe it will allow to find a better remedy or implement some workarounds. 


I have just sped up the tests of Airflow to run "slices" of tests with lower-resolution tests and it seems that the issue started happening more frequently, and my guess it might have something to do with an "aggressiveness" of `uv` downloads, triggering errors on the PyPI side when too many requests for the same, big files to download are run nearly in parallel..


We have more than 90+ providers in Airlfow and we run a set of tests (massively parallel in GitHub actions) where for each provider we run `uv sync --resolution lowest` - to make sure that our provider's lower-bound dependencies are good. As mentioned - the https://github.com/apache/airflow/pull/48696 split our 90+ sequentially (in up to 4 separate containers per runnr) parallell test runs of  essentially `uv sync --resolution lowest-direct && uv run pytest`. The `uv sync` in each of those runs takes a lot of time - because each provider has it's own set of dependencies, and often it means that the lowest dependencies there have to be downloaded and built, because essentially only the "highest" dependencies are already in the cache/installed in the image. So caching can't help with speed-ups here too much.

The PR above added further split - the whole list of 90+ providers is split into 5 slices, and they are run in paralllel. Additionally, those jobs are multiplied by a number of Python versions (3.9 - 3.12 currently) - so we have essentially 20 parallel jobs like that each of them with 4 containers doing essentially `uv sync` on some providers of airlfow at the same time on 20 different machines. I **think** that is what triggers the issue, as it seem to start happening more frequently when I run it. Sometimes even it happen in bursts - i.e. multiple jobs running on separate Github runners get affected about the same time.

You can see it for example here (if you look at all the jobs - they all failed, and all of them failing on trying to retrieve `pyspark` or `pydruid`: https://github.com/apache/airflow/actions/runs/14232431820/job/39885872511

Example failure:

```python
   × Failed to download and build `pyspark==3.1.3`
    ├─▶ Failed to extract archive
    ├─▶ failed to unpack
    │   `/root/.cache/uv/sdists-v9/.tmp916EEe/pyspark-3.1.3/deps/jars/derby-10.12.1.1.jar`
    ├─▶ failed to unpack `pyspark-3.1.3/deps/jars/derby-10.12.1.1.jar` into
    │   `/root/.cache/uv/sdists-v9/.tmp916EEe/pyspark-3.1.3/deps/jars/derby-10.12.1.1.jar`
    ├─▶ error decoding response body
    ├─▶ request or response body error
    ├─▶ error reading a body from connection
    ╰─▶ stream closed because of a broken pipe
    help: `pyspark` (v3.1.3) was included because `apache-airflow[all]`
          (v3.0.0.dev0) depends on `apache-airflow-providers-apache-spark`
          (v5.1.1) which depends on `pyspark>=3.1.3`
```

So the question is - can we somehow increase the retry time, get more exponential back-off kick-in in those cases or maybe get some less "aggressive" behaviour in those specific jobs ? 

I understand it's not likely `uv` fault - it just tries to do things as fast as possible (which is the whole point of `uv` :)) - but also we need to recognize the fact that maybe some scenarios like that can hammer pypi and fastly in front of it a bit "too" hard, and maybe there should be a way to tune it down (or maybe there already is?) 


---

_Comment by @zanieb on 2025-04-03 13:15_

> I think there is a bit more to the issue than just showing the retry. Let me explain a bit more context, maybe it will allow to find a better remedy or implement some workarounds.

Of course, we just want to show the retry properly so we can easily tell if we're retrying as we should. I moved that to another issue https://github.com/astral-sh/uv/issues/12602

You could lower `UV_CONCURRENT_DOWNLOADS`?



---

_Label `enhancement` removed by @zanieb on 2025-04-03 13:15_

---

_Label `bug` added by @zanieb on 2025-04-03 13:15_

---

_Comment by @potiuk on 2025-04-03 14:38_

I am trying something else - we figured that we can change our CI to actually bind same volume as ~/.cache that will be shared both - between parallel containers running uv sync **and** subsequent runs of tests on the same machine for the sequential part. 

I just merged PR for that https://github.com/apache/airflow/pull/48740 and I hope it might mitigate the issue (credits to @ashb on suggesting the approach).

---

_Comment by @potiuk on 2025-04-03 18:18_

Quick feedback. Mounting a folder to `/root/.cache` and sharing it among all the jobs 80 (eventually) or so containers running per machine helped . This might be a non-issue. 

---

_Comment by @potiuk on 2025-04-04 17:13_

That definitely works better now when we mounted host directory as a `~/.cache`  - I proposed to update the docs to mention the case in #12676 - so that others can discover it more easily, but other than that it can be closed (the #12602 is still nice to have anyway though)

---

_Comment by @potiuk on 2025-04-05 08:31_

FYI.. We had ONE case I know of that happened since.. So the problem is not entirely fixed but it is at least 1-2 order of magnitude less frequent now:

https://github.com/apache/airflow/actions/runs/14277256505/job/40022307363#step:6:1552

```
  Check if provider atlassian.jira is excluded in {'3.9': ['cloudant']}
  Provider atlassian.jira is not excluded.
  Ignoring existing lockfile due to change in resolution mode: `highest` vs. `lowest-direct`
  warning: The direct dependency `types-protobuf` is unpinned. Consider setting a lower bound when using `--resolution lowest` to avoid using outdated versions.
    × Failed to download and build `pyspark==3.1.3`
    ├─▶ Failed to extract archive
    ├─▶ failed to unpack
    │   `/root/.cache/uv/sdists-v9/.tmp9iiJYH/pyspark-3.1.3/deps/jars/hk2-utils-2.6.1.jar`
    ├─▶ failed to unpack `pyspark-3.1.3/deps/jars/hk2-utils-2.6.1.jar` into
    │   `/root/.cache/uv/sdists-v9/.tmp9iiJYH/pyspark-3.1.3/deps/jars/hk2-utils-2.6.1.jar`
    ├─▶ error decoding response body
    ├─▶ request or response body error
    ├─▶ error reading a body from connection
    ╰─▶ stream closed because of a broken pipe
    help: `pyspark` (v3.1.3) was included because `apache-airflow[all]`
          (v3.0.0.dev0) depends on `apache-airflow-providers-apache-spark`
          (v5.1.1) which depends on `pyspark>=3.1.3`
  Test failed with 1. Dumping logs
  Wait 10 seconds for logs to find their way to stderr.
```

---

_Comment by @larruda on 2025-04-28 21:13_

so this is why my installation is taking so long! 

---

_Comment by @potiuk on 2025-05-03 22:56_

We are still getting that one occassionally in our CI  - for example here:

https://github.com/apache/airflow/actions/runs/14813939634/job/41592225387#step:6:1105

Is it possible to get some extra informatioln about the number of retries and maybe even control it ?


```python
  Forcing dependencies to lowest versions for Airflow.
  
  Ignoring existing lockfile due to change in resolution mode: `highest` vs. `lowest-direct`
    × Failed to download and build `pydruid==0.4.1`
    ├─▶ Failed to extract archive: pydruid==0.4.1
    ├─▶ failed to unpack
    │   `/root/.cache/uv/sdists-v9/.tmp08tfKH/pydruid-0.4.1/env/lib/python2.7/site-packages/virtualenv.py`
    ├─▶ failed to unpack
    │   `pydruid-0.4.1/env/lib/python2.7/site-packages/virtualenv.py` into
    │   `/root/.cache/uv/sdists-v9/.tmp08tfKH/pydruid-0.4.1/env/lib/python2.7/site-packages/virtualenv.py`
    ├─▶ error decoding response body
    ├─▶ request or response body error
    ├─▶ error reading a body from connection
    ╰─▶ stream closed because of a broken pipe
    help: `pydruid` (v0.4.1) was included because `apache-airflow[all]` (v3.1.0)
          depends on `apache-airflow-providers-apache-druid` (v4.1.1) which
          depends on `pydruid>=0.4.1`
  Test failed with 1. Dumping logs
```

---

_Comment by @konstin on 2025-05-03 23:06_

Re extra information, we log all errors we retry when `-v` or more specifically `RUST_LOG=uv_client=debug` is set.

---

_Comment by @konstin on 2025-05-03 23:35_

I can't reproduce this locally, but I gave it a try at #13281.

---

_Comment by @notatallshaw on 2025-05-05 14:24_

> ├─▶ failed to unpack
>     │   `/root/.cache/uv/sdists-v9/.tmp08tfKH/pydruid-0.4.1/env/lib/python2.7/site-packages/virtualenv.py`
>     ├─▶ failed to unpack
>     │   `pydruid-0.4.1/env/lib/python2.7/site-packages/virtualenv.py` into
>     │   `/root/.cache/uv/sdists-v9/.tmp08tfKH/pydruid-0.4.1/env/lib/python2.7/site-packages/virtualenv.py`
>     ├─▶ error decoding response body

Python 2.7? 🤨

---

_Comment by @potiuk on 2025-05-05 14:32_

> > ├─▶ failed to unpack
> > │   `/root/.cache/uv/sdists-v9/.tmp08tfKH/pydruid-0.4.1/env/lib/python2.7/site-packages/virtualenv.py`
> > ├─▶ failed to unpack
> > │   `pydruid-0.4.1/env/lib/python2.7/site-packages/virtualenv.py` into
> > │   `/root/.cache/uv/sdists-v9/.tmp08tfKH/pydruid-0.4.1/env/lib/python2.7/site-packages/virtualenv.py`
> > ├─▶ error decoding response body
> 
> Python 2.7? 🤨

OH GOD.... Yeah... You are completely right.... So it seems it is actually trying to build installl a VERY OLD PyDruid. (2018)... We seem to have still a few of those very old lower-bindings that we can bump.


Thanks @notatallshaw For that 💡 


---

_Comment by @potiuk on 2025-05-05 14:38_

https://github.com/apache/airflow/pull/50205

---

_Comment by @potiuk on 2025-05-05 14:38_

I think we can close that one :)

---

_Comment by @potiuk on 2025-05-21 17:23_

Actually I have another theory why it happens now. Will open a separate issue not to confuse the thread. 

---

_Closed by @potiuk on 2025-05-21 17:23_

---
