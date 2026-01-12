```yaml
number: 1958
title: Include linehaul information in user agent
type: issue
state: closed
author: pradyunsg
labels:
  - good first issue
  - internal
assignees: []
created_at: 2024-02-25T00:12:35Z
updated_at: 2024-03-18T10:46:59Z
url: https://github.com/astral-sh/uv/issues/1958
synced_at: 2026-01-12T15:58:33Z
```

# Include linehaul information in user agent

---

_@pradyunsg_

This is effectively a request to include download-related information available to PyPI when interacting with the index server, so that informtion can used to make ecosystem-wide decision (by querying said information via https://warehouse.pypa.io/api-reference/bigquery-datasets.html#download-statistics-table).

https://github.com/pypi/linehaul-cloud-function is the PyPI side implementation. https://github.com/pypa/pip/blob/24.0/src/pip/_internal/network/session.py#L109 is the pip side implementation.

This data powers decision making such as https://pypistats.org/packages/__all__ (and similar sites), https://mayeut.github.io/manylinux-timeline/ and a few ad-hoc queries to determine usage patterns across the ecosystem.


---

_Comment by @charliermarsh on 2024-02-25 00:27_

Perfect, thanks @pradyunsg!

---

_Label `internal` added by @charliermarsh on 2024-02-25 00:27_

---

_Comment by @charliermarsh on 2024-02-25 00:30_

Everything here looks pretty straightforward (and we should already have it all available), perhaps with the exception of `setuptools_version`.

---

_Label `good first issue` added by @charliermarsh on 2024-02-25 00:30_

---

_Comment by @pradyunsg on 2024-02-25 00:56_

> perhaps with the exception of setuptools_version

Yea, skipping values you don't have readily available seems fine. 

---

_Comment by @hauntsaninja on 2024-02-25 03:27_

Last I benchmarked, construction of user agent was surprisingly expensive in pip, took like 200ms.

---

_Comment by @hauntsaninja on 2024-02-25 03:31_

Hm, looks like it might cost up to 600ms in a representative environment:
```
λ hyperfine -w 3 'python -c "import pkg_resources; from pip._internal.network.session import *; user_agent()"' 'python -c "import pkg_resources; from pip._internal.network.session import *"'
Benchmark 1: python -c "import pkg_resources; from pip._internal.network.session import *; user_agent()"
  Time (mean ± σ):      1.423 s ±  0.031 s    [User: 0.677 s, System: 0.365 s]
  Range (min … max):    1.388 s …  1.482 s    10 runs
 
Benchmark 2: python -c "import pkg_resources; from pip._internal.network.session import *"
  Time (mean ± σ):     854.2 ms ±   6.0 ms    [User: 414.9 ms, System: 198.2 ms]
  Range (min … max):   843.4 ms … 862.7 ms    10 runs
```

If pip were to skip getting setuptools version then pip's user agent construction costs 190ms for me. Next most expensive thing is rustc version, which wouldn't be horrible to cache. I guess off topic for this tracker, but Pradyun let me know if pip is interested in PRs here

---

_Comment by @konstin on 2024-02-25 14:15_

I've asked upstream about `rustc` startup time: https://github.com/rust-lang/rustup/issues/2626#issuecomment-1962952569. Is there a page i can link with rust version stats to motivate this use case?

> Next most expensive thing is rustc version, which wouldn't be horrible to cache.

The entrypoint is a shim, so afaik you can't cache it reliably (i'm happy to query the default rustc from a different place though).

Is the setuptools information from uv relevant given that we don't install it be default, and always use the latest (compatible) version in build envs?

Except for the `rustc` call and the (base) interpreter metadata call we cache already, i think we should be able to do without any subprocess calls, i.e. fast enough it's not noticeable on profiles.

---

_Comment by @alex on 2024-02-25 14:19_

In general I would say that the setuptools information is much less useful than it used to be: projects who need a newer setuptools can now reliably depend on it with `build-system.requires`.

What do you mean a page with rust version stats? Are you asking where you can see the stats from PyPI? They're all available in BigQuery. Here's an example of the type of analysis I do with it: 
[pyca_cryptography_rage_dashboard (1).pdf](https://github.com/astral-sh/uv/files/14396641/pyca_cryptography_rage_dashboard.1.pdf)


---

_Comment by @charliermarsh on 2024-02-25 14:19_

Let’s just stick to what we have access to (I’d like to omit rustc and setuptools for now).

---

_Comment by @konstin on 2024-02-25 14:37_

> What do you mean a page with rust version stats? Are you asking where you can see the stats from PyPI? They're all available in BigQuery. Here's an example of the type of analysis I do with it:
[pyca_cryptography_rage_dashboard (1).pdf](https://github.com/astral-sh/uv/files/14396641/pyca_cryptography_rage_dashboard.1.pdf)

Yes, something like [__all__](https://pypistats.org/packages/__all__) but including the rust version. Getting the data from bigquery is quite the overhead if someone wants just simple caniuse.com style check.

---

_Comment by @alex on 2024-02-25 14:38_

Ah. I'm not aware of any website that displays rust versions for all of PyPI.

---

_Assigned to @konstin by @konstin on 2024-03-14 09:27_

---

_Closed by @konstin on 2024-03-18 10:46_

---
