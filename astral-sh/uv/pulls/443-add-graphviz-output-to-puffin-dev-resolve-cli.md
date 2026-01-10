```yaml
number: 443
title: " Add graphviz output to puffin-dev resolve-cli"
type: pull_request
state: merged
author: konstin
labels:
  - internal
assignees: []
merged: true
base: main
head: graphviz
created_at: 2023-11-17T14:09:08Z
updated_at: 2023-11-17T18:16:25Z
url: https://github.com/astral-sh/uv/pull/443
synced_at: 2026-01-10T15:50:28Z
```

#  Add graphviz output to puffin-dev resolve-cli

---

_Pull request opened by @konstin on 2023-11-17 14:09_

 I added output in graphviz DOT format to `puffin-dev resolve-cli` to help with debugging resolutions. This requires tracking the requested ranges in the graph. I also fixed the direction of the graph.

 Output for `black`:

```dot
digraph {
    0 [ label="click\n8.1.7"]
    1 [ label="black\n23.11.0"]
    2 [ label="packaging\n23.2"]
    3 [ label="mypy-extensions\n1.0.0"]
    4 [ label="tomli\n2.0.1"]
    5 [ label="pathspec\n0.11.2"]
    6 [ label="typing-extensions\n4.8.0"]
    7 [ label="platformdirs\n4.0.0"]
    1 -> 0 [ label=">=8.0.0"]
    1 -> 3 [ label=">=0.4.3"]
    1 -> 5 [ label=">=0.9.0"]
    1 -> 4 [ label=">=1.1.0"]
    1 -> 6 [ label=">=4.0.1"]
    1 -> 2 [ label=">=22.0"]
    1 -> 7 [ label=">=2"]
}
```

![image](https://github.com/astral-sh/puffin/assets/6826232/4a440fcd-6248-4349-8e1a-c3e0363e42b1)

transformers:

![image](https://github.com/astral-sh/puffin/assets/6826232/a13a693c-a8c0-4a4f-95d9-3458431c678a)

jupyter:

![graphviz](https://github.com/astral-sh/puffin/assets/6826232/ef730033-6fd9-4ea9-ac93-8c874c19a101)




---

_Review comment by @charliermarsh on `crates/puffin-resolver/src/resolution.rs`:55 on 2023-11-17 14:29_

Can we add a `From` for this and use `.into()` rather than making this public?

---

_@charliermarsh reviewed on 2023-11-17 14:30_

---

_@charliermarsh approved on 2023-11-17 14:30_

---

_Label `internal` added by @charliermarsh on 2023-11-17 14:30_

---

_Merged by @charliermarsh on 2023-11-17 18:16_

---

_Closed by @charliermarsh on 2023-11-17 18:16_

---

_Branch deleted on 2023-11-17 18:16_

---
