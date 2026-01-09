---
number: 7234
title: "Performance: uv {add|remove|sync} on private registry slower and finding \"stale response\""
type: issue
state: closed
author: billy-doyle
labels:
  - question
  - tracing
assignees: []
created_at: 2024-09-09T22:11:29Z
updated_at: 2024-10-21T22:14:31Z
url: https://github.com/astral-sh/uv/issues/7234
synced_at: 2026-01-07T13:12:17-06:00
---

# Performance: uv {add|remove|sync} on private registry slower and finding "stale response"

---

_Issue opened by @billy-doyle on 2024-09-09 22:11_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->

Doing any `uv` command results in significantly slower times then just using pypi index url. Only change is using this private index url (pyproject.toml) like this:

```toml
[tool.uv]
index-url = "https://my-private-registry/repository/pypi/simple"
```

Using pypi (used curlify as an example public package but applies to any, requests, pandas etc):
```bash
(data_sci) billy@sa ~/data_sci % uv remove curlify                   
Resolved 217 packages in 482ms
   Built data-sci @ file:///Users/billy/data_sci
Prepared 1 package in 489ms
Uninstalled 2 packages in 4ms
Installed 1 package in 3ms
 - curlify==2.2.1
 ~ data-sci==0.0.0 (from file:///Users/billy/data_sci)
```

Using private registry (sonatype nexus if it matters):
```bash
(data_sci) billy@sa ~/data_sci % uv remove curlify
Resolved 217 packages in 4.62s
   Built data-sci @ file:///Users/billy/data_sci
Prepared 1 package in 2.50s
Uninstalled 2 packages in 6ms
Installed 1 package in 2ms
 - curlify==2.2.1
 ~ data-sci==0.0.0 (from file:///Users/billy/data_sci)
```

Debug logs with `-vvv` for a package, here `confluent-kafka` as an example. Seeing a lot of these `Found stale response` logs.

```log
    0.457895s   6ms DEBUG uv_client::cached_client Found stale response for: https://my-private-registry/repository/pypi/simple/confluent-kafka/
    0.457901s   6ms DEBUG uv_client::cached_client Sending revalidation request for: https://my-private-registry/repository/pypi/simple/confluent-kafka/
    uv_client::cached_client::revalidation_request url="https://my-private-registry/repository/pypi/simple/confluent-kafka/"
...
    0.963099s 511ms DEBUG uv_client::cached_client Found not-modified response for: https://my-private-registry/repository/pypi/simple/confluent-kafka/
    uv_client::cached_client::refresh_cache file=/Users/billy/Library/Caches/uv/simple-v12/index/4dd2fc1dc4fa3d1d/confluent-kafka.rkyv
uv_resolver::version_map::from_metadata 
uv_distribution::distribution_database::get_or_build_wheel_metadata dist=confluent-kafka==2.5.0
  uv_client::registry_client::wheel_metadata built_dist=confluent-kafka==2.5.0
    uv_client::cached_client::get_serde 
      uv_client::cached_client::get_cacheable 
        uv_client::cached_client::read_and_parse_cache file=/Users/billy/Library/Caches/uv/wheels-v1/index/4dd2fc1dc4fa3d1d/confluent-kafka/confluent_kafka-2.5.0-cp312-cp312-macosx_10_9_x86_64.msgpack
uv_client::cached_client::from_path_sync path="/Users/billy/Library/Caches/uv/wheels-v1/index/4dd2fc1dc4fa3d1d/confluent-kafka/confluent_kafka-2.5.0-cp312-cp312-macosx_10_9_x86_64.msgpack"
        0.964585s   0ms DEBUG uv_client::cached_client Found stale response for: https://my-private-registry/repository/pypi/packages/confluent-kafka/2.5.0/confluent_kafka-2.5.0-cp312-cp312-macosx_10_9_x86_64.whl#sha256=db987d8953d0d58a28a455e43a1da74a0e9dec7a12a74f5abd85a7cb308aefd4
        0.964598s   0ms DEBUG uv_client::cached_client Sending revalidation request for: https://my-private-registry/repository/pypi/packages/confluent-kafka/2.5.0/confluent_kafka-2.5.0-cp312-cp312-macosx_10_9_x86_64.whl#sha256=db987d8953d0d58a28a455e43a1da74a0e9dec7a12a74f5abd85a7cb308aefd4
        uv_client::cached_client::revalidation_request url="https://my-private-registry/repository/pypi/packages/confluent-kafka/2.5.0/confluent_kafka-2.5.0-cp312-cp312-macosx_10_9_x86_64.whl#sha256=db987d8953d0d58a28a455e43a1da74a0e9dec7a12a74f5abd85a7cb308aefd4"
...
uv_resolver::resolver::choose_version package=confluent-kafka
  1.104862s   0ms DEBUG uv_resolver::resolver Searching for a compatible version of confluent-kafka (>=2.3.0, <3.0.0)
  1.104876s   0ms DEBUG uv_resolver::resolver Selecting: confluent-kafka==2.5.0 [preference] (confluent_kafka-2.5.0-cp312-cp312-macosx_10_9_x86_64.whl)
uv_resolver::resolver::get_dependencies_forking package=confluent-kafka, version=2.5.0
  uv_resolver::resolver::get_dependencies package=confluent-kafka, version=2.5.0
```


