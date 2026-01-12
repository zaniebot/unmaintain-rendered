```yaml
number: 19041
title: "[ty] Add background request task support"
type: pull_request
state: merged
author: dhruvmanila
labels:
  - internal
  - ty
assignees: []
merged: true
base: main
head: dhruv/background-request-handler
created_at: 2025-06-30T05:43:17Z
updated_at: 2025-07-07T14:03:10Z
url: https://github.com/astral-sh/ruff/pull/19041
synced_at: 2026-01-12T15:56:30Z
```

# [ty] Add background request task support

---

_@dhruvmanila_

## Summary

This PR adds a new trait to support running a request in the background.

Currently, there exists a `BackgroundDocumentRequestHandler` trait which is similar but is scoped to a specific document (file in an editor context). The new trait `BackgroundRequestHandler` is not tied to a specific document nor a specific project but it's for the entire workspace.

This is added to support running workspace wide requests like computing the [workspace diagnostics](https://microsoft.github.io/language-server-protocol/specifications/lsp/3.17/specification/#workspace_diagnostic) or [workspace symbols](https://microsoft.github.io/language-server-protocol/specifications/lsp/3.17/specification/#workspace_symbol).

**Note:** There's a slight difference with what a "workspace" means between the server and ty. Currently, there's a 1-1 relationship between a workspace in an editor and the project database corresponding to that workspace in ty but this could change in the future when Micha adds support for multiple workspaces or multi-root workspaces.

The data that would be required by the request handler (based on implementing workspace diagnostics) is the list of databases (`ProjectDatabse`) corresponding to the projects in the workspace and the index (`Index`) that contains the open documents. The `WorkspaceSnapshot` represents this and is passed to the handler similar to `DocumentSnapshot`.

## Test Plan

This is used in implementing the workspace diagnostics which is where this is tested.


---

_Review requested from @carljm by @dhruvmanila on 2025-06-30 05:43_

---

_Review requested from @MichaReiser by @dhruvmanila on 2025-06-30 05:43_

---

_Review requested from @AlexWaygood by @dhruvmanila on 2025-06-30 05:43_

---

_Review requested from @sharkdp by @dhruvmanila on 2025-06-30 05:43_

---

_Review requested from @dcreager by @dhruvmanila on 2025-06-30 05:43_

---

_Label `internal` added by @dhruvmanila on 2025-06-30 05:43_

---

_Label `ty` added by @dhruvmanila on 2025-06-30 05:43_

---

_Comment by @github-actions[bot] on 2025-06-30 05:46_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
operator (https://github.com/canonical/operator)
- TOTAL MEMORY USAGE: ~106MB
+ TOTAL MEMORY USAGE: ~97MB

trio (https://github.com/python-trio/trio)
-     memo fields = ~156MB
+     memo fields = ~142MB

isort (https://github.com/pycqa/isort)
- TOTAL MEMORY USAGE: ~80MB
+ TOTAL MEMORY USAGE: ~88MB

vision (https://github.com/pytorch/vision)
- TOTAL MEMORY USAGE: ~334MB
+ TOTAL MEMORY USAGE: ~368MB

mkdocs (https://github.com/mkdocs/mkdocs)
-     memo fields = ~106MB
+     memo fields = ~97MB

tornado (https://github.com/tornadoweb/tornado)
- TOTAL MEMORY USAGE: ~156MB
+ TOTAL MEMORY USAGE: ~171MB

bandersnatch (https://github.com/pypa/bandersnatch)
-     memo fields = ~72MB
+     memo fields = ~66MB

paasta (https://github.com/yelp/paasta)
-     memo fields = ~156MB
+     memo fields = ~171MB

django-stubs (https://github.com/typeddjango/django-stubs)
- TOTAL MEMORY USAGE: ~171MB
+ TOTAL MEMORY USAGE: ~189MB

meson (https://github.com/mesonbuild/meson)
-     memo fields = ~334MB
+     memo fields = ~304MB

```
</details>


---

_Review request for @dcreager removed by @dhruvmanila on 2025-06-30 13:48_

---

_Review request for @carljm removed by @dhruvmanila on 2025-06-30 13:48_

---

_Review request for @sharkdp removed by @dhruvmanila on 2025-06-30 13:48_

---

_Review request for @AlexWaygood removed by @dhruvmanila on 2025-06-30 13:48_

---

_Review requested from @BurntSushi by @dhruvmanila on 2025-06-30 13:48_

---

_Review comment by @BurntSushi on `crates/ty_server/src/server/api.rs`:138 on 2025-07-02 16:17_

Could you add a comment saying why this code is kept around even though it isn't used?

---

_Review comment by @BurntSushi on `crates/ty_server/src/server/api/traits.rs`:64 on 2025-07-02 16:22_

One thing I'd find helpful here is some prose (perhaps on the module doc) that briefly explains the traits here and how they're connected.

If you think this code is going to be heavily refactored soon, then it might not make sense to add docs.

---

_@BurntSushi approved on 2025-07-02 16:23_

Nice! My only comments are requesting docs, probably because I'm somewhat unfamiliar with this part of the LSP. :-)

---

_@dhruvmanila reviewed on 2025-07-03 05:26_

---

_Review comment by @dhruvmanila on `crates/ty_server/src/server/api.rs`:138 on 2025-07-03 05:26_

I split the workspace diagnostics work into two PRs to ease up the review process. This is going to be used in the follow-up PR (#18939). Sorry, I should've highlighted this as a review comment!

---

_Review comment by @dhruvmanila on `crates/ty_server/src/server/api/traits.rs`:64 on 2025-07-03 05:28_

Definitely! Most of this stuff is currently in my and Micha's mind but it would be useful to have it written down. I'll prioritize it soon as there are now more people who'll be working on the server.

---

_@dhruvmanila reviewed on 2025-07-03 05:28_

---

_Merged by @dhruvmanila on 2025-07-03 11:01_

---

_Closed by @dhruvmanila on 2025-07-03 11:01_

---

_Branch deleted on 2025-07-03 11:01_

---

_Comment by @codspeed-hq[bot] on 2025-07-03 11:07_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
<!-- __CODSPEED_WALLTIME_PERFORMANCE_REPORT_COMMENT__ -->

## [CodSpeed WallTime Performance Report](https://codspeed.io/astral-sh/ruff/branches/dhruv%2Fbackground-request-handler?runnerMode=WallTime)

### Merging #19041 will **degrade performances by 4.82%**

<sub>Comparing <code>dhruv/background-request-handler</code> (1252436) with <code>main</code> (e212dc2)</sub>



### Summary

`❌ 1` regressions  
`✅ 7` untouched benchmarks  


> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/ruff/branches/dhruv%2Fbackground-request-handler?runnerMode=WallTime)._

### Benchmarks breakdown

|     | Benchmark | `BASE` | `HEAD` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ❌ | `` multithreaded[pydantic] `` | 321 ms | 337.3 ms | -4.82% |


---

_@MichaReiser reviewed on 2025-07-07 09:26_

---

_Review comment by @MichaReiser on `crates/ty_server/src/server/api/traits.rs`:61 on 2025-07-07 09:26_

I wonder if we should call this `SessionSnapshot` instead. It's unfortunate that the LSP specification made such a mess of the term workspace (which can refer to an open folder, or all open foldlers). 



---

_@MichaReiser reviewed on 2025-07-07 09:30_

---

_Review comment by @MichaReiser on `crates/ty_server/src/session.rs`:229 on 2025-07-07 09:30_

I'd suggest removing the `AssertUnwindSafe` here and insteaed wrap the entire snapshot in `crates/ty_server/src/server/api.rs` in an `AssertUnwindSafe` and add a comment saying that it's safe to move it across unwind boundaries because the value isn't used after unwinding.

---

_Comment by @MichaReiser on 2025-07-07 09:30_

Nice!

---

_@dhruvmanila reviewed on 2025-07-07 13:55_

---

_Review comment by @dhruvmanila on `crates/ty_server/src/session.rs`:229 on 2025-07-07 13:55_

That would mean that the `snapshot` parameter type of `BackgroundRequestHandler::run` would need to be `AssertUnwindSafe<SessionSnapshot>`. Or, are you suggesting to wrap the `AssertUnwindSafe` on the entire closure that is the first argument to `catch_unwind`?

---

_@dhruvmanila reviewed on 2025-07-07 13:59_

---

_Review comment by @dhruvmanila on `crates/ty_server/src/session.rs`:229 on 2025-07-07 13:59_

~~The former doesn't seems to work as the compiler still errors stating that some reference may not be safe to be transferred across the catch_unwind boundary.~~

Edit: Nevermind, it works.

---

_Comment by @dhruvmanila on 2025-07-07 14:03_

Addressed these review comments in https://github.com/astral-sh/ruff/pull/19177.

---
