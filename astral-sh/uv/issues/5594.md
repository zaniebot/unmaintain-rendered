```yaml
number: 5594
title: "RFC: Handling diverging requires-python in workspaces with forking on the python version"
type: issue
state: open
author: konstin
labels:
  - enhancement
  - needs-design
assignees: []
created_at: 2024-07-30T11:16:48Z
updated_at: 2025-07-01T19:52:58Z
url: https://github.com/astral-sh/uv/issues/5594
synced_at: 2026-01-10T03:32:44Z
```

# RFC: Handling diverging requires-python in workspaces with forking on the python version

---

_Issue opened by @konstin on 2024-07-30 11:16_

When using workspaces, we uses the combined requires-python of all packages. Say you have foo that requires python 3.8 and bar that requires python 3.12, we require that all dependencies are compatible with python 3.8. This limits the usefulness of workspaces (#5578, #5398).

An alternative would the following: We create separate resolution for workspace members with separate python requirements: When we encounter foo and bar, we fork into a resolution for 3.8 through 3.11 and one for at least 3.12. In the `>=3.8,<3.12` one, our root requirements are only foo, while for `>=3.12`, we start with foo and bar. When running `uv sync -p 3.8` we then only install foo and its deps, while with `uv sync -p 3.12`, we install foo and bar and their deps. `uv sync -p 3.8 --package bar` is forbidden (error: bar requires at least python `>=3.12`, but you have `3.8`). Naturally, you can have a requirement for `bar` in `foo`, while a requirement `bar; python_version >= "3.12"` is allowed in foo; the inverse is always allowed.

Implementing this likely requires #5583.

---

_Label `enhancement` added by @konstin on 2024-07-30 11:16_

---

_Label `needs-design` added by @konstin on 2024-07-30 11:16_

---

_Label `preview` added by @konstin on 2024-07-30 11:16_

---

_Label `preview` removed by @zanieb on 2024-08-20 18:21_

---

_Comment by @chadrik on 2025-07-01 19:40_

We're facing a pretty serious problem with the way that the workspaces treat the `requires-python` value of their constituent projects.

## Current problem

### Scenario 1

* we have a workspace with two projects:  `projectA` and `projectB`. `projectB` depends on `projectA`  
* `projectA` sets `requires-python >= 3.9` and `projectB` sets `requires-python >= 3.10`  
* uv lock uses python 3.10 to resolve dependency versions, which is the highest value in the intersection between `projectA` and `project B` (as described [here](https://github.com/eth3lbert/uv/commit/bf8934e3e442efb4be5af3d0d4db60ce6d72c600))  
* some of these resolved dependencies do not support python 3.9, so `projectA` cannot be successfully distributed and run using python 3.9

### Scenario 2

* to avoid the problem above, we establish a convention to set every project’s `requires-python` to the global minimum version across the workspace.   
  * thus, `projectA` sets `requires-python` to 3.9 and `projectB` also sets `requires-python` to 3.9, despite the fact that its code base uses features that are only available in python 3.10 
* however, when we publish `projectB,` its metadata declares that it supports python 3.9, so it is erroneously installed for python 3.9 projects and we encounter runtime errors due to the fact that the code uses python 3.10 features

## Desired behavior

The behavior that we want is something like the following:

* use `requires-python` to specify the minimum supported version of python for each workspace member (i.e. use this value as intended instead of trying to abuse it to work around this bug, as in scenario 2)
* running `uv lock` at the workspace level should find the intersection of all the `requires-python` values in the project, and use the *lowest* value, rather than the highest as it does now.  At the very least, we would like an option to control the strategy for choosing the `requires-python` that value that gets baked into `uv.lock`


---
