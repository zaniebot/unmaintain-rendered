```yaml
number: 15495
title: Avoid indexing the same workspace multiple times
type: pull_request
state: merged
author: dhruvmanila
labels:
  - server
assignees: []
merged: true
base: main
head: dhruv/avoid-indexing-multiple-times
created_at: 2025-01-15T12:44:22Z
updated_at: 2025-01-15T15:29:17Z
url: https://github.com/astral-sh/ruff/pull/15495
synced_at: 2026-01-12T15:55:51Z
```

# Avoid indexing the same workspace multiple times

---

_@dhruvmanila_

## Summary

This is not lazy indexing but it should somewhat help with #13686.

Currently, processing the change notifications for config files doesn't account for the fact that multiple config files could belong to the same workspace. This means that the server will re-index the same workspace `n` times where `n` is the number of file events which belongs to the same workspace. This is evident in the following trace logs:

**Trace logs:**

```
[Trace - 6:21:15 PM] Sending notification 'workspace/didChangeWatchedFiles'.
Params: {
    "changes": [
        {
            "uri": "file:///Users/dhruv/work/astral/parser-checkouts/home-assistant-core/pylint/ruff.toml",
            "type": 1
        },
        {
            "uri": "file:///Users/dhruv/work/astral/parser-checkouts/home-assistant-core/script/ruff.toml",
            "type": 2
        },
        {
            "uri": "file:///Users/dhruv/work/astral/parser-checkouts/home-assistant-core/script/scaffold/templates/ruff.toml",
            "type": 2
        },
        {
            "uri": "file:///Users/dhruv/work/astral/parser-checkouts/home-assistant-core/pyproject.toml",
            "type": 2
        }
    ]
}

...

[Trace - 6:21:19 PM] Sending notification 'workspace/didChangeWatchedFiles'.
Params: {
    "changes": [
        {
            "uri": "file:///Users/dhruv/work/astral/parser-checkouts/home-assistant-core/tests/testing_config/custom_components/ruff.toml",
            "type": 1
        },
        {
            "uri": "file:///Users/dhruv/work/astral/parser-checkouts/home-assistant-core/tests/ruff.toml",
            "type": 2
        }
    ]
}

...
```

**Server logs:**