---

_Comment by @charliermarsh on 2024-09-09 23:39_

There could be lots of reasons for this...

1. Does the registry support `.metadata` files? I.e., does `https://my-private-registry/repository/pypi/packages/confluent-kafka/2.5.0/confluent_kafka-2.5.0-cp312-cp312-macosx_10_9_x86_64.whl.metadata` exist?
2. Does the registry support range requests? (There will be warnings in the `--verbose` output if not.)
3. Does the registry include HTTP caching headers for the simple API responses (like `https://my-private-registry/repository/pypi/simple/confluent-kafka`)? PyPI provides a 10-minute caching header.

---

_Label `question` added by @charliermarsh on 2024-09-09 23:41_

---

_Comment by @billy-doyle on 2024-09-09 23:57_

> There could be lots of reasons for this...
> 
> 1. Does the registry support `.metadata` files? I.e., does `https://my-private-registry/repository/pypi/packages/confluent-kafka/2.5.0/confluent_kafka-2.5.0-cp312-cp312-macosx_10_9_x86_64.whl.metadata` exist?
> 2. Does the registry support range requests? (There will be warnings in the `--verbose` output if not.)
> 3. Does the registry include HTTP caching headers for the simple API responses (like `https://my-private-registry/repository/pypi/simple/confluent-kafka`)? PyPI provides a 10-minute caching header.

Hmm going off this https://help.sonatype.com/en/pypi-repositories.html and looking in the registry Im not seeing any .whl.metadata file

As for 2 I only see a single warn that the cached request doesnt match current request for the package, so don't think its that.

And 3 ive found https://community.sonatype.com/t/setting-cache-control-parameter-in-nexus-repository-manager-3-13/1434/2

I presume should take this up with my private registry, anything `uv` can do log wise to indicate these possible checks?

---

_Comment by @zanieb on 2024-10-21 21:47_

I'm not quite sure if there's something actionable for us to take on here.

---

_Label `tracing` added by @zanieb on 2024-10-21 21:47_

---

_Comment by @billy-doyle on 2024-10-21 21:50_

Per https://help.sonatype.com/en/sonatype-nexus-repository-3-71-0-release-notes.html, NEXUS-43268 | Caching works as expected for pypi.org simple index pages. Upgraded this and saw much better perf


---

_Closed by @billy-doyle on 2024-10-21 21:50_

---

_Comment by @zanieb on 2024-10-21 22:14_

Thank you for following up!

---
