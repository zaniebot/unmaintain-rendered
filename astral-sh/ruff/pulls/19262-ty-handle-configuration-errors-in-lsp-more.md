```yaml
number: 19262
title: "[ty] Handle configuration errors in LSP more gracefully"
type: pull_request
state: merged
author: MichaReiser
labels:
  - server
  - ty
assignees: []
merged: true
base: main
head: micha/handle-configuration-errors
created_at: 2025-07-10T13:43:59Z
updated_at: 2025-07-14T10:27:54Z
url: https://github.com/astral-sh/ruff/pull/19262
synced_at: 2026-01-12T15:56:35Z
```

# [ty] Handle configuration errors in LSP more gracefully

---

_@MichaReiser_

## Summary

Up to now, ty crashed if the project configuration contained invalid syntax are was invalid otherwise. 

This PR handles this case more gracefully by showing an error popup and falling back to the default settings.
We probably want to mark the database as "in an error state" in the future when the LSP offers more destructive operations
like formatting so that these can be disabled until the configuration errors are resolved. 

Closes https://github.com/astral-sh/ty/issues/76

## Test Plan



https://github.com/user-attachments/assets/87db8aab-752a-4a44-a771-896737cdc13c


---

_Label `server` added by @MichaReiser on 2025-07-10 13:44_

---

_Label `ty` added by @MichaReiser on 2025-07-10 13:44_

---

_Review requested from @carljm by @MichaReiser on 2025-07-10 13:44_

---

_Review requested from @AlexWaygood by @MichaReiser on 2025-07-10 13:44_

---

_Review requested from @sharkdp by @MichaReiser on 2025-07-10 13:44_

---

_Review requested from @dcreager by @MichaReiser on 2025-07-10 13:44_

---

_Label `server` added by @MichaReiser on 2025-07-10 13:44_

---

_Label `ty` added by @MichaReiser on 2025-07-10 13:44_

---

_Comment by @github-actions[bot] on 2025-07-10 13:47_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Comment by @codspeed-hq[bot] on 2025-07-10 13:54_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
<!-- __CODSPEED_INSTRUMENTATION_PERFORMANCE_REPORT_COMMENT__ -->

## [CodSpeed Instrumentation Performance Report](https://codspeed.io/astral-sh/ruff/branches/micha%2Fhandle-configuration-errors?runnerMode=Instrumentation)

### Merging #19262 will **not alter performance**

<sub>Comparing <code>micha/handle-configuration-errors</code> (f2bba75) with <code>main</code> (9002604)</sub>



### Summary

`✅ 40` untouched benchmarks  





---

_Review request for @dcreager removed by @MichaReiser on 2025-07-10 13:55_

---

_Review request for @sharkdp removed by @MichaReiser on 2025-07-10 13:55_

---

_Review request for @AlexWaygood removed by @MichaReiser on 2025-07-10 13:55_

---

_Review requested from @Gankra by @MichaReiser on 2025-07-10 13:55_

---

_Review requested from @dhruvmanila by @MichaReiser on 2025-07-10 13:55_

---

_Review request for @carljm removed by @MichaReiser on 2025-07-10 13:55_

---

_Review comment by @dhruvmanila on `crates/ty_server/src/session.rs`:287 on 2025-07-14 05:50_

I think these should be in the same message. It might be a bit confusing to see the "Falling back ..." message without understanding that it's tied to the previous message.

---

_Comment by @dhruvmanila on 2025-07-14 05:53_

> We probably want to mark the database as "in an error state" in the future when the LSP offers more destructive operations
> like formatting so that these can be disabled until the configuration errors are resolved.

Yeah, I think we need to as this behavior has proven to be confusing to some users: https://github.com/astral-sh/ruff-vscode/issues/711

---

_Review comment by @dhruvmanila on `crates/ty_server/src/session.rs`:293 on 2025-07-14 05:58_

Given that we also have `DefaultProject`, it might be useful to rename this to be something else like `project_db`. Or, we could clone `root` before and inline this logic in `self.projects.insert(..., ...)`:

```rs
self.projects.insert(
	root.clone(),
	ProjectMetadata::from_options(Options::default(), root, None)
	    .context("Failed to convert default options to metadata")
	    .and_then(|metadata| ProjectDatabase::new(metadata, system))
	    .expect("Default configuration to be valid");
);
```

---

_@dhruvmanila approved on 2025-07-14 05:59_

---

_Merged by @MichaReiser on 2025-07-14 10:27_

---

_Closed by @MichaReiser on 2025-07-14 10:27_

---

_Branch deleted on 2025-07-14 10:27_

---