```
 14.838004208s TRACE     ruff:main notification{method="workspace/didChangeWatchedFiles"}: ruff_server::server::api: enter
  14.838043583s DEBUG     ruff:main notification{method="workspace/didChangeWatchedFiles"}: ruff_server::session::index::ruff_settings: Indexing settings for workspace: /Users/dhruv/work/astral/parser-checkouts/home-assistant-core
  14.854324541s DEBUG ThreadId(55) ruff_server::session::index::ruff_settings: Ignored path via `exclude`: /Users/dhruv/work/astral/parser-checkouts/home-assistant-core/.vscode
  14.854388500s DEBUG ThreadId(55) ruff_server::session::index::ruff_settings: Ignored path via `exclude`: /Users/dhruv/work/astral/parser-checkouts/home-assistant-core/.git
  14.937713291s DEBUG     ruff:main notification{method="workspace/didChangeWatchedFiles"}: ruff_server::session::index::ruff_settings: Indexing settings for workspace: /Users/dhruv/work/astral/parser-checkouts/home-assistant-core
  14.954429833s DEBUG ThreadId(75) ruff_server::session::index::ruff_settings: Ignored path via `exclude`: /Users/dhruv/work/astral/parser-checkouts/home-assistant-core/.git
  14.954675708s DEBUG ThreadId(66) ruff_server::session::index::ruff_settings: Ignored path via `exclude`: /Users/dhruv/work/astral/parser-checkouts/home-assistant-core/.vscode
  15.041465500s DEBUG     ruff:main notification{method="workspace/didChangeWatchedFiles"}: ruff_server::session::index::ruff_settings: Indexing settings for workspace: /Users/dhruv/work/astral/parser-checkouts/home-assistant-core
  15.056731541s DEBUG ThreadId(78) ruff_server::session::index::ruff_settings: Ignored path via `exclude`: /Users/dhruv/work/astral/parser-checkouts/home-assistant-core/.vscode
  15.056796833s DEBUG ThreadId(78) ruff_server::session::index::ruff_settings: Ignored path via `exclude`: /Users/dhruv/work/astral/parser-checkouts/home-assistant-core/.git
  15.117545833s DEBUG     ruff:main notification{method="workspace/didChangeWatchedFiles"}: ruff_server::session::index::ruff_settings: Indexing settings for workspace: /Users/dhruv/work/astral/parser-checkouts/home-assistant-core
  15.133091666s DEBUG ThreadId(90) ruff_server::session::index::ruff_settings: Ignored path via `exclude`: /Users/dhruv/work/astral/parser-checkouts/home-assistant-core/.vscode
  15.133146500s DEBUG ThreadId(90) ruff_server::session::index::ruff_settings: Ignored path via `exclude`: /Users/dhruv/work/astral/parser-checkouts/home-assistant-core/.git
  15.220340666s TRACE ruff:worker:6 request{id=5 method="textDocument/diagnostic"}: ruff_server::server::api: enter
  15.220401458s DEBUG ruff:worker:6 request{id=5 method="textDocument/diagnostic"}: ruff_server::resolve: Included path via `include`: /Users/dhruv/work/astral/parser-checkouts/home-assistant-core/homeassistant/bootstrap.py
  18.577521250s TRACE     ruff:main notification{method="workspace/didChangeWatchedFiles"}: ruff_server::server::api: enter
  18.577561291s DEBUG     ruff:main notification{method="workspace/didChangeWatchedFiles"}: ruff_server::session::index::ruff_settings: Indexing settings for workspace: /Users/dhruv/work/astral/parser-checkouts/home-assistant-core
  18.616564583s DEBUG ThreadId(102) ruff_server::session::index::ruff_settings: Ignored path via `exclude`: /Users/dhruv/work/astral/parser-checkouts/home-assistant-core/.vscode
  18.616627291s DEBUG ThreadId(102) ruff_server::session::index::ruff_settings: Ignored path via `exclude`: /Users/dhruv/work/astral/parser-checkouts/home-assistant-core/.git
  18.687424250s DEBUG     ruff:main notification{method="workspace/didChangeWatchedFiles"}: ruff_server::session::index::ruff_settings: Indexing settings for workspace: /Users/dhruv/work/astral/parser-checkouts/home-assistant-core
  18.704441416s DEBUG ThreadId(114) ruff_server::session::index::ruff_settings: Ignored path via `exclude`: /Users/dhruv/work/astral/parser-checkouts/home-assistant-core/.vscode
  18.704694958s DEBUG ThreadId(121) ruff_server::session::index::ruff_settings: Ignored path via `exclude`: /Users/dhruv/work/astral/parser-checkouts/home-assistant-core/.git
  18.769627500s TRACE ruff:worker:4 request{id=6 method="textDocument/diagnostic"}: ruff_server::server::api: enter
  18.769696791s DEBUG ruff:worker:4 request{id=6 method="textDocument/diagnostic"}: ruff_server::resolve: Included path via `include`: /Users/dhruv/work/astral/parser-checkouts/home-assistant-core/homeassistant/bootstrap.py
```

This PR updates the logic to consider all the change events at once keeping track of the workspace path that have been already indexed.

I want to include this in tomorrow's release to check how this change would affect the linked issue.

## Test Plan

Run the same scenario as above and check the logs to see that the server isn't re-indexing the same workspace multiple times:

**Trace logs:**

```
[Trace - 6:04:07 PM] Sending notification 'workspace/didChangeWatchedFiles'.
Params: {
    "changes": [
        {
            "uri": "file:///Users/dhruv/work/astral/parser-checkouts/home-assistant-core/script/ruff.toml",
            "type": 1
        },
        {
            "uri": "file:///Users/dhruv/work/astral/parser-checkouts/home-assistant-core/script/scaffold/templates/ruff.toml",
            "type": 1
        },
        {
            "uri": "file:///Users/dhruv/work/astral/parser-checkouts/home-assistant-core/pylint/ruff.toml",
            "type": 2
        },
        {
            "uri": "file:///Users/dhruv/work/astral/parser-checkouts/home-assistant-core/pyproject.toml",
            "type": 2
        }
    ]
}

...

[Trace - 6:04:11 PM] Sending notification 'workspace/didChangeWatchedFiles'.
Params: {
    "changes": [
        {
            "uri": "file:///Users/dhruv/work/astral/parser-checkouts/home-assistant-core/tests/testing_config/custom_components/ruff.toml",
            "type": 1
        },
        {
            "uri": "file:///Users/dhruv/work/astral/parser-checkouts/home-assistant-core/tests/ruff.toml",
            "type": 2
        }
    ]
}

...
```

