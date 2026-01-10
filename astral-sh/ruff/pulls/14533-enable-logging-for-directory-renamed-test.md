```yaml
number: 14533
title: Enable logging for directory-renamed test
type: pull_request
state: merged
author: MichaReiser
labels:
  - ty
assignees: []
merged: true
base: main
head: micha/extra-logging-directory-renamed
created_at: 2024-11-22T16:36:59Z
updated_at: 2024-11-22T16:44:07Z
url: https://github.com/astral-sh/ruff/pull/14533
synced_at: 2026-01-10T20:50:57Z
```

# Enable logging for directory-renamed test

---

_Pull request opened by @MichaReiser on 2024-11-22 16:36_

## Summary
The test started to flake but I'm unable to reproduce the flake locally. Enable logging to understand if:

* The test doesn't wait long enough for all events to come in, because it only uses a timeout for the first event
* The test observes another event than in the cases where it succeeds

We can remove the logging when the flake is fixed.

It's a bit annoying that `setup_logging` only works for the current thread. That means we won't see the logs of the actual file watcher thread. We can explore more involved debugging if what's in this PR isn't sufficient.

Part of https://github.com/astral-sh/ruff/issues/14473

## Test Plan

Running the test prints the logs to stdout

```
2   0.000170s DEBUG red_knot_workspace::workspace::metadata Searching for a workspace in '/tmp/.tmp36yrfj/workspace'
2   0.000230s DEBUG red_knot_workspace::workspace::metadata The ancestor directories contain no `pyproject.toml`. Falling back to a virtual project.
2   0.000378s INFO red_knot_python_semantic::program Target version: Python 3.12
2   0.000391s DEBUG red_knot_python_semantic::module_resolver::resolver Adding first-party search path '/tmp/.tmp36yrfj/workspace'
2   0.000400s DEBUG red_knot_python_semantic::module_resolver::resolver Using vendored stdlib
2   0.003085s TRACE red_knot_workspace::db Salsa event: Event { thread_id: ThreadId(2), kind: WillExecute { database_key: dynamic_resolution_paths(Id(c00)) } }
2   0.003104s DEBUG red_knot_python_semantic::module_resolver::resolver Resolving dynamic module resolution paths
2   0.003146s DEBUG red_knot_workspace::watch::watcher Watching path: `/tmp/.tmp36yrfj/workspace`
2   0.003280s INFO red_knot_workspace::watch::workspace_watcher Set up file watchers for ["/tmp/.tmp36yrfj/workspace"]
2   0.113568s TRACE red_knot_workspace::db Salsa event: Event { thread_id: ThreadId(2), kind: WillExecute { database_key: resolve_module_query(Id(1400)) } }
2┐red_knot_python_semantic::module_resolver::resolver::resolve_module{name=sub.a}
2├─   0.113755s   0ms TRACE red_knot_python_semantic::module_resolver::resolver Resolved module `sub.a` to `/tmp/.tmp36yrfj/workspace/sub/a.py`
2┘
2   0.113825s TRACE red_knot_workspace::db Salsa event: Event { thread_id: ThreadId(2), kind: WillExecute { database_key: resolve_module_query(Id(1401)) } }
2┐red_knot_python_semantic::module_resolver::resolver::resolve_module{name=foo.baz}
2├─   0.113950s   0ms DEBUG red_knot_python_semantic::module_resolver::resolver Module `foo.baz` not found in search paths
2┘
2┐red_knot_workspace::workspace::index_package_files{package=workspace}
2├─   0.119111s   5ms INFO red_knot_workspace::workspace Found 3 files in package `workspace`
2┘
2   0.119189s DEBUG file_watching Try stopping watch with timeout 10s
2   0.129405s DEBUG file_watching Flushed file watcher
2   0.129420s DEBUG red_knot_workspace::watch::watcher Stop file watcher
2   0.142222s DEBUG file_watching Stopping file watcher
2   0.142238s DEBUG file_watching Event: Created { path: "/tmp/.tmp36yrfj/workspace/foo", kind: Directory }
2   0.142246s DEBUG file_watching Event: Deleted { path: "/tmp/.tmp36yrfj/workspace/sub", kind: Any }
2┐red_knot_workspace::db::changes::apply_changes{}
2├─   0.142312s   0ms TRACE red_knot_workspace::db Salsa event: Event { thread_id: ThreadId(2), kind: DidSetCancellationFlag }
2├─   0.142392s   0ms TRACE red_knot_workspace::db Salsa event: Event { thread_id: ThreadId(2), kind: DidSetCancellationFlag }
2├─   0.142414s   0ms TRACE red_knot_workspace::db Salsa event: Event { thread_id: ThreadId(2), kind: DidSetCancellationFlag }
2├─   0.142429s   0ms TRACE red_knot_workspace::db Salsa event: Event { thread_id: ThreadId(2), kind: DidSetCancellationFlag }
2├─   0.142459s   0ms TRACE red_knot_workspace::db Salsa event: Event { thread_id: ThreadId(2), kind: DidSetCancellationFlag }
2├─   0.142474s   0ms TRACE red_knot_workspace::db Salsa event: Event { thread_id: ThreadId(2), kind: DidSetCancellationFlag }
2├─   0.142486s   0ms TRACE red_knot_workspace::db Salsa event: Event { thread_id: ThreadId(2), kind: DidSetCancellationFlag }
2├─   0.142526s   0ms DEBUG red_knot_workspace::workspace Reloading files for package `workspace`
2├─   0.142543s   0ms TRACE red_knot_workspace::db Salsa event: Event { thread_id: ThreadId(2), kind: DidSetCancellationFlag }
2┘
2   0.142600s TRACE red_knot_workspace::db Salsa event: Event { thread_id: ThreadId(2), kind: WillExecute { database_key: resolve_module_query(Id(1400)) } }
2┐red_knot_python_semantic::module_resolver::resolver::resolve_module{name=sub.a}
2├─   0.142694s   0ms TRACE red_knot_workspace::db Salsa event: Event { thread_id: ThreadId(2), kind: DidValidateMemoizedValue { database_key: dynamic_resolution_paths(Id(c00)) } }
2├─   0.142717s   0ms DEBUG red_knot_python_semantic::module_resolver::resolver Module `sub.a` not found in search paths
2┘
2   0.142765s TRACE red_knot_workspace::db Salsa event: Event { thread_id: ThreadId(2), kind: WillExecute { database_key: resolve_module_query(Id(1401)) } }
2┐red_knot_python_semantic::module_resolver::resolver::resolve_module{name=foo.baz}
2├─   0.142872s   0ms TRACE red_knot_python_semantic::module_resolver::resolver Resolved module `foo.baz` to `/tmp/.tmp36yrfj/workspace/foo/baz/__init__.py`
2┘
2┐red_knot_workspace::workspace::index_package_files{package=workspace}
2├─   0.145997s   3ms INFO red_knot_workspace::workspace Found 3 files in package `workspace`
2┘

Process finished with exit code 0

```

---

_Label `red-knot` added by @MichaReiser on 2024-11-22 16:37_

---

_Marked ready for review by @MichaReiser on 2024-11-22 16:38_

---

_Review requested from @carljm by @MichaReiser on 2024-11-22 16:38_

---

_Review requested from @AlexWaygood by @MichaReiser on 2024-11-22 16:38_

---

_Review requested from @sharkdp by @MichaReiser on 2024-11-22 16:38_

---

_Merged by @MichaReiser on 2024-11-22 16:41_

---

_Closed by @MichaReiser on 2024-11-22 16:41_

---

_Branch deleted on 2024-11-22 16:41_

---

_Comment by @github-actions[bot] on 2024-11-22 16:44_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---
