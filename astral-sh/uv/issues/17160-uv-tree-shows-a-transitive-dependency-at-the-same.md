```yaml
number: 17160
title: "`uv tree` shows a transitive dependency at the same depth (root) than the current package"
type: issue
state: open
author: AlexandreDecan
labels:
  - bug
assignees: []
created_at: 2025-12-17T08:13:55Z
updated_at: 2025-12-17T11:24:09Z
url: https://github.com/astral-sh/uv/issues/17160
synced_at: 2026-01-10T03:11:35Z
```

# `uv tree` shows a transitive dependency at the same depth (root) than the current package

---

_Issue opened by @AlexandreDecan on 2025-12-17 08:13_

### Summary

Hello, 

I'm currently running several experiments to simulate the installation of various Python packages through time. The process is basically the following one: (1) Create an empty project called "root"; (2) Add the target package as a dependency; (3) Display the dependency tree. With this process, the dependency tree should be something like "root > target_package > [TREE of the target package]". However, I found many cases where additional packages are reported in the tree, at the same level than `root`. 

Consider the following example with `apache-airflow-providers-trino` in version `4.3.0`, whose installation is simulated on `2023-01-01` with Python 3.11. We start by creating the bare project: 
`uv init . --bare --name root --python 3.11`
Then we add the target package as a dependency: 
`uv add apache-airflow-providers-trino==4.3.0 --exclude-newer 2023-01-01 --no-sync --python 3.11`
A quick look at the `pyproject.toml` confirms that only `apache-airflow-providers-trino` was added as a direct dependency: 
`cat pyproject.toml` returns:
```
[project]
name = "root"
version = "0.1.0"
requires-python = ">=3.11"
dependencies = [
    "apache-airflow-providers-trino==4.3.0",
]
```

Then, I use `uv tree` to display the dependency tree. Since the tree is quite large, I limit its depth to 1 and truncate the output to the first few lines (this is enough to see the "issues"): 
`uv tree --no-dev --no-dedupe --quiet --depth 1 | head -5 `

This displays: 

```
root v0.1.0
└── apache-airflow-providers-trino v4.3.0
apache-airflow-core v3.1.5
├── a2wsgi v1.10.10
├── aiosqlite v0.22.0

```

My question is: why is `apache-airflow-core` being reported at the same level than `root`? It should be somewhere nested below `apache-airflow-providers-trino` (and actually is reported there as well) only.

Thanks!

### Platform

Linux 6.17.9-300.fc43.x86_64 x86_64 GNU/Linux

### Version

uv 0.9.15

### Python version

3.11

---

_Label `bug` added by @AlexandreDecan on 2025-12-17 08:13_

---

_Renamed from "uv tree shows a transitive dependency as a direct one" to "`uv tree` shows a transitive dependency as the same level than the current package" by @AlexandreDecan on 2025-12-17 08:15_

---

_Renamed from "`uv tree` shows a transitive dependency as the same level than the current package" to "`uv tree` shows a transitive dependency at the same depth (root) than the current package" by @AlexandreDecan on 2025-12-17 08:15_

---

_Comment by @konstin on 2025-12-17 10:52_

This is definitively a bug.

MRE:
```toml
[project]
name = "debug"
version = "0.1.0"
requires-python = ">=3.11"
dependencies = [
  "apache-airflow",
]
```

The problem is that our cycle removing code is orphaning an apache node. When adding some debug prints in `TreeDisplay`:

```
$ uv tree
Resolved 127 packages in 7ms
Removing cycle edge: Some((NodeIndex(2), NodeIndex(3)))
Removing cycle edge: Some((NodeIndex(4), NodeIndex(3)))
NodeIndex(0) Root
NodeIndex(1) Package(PackageId { name: PackageName("debug"), version: Some("0.1.0"), source: Editable("") })
NodeIndex(2) Package(PackageId { name: PackageName("apache-airflow"), version: Some("3.1.5"), source: Registry(Url(UrlString("https://pypi.org/simple"))) })
NodeIndex(3) Package(PackageId { name: PackageName("apache-airflow-core"), version: Some("3.1.5"), source: Registry(Url(UrlString("https://pypi.org/simple"))) })
NodeIndex(4) Package(PackageId { name: PackageName("apache-airflow-task-sdk"), version: Some("1.1.5"), source: Registry(Url(UrlString("https://pypi.org/simple"))) })
NodeIndex(5) Package(PackageId { name: PackageName("a2wsgi"), version: Some("1.10.10"), source: Registry(Url(UrlString("https://pypi.org/simple"))) })
```

```
debug v0.1.0
└── apache-airflow v3.1.5
    ├── apache-airflow-core v3.1.5
    │   ├── a2wsgi v1.10.10
    │   ├── aiosqlite v0.22.0
...
    └── apache-airflow-task-sdk v1.1.5 (*)
apache-airflow-core v3.1.5 (*)
(*) Package tree already displayed
```

CC @charliermarsh re https://github.com/astral-sh/uv/pull/8564, it looks like we're removing edges in a reachable cycle, but I don't understand why we're defining roots as having no incoming edges, instead of starting at the workspace members.

---

_Comment by @AlexandreDecan on 2025-12-17 11:24_

I'm glad I found something useful :-) 

---