**Server logs:**

```
  17.047706750s TRACE     ruff:main notification{method="workspace/didChangeWatchedFiles"}: ruff_server::server::api: enter
  17.047747875s DEBUG     ruff:main notification{method="workspace/didChangeWatchedFiles"}: ruff_server::session::index::ruff_settings: Indexing settings for workspace: /Users/dhruv/work/astral/parser-checkouts/home-assistant-core
  17.080006083s DEBUG ThreadId(54) ruff_server::session::index::ruff_settings: Ignored path via `exclude`: /Users/dhruv/work/astral/parser-checkouts/home-assistant-core/.vscode
  17.080085708s DEBUG ThreadId(54) ruff_server::session::index::ruff_settings: Ignored path via `exclude`: /Users/dhruv/work/astral/parser-checkouts/home-assistant-core/.git
  17.145328791s TRACE ruff:worker:6 request{id=5 method="textDocument/diagnostic"}: ruff_server::server::api: enter
  17.145386166s DEBUG ruff:worker:6 request{id=5 method="textDocument/diagnostic"}: ruff_server::resolve: Included path via `include`: /Users/dhruv/work/astral/parser-checkouts/home-assistant-core/homeassistant/bootstrap.py
  20.756845958s TRACE     ruff:main notification{method="workspace/didChangeWatchedFiles"}: ruff_server::server::api: enter
  20.756923375s DEBUG     ruff:main notification{method="workspace/didChangeWatchedFiles"}: ruff_server::session::index::ruff_settings: Indexing settings for workspace: /Users/dhruv/work/astral/parser-checkouts/home-assistant-core
  20.781733916s DEBUG ThreadId(66) ruff_server::session::index::ruff_settings: Ignored path via `exclude`: /Users/dhruv/work/astral/parser-checkouts/home-assistant-core/.vscode
  20.781825875s DEBUG ThreadId(75) ruff_server::session::index::ruff_settings: Ignored path via `exclude`: /Users/dhruv/work/astral/parser-checkouts/home-assistant-core/.git
  20.848340750s TRACE ruff:worker:7 request{id=6 method="textDocument/diagnostic"}: ruff_server::server::api: enter
  20.848408041s DEBUG ruff:worker:7 request{id=6 method="textDocument/diagnostic"}: ruff_server::resolve: Included path via `include`: /Users/dhruv/work/astral/parser-checkouts/home-assistant-core/homeassistant/bootstrap.py
```

---

_Label `server` added by @dhruvmanila on 2025-01-15 12:44_

---

_@MichaReiser reviewed on 2025-01-15 12:53_

---

_Review comment by @MichaReiser on `crates/ruff_server/src/session/index.rs`:276 on 2025-01-15 12:53_

That makes sense to me. We do something similar in Red Knot where we process all changes and compute what requires updating and only then do the updating.

---

_Marked ready for review by @dhruvmanila on 2025-01-15 12:59_

---

_Review requested from @MichaReiser by @dhruvmanila on 2025-01-15 12:59_

---

_Comment by @github-actions[bot] on 2025-01-15 13:03_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review comment by @MichaReiser on `crates/ruff_server/src/session/index.rs`:276 on 2025-01-15 13:09_

I was a little confused at first why we aren't matching on the file names. It might be worth mentioning in the documentation that it's expected that the `FileEvent` are only for configuration files.

---

_@MichaReiser approved on 2025-01-15 13:10_

---

_Comment by @MichaReiser on 2025-01-15 13:14_

Unrelated to your changes but I noticed it in the tracing logs. I think we could improve the logs by making tracing aware that the walk_dir threads run as part of the request (untested)

```patch
Index: crates/ruff_server/src/session/index/ruff_settings.rs
IDEA additional info:
Subsystem: com.intellij.openapi.diff.impl.patch.CharsetEP
<+>UTF-8
===================================================================
diff --git a/crates/ruff_server/src/session/index/ruff_settings.rs b/crates/ruff_server/src/session/index/ruff_settings.rs
--- a/crates/ruff_server/src/session/index/ruff_settings.rs	(revision f20780584daf0b42a641639f3e8da4d67e005dff)
+++ b/crates/ruff_server/src/session/index/ruff_settings.rs	(date 1736946797069)
@@ -211,8 +211,11 @@
         let index = std::sync::RwLock::new(index);
         let has_error = AtomicBool::new(has_error);
 
+        let index_span = tracing::debug_span!("walk_dir");
+
         walker.run(|| {
             Box::new(|result| {
+                let index_span = index_span.clone();
+                let _span = index_span.entered();
                 let Ok(entry) = result else {
                     return WalkState::Continue;
                 };

```

---

_Comment by @dhruvmanila on 2025-01-15 13:17_

> Unrelated to your changes but I noticed it in the tracing logs. I think we could improve the logs by making tracing aware that the walk_dir threads run as part of the request (untested)

Thank you, will look at it in a follow-up PR.

---

_Comment by @MichaReiser on 2025-01-15 13:17_

This is great. It would be a much simpler fix than making indexing actually lazy!

---

_Comment by @dhruvmanila on 2025-01-15 13:28_

> This is great. It would be a much simpler fix than making indexing actually lazy!

It's not a complete fix because the client sends multiple `didChangeWatchedFiles` request as seen in the above trace logs. One of the ways to solve this would be to implement the file watching on the server side and not rely on client side implementation and we would make sure to only re-index the project once.

@MichaReiser I'm interested in hearing your opinion about whether that would be more feasible than lazy indexing. Maybe we could re-use the existing implementation from Red knot?

---

_Merged by @dhruvmanila on 2025-01-15 13:28_

---

_Closed by @dhruvmanila on 2025-01-15 13:28_

---

_Branch deleted on 2025-01-15 13:28_

---

_Comment by @MichaReiser on 2025-01-15 13:36_

> It's not a complete fix because the client sends multiple didChangeWatchedFiles request as seen in the above trace logs. 

I don't see how this can be avoided. At least not if the modifications happen too far apart. Red Knot debounces the events to combine events that happen shortly after each other, but that timeout must be small (<300ms), or users experience a lag when making changes in the editor. The server could implement its own debounce logic to debounce the change events coming from the client, but it increases the risk that we respond to requests with an outdated configuration, and it introduces a delay that will become noticeable to users.

Lazy indexing might also not help if the changes happen too far apart because VS code might request updated diagnostics, forcing Ruff to search for the configuration.  

Let's see if your change improves the experience. I think it's fairly likely that it will.

---

_Comment by @dhruvmanila on 2025-01-15 14:45_

> The server could implement its own debounce logic to debounce the change events coming from the client, but it increases the risk that we respond to requests with an outdated configuration, and it introduces a delay that will become noticeable to users.

This is an interesting idea. I think the risk is less because at least for pull diagnostics it's only when the server will ask the client to refresh the diagnostics then the client will ask for diagnostics:

https://github.com/astral-sh/ruff/blob/b5dbb2a1d70feae84c3d9ee3d9d84301636f7a7b/crates/ruff_server/src/server/api/notifications/did_change_watched_files.rs#L25-L47

We could experiment with a small amount of debounce period only after which the server will re-index the project and ask the client to refresh the diagnostics / publish the new diagnostics.

---

_Comment by @MichaReiser on 2025-01-15 14:46_

> We could experiment with a small amount of debounce period only after which the server will re-index the project and ask the client to refresh the diagnostics / publish the new diagnostics.

I suspect that VS codes file watcher already does some debouncing but maybe not?

---

_Comment by @dhruvmanila on 2025-01-15 15:29_

> I suspect that VS codes file watcher already does some debouncing but maybe not?

I'll need to look, maybe it does.

---
