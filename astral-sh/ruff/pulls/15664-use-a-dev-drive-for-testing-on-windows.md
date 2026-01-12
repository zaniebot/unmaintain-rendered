```yaml
number: 15664
title: Use a dev drive for testing on Windows
type: pull_request
state: closed
author: zanieb
labels:
  - ci
assignees: []
draft: true
base: main
head: zb/dev-drive
created_at: 2025-01-21T23:42:03Z
updated_at: 2025-12-10T17:38:44Z
url: https://github.com/astral-sh/ruff/pull/15664
synced_at: 2026-01-12T15:55:52Z
```

# Use a dev drive for testing on Windows

---

_@zanieb_

Investigating the benefits of this here.

Requires Windows 2025, switching over in https://github.com/astral-sh/ruff/pull/15661

---

_Label `internal` added by @zanieb on 2025-01-21 23:42_

---

_Comment by @github-actions[bot] on 2025-01-21 23:54_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Comment by @zanieb on 2025-01-22 00:10_

Hm okay this gives a big speed-up but a test is failing

```
──── STDERR:             red_knot::file_watching directory_deleted
thread 'directory_deleted' panicked at /rustc/9fc6b43126469e3858e2fe86cafb4f0fd5068869\library\core\src\ops\function.rs:250:5:
Didn't observe expected change:
  - Deleted { path: "E:\\ruff-tmp\\.tmppcH4p4\\project\\sub\\a.py", kind: Any }
  - Deleted { path: "E:\\ruff-tmp\\.tmppcH4p4\\project\\sub\\__init__.py", kind: Any }
note: run with `RUST_BACKTRACE=1` environment variable to display a backtrace
```

---

_Comment by @zanieb on 2025-01-22 03:20_

I think the small runner on the `D:\` drive is too slow at ~12 minutes.

@MichaReiser would you be interested in investigating if this is a real bug with Dev Drives? I'm not sure if there are known issues with the file watcher?

I'm happy to procure more logs if you tell me how. Or I can test a minimal snippet, if you can extract the core folder-watching behavior or point me to it?

---

_Label `internal` removed by @zanieb on 2025-01-22 16:03_

---

_Label `ci` added by @zanieb on 2025-01-22 16:03_

---

_@zanieb reviewed on 2025-01-22 16:03_

---

_Review comment by @zanieb on `.github/workflows/setup-dev-drive.ps1`:1 on 2025-01-22 16:03_

Scary, but just copied over from uv.


---

_Comment by @zanieb on 2025-01-22 16:59_

Glorious, here are the logs

```
──── STDERR:             red_knot::file_watching directory_deleted
2025-01-22T16:57:43.337130Z DEBUG red_knot_project::metadata: Searching for a project in 'E:\ruff-tmp\.tmpLgedMu\project'
2025-01-22T16:57:43.337503Z DEBUG red_knot_project::metadata: The ancestor directories contain no `pyproject.toml`. Falling back to a virtual project.
2025-01-22T16:57:43.338307Z  INFO red_knot_python_semantic::program: Python version: Python 3.9, platform: all
2025-01-22T16:57:43.338388Z DEBUG red_knot_python_semantic::module_resolver::resolver: Adding first-party search path 'E:\ruff-tmp\.tmpLgedMu\project'
2025-01-22T16:57:43.338468Z DEBUG red_knot_python_semantic::module_resolver::resolver: Using vendored stdlib
2025-01-22T16:57:43.345608Z TRACE red_knot_project::db: Salsa event: Event { thread_id: ThreadId(2), kind: WillExecute { database_key: dynamic_resolution_paths(Id(800)) } }
2025-01-22T16:57:43.345729Z DEBUG red_knot_python_semantic::module_resolver::resolver: Resolving dynamic module resolution paths
2025-01-22T16:57:43.345984Z DEBUG red_knot_project::watch::watcher: Watching path: `E:\ruff-tmp\.tmpLgedMu\project`
2025-01-22T16:57:43.346325Z  INFO red_knot_project::watch::project_watcher: Set up file watchers for ["E:\ruff-tmp\.tmpLgedMu\project"]
2025-01-22T16:57:43.448406Z TRACE ruff_db::files: Adding file 'E:\ruff-tmp\.tmpLgedMu\project\bar.py'
2025-01-22T16:57:43.449023Z TRACE red_knot_project::db: Salsa event: Event { thread_id: ThreadId(2), kind: WillExecute { database_key: resolve_module_query(Id(1000)) } }
2025-01-22T16:57:43.449297Z TRACE resolve_module: ruff_db::files: Adding file 'E:\ruff-tmp\.tmpLgedMu\project\sub\__init__.py' name=sub.a
2025-01-22T16:57:43.449472Z TRACE resolve_module: ruff_db::files: Adding file 'E:\ruff-tmp\.tmpLgedMu\project\sub\a\__init__.pyi' name=sub.a
2025-01-22T16:57:43.449590Z TRACE resolve_module: ruff_db::files: Adding file 'E:\ruff-tmp\.tmpLgedMu\project\sub\a\__init__.py' name=sub.a
2025-01-22T16:57:43.449696Z TRACE resolve_module: ruff_db::files: Adding file 'E:\ruff-tmp\.tmpLgedMu\project\sub\a.pyi' name=sub.a
2025-01-22T16:57:43.449789Z TRACE resolve_module: ruff_db::files: Adding file 'E:\ruff-tmp\.tmpLgedMu\project\sub\a.py' name=sub.a
2025-01-22T16:57:43.449905Z TRACE resolve_module: red_knot_python_semantic::module_resolver::resolver: Resolved module `sub.a` to `E:\ruff-tmp\.tmpLgedMu\project\sub\a.py` name=sub.a
2025-01-22T16:57:43.455836Z  INFO Project::index_files: red_knot_project: Found 3 files in project `project` package=project
thread 'directory_deleted' panicked at /rustc/9fc6b43126469e3858e2fe86cafb4f0fd5068869\library\core\src\ops\function.rs:250:5:
Didn't observe expected change:
  - Deleted { path: "E:\\ruff-tmp\\.tmpLgedMu\\project\\sub\\a.py", kind: Any }
  - Deleted { path: "E:\\ruff-tmp\\.tmpLgedMu\\project\\sub\\__init__.py", kind: Any }
note: run with `RUST_BACKTRACE=1` environment variable to display a backtrace
```

---

_Comment by @zanieb on 2025-01-22 17:28_

(they don't seem helpful — not sure how to get more...)

---

_Comment by @MichaReiser on 2025-01-24 17:25_

> (they don't seem helpful — not sure how to get more...)

How dare you! I improved them a lot and I think there's some useful content in them. 

The specific test tests if the file watcher correctly handles the case where an entire directory gets deleted, specifically the `sub` directory. That's why the test expects a change event for the `sub` directory. What's interesting is that no such event gets emitted on the devdrive. It only emits the events for the files in that directory

```
Didn't observe expected change:
  - Deleted { path: "E:\\ruff-tmp\\.tmpLgedMu\\project\\sub\\a.py", kind: Any }
  - Deleted { path: "E:\\ruff-tmp\\.tmpLgedMu\\project\\sub\\__init__.py", kind: Any }
```

Now, we could fix the test to expect the events for one of the files instead of the directories, and I suspect it would then pass just fine on all platforms. 

However, this reveals a more fundamental problem with changing to dev drives: Windows's file-watching API returns different events depending on whether the path is a dev drive or not. Migrating to a dev drive would, therefore, remove the coverage for regular Windows drives, which I consider fairly important.

That means I'm not sure if we should change our runner to use dev drivers. Is there a way to only use a dev drive for some directories, e.g., not the directory created by tempdir?





---

_@MichaReiser reviewed on 2025-01-24 17:38_

---

_Review comment by @MichaReiser on `.github/workflows/ci.yaml`:353 on 2025-01-24 17:38_

This should not be needed when you use `testing::setup_tracing()` It always writes the logs and they should be visible if the test fails

---

_@MichaReiser reviewed on 2025-01-24 17:39_

---

_Review comment by @MichaReiser on `.github/workflows/setup-dev-drive.ps1`:46 on 2025-01-24 17:39_

We can probably get away with a smaller drive for Ruff.

---

_@MichaReiser reviewed on 2025-01-24 17:40_

---

_Review comment by @MichaReiser on `.github/workflows/setup-dev-drive.ps1`:41 on 2025-01-24 17:40_

A beautiful mix of space and tab indentation. hehe

---

_Comment by @zanieb on 2025-01-24 17:48_

Funny, I read

```
Didn't observe expected change:
  - Deleted { path: "E:\\ruff-tmp\\.tmpLgedMu\\project\\sub\\a.py", kind: Any }
  - Deleted { path: "E:\\ruff-tmp\\.tmpLgedMu\\project\\sub\\__init__.py", kind: Any }
  ```
  
  as: we expected these changes but did not observe them.
  
  Now I see — the message is much more helpful read that way.

---

_Comment by @zanieb on 2025-01-24 17:50_

> However, this reveals a more fundamental problem with changing to dev drives: Windows's file-watching API returns different events depending on whether the path is a dev drive or not. Migrating to a dev drive would, therefore, remove the coverage for regular Windows drives, which I consider fairly important.

Yeah I agree coverage for a regular drive seems important. It sounds like it'd actually be important to get both coverage for a Dev Drive _and_ the regular file system. I can own that.

> That means I'm not sure if we should change our runner to use dev drivers. Is there a way to only use a dev drive for some directories, e.g., not the directory created by tempdir?

Yeah, but then we'll probably miss out on the benefits. I can explore.

---

_Comment by @MichaReiser on 2025-01-24 17:50_

That's fair. There's definetely still room for improvement. Ideally, the message would also say which event we expected. But it's much better than in the past where it just told you: Too bad, didn't work 😆 

---

_Comment by @zanieb on 2025-01-24 17:50_

Thanks for taking a look Micha. I see this as relatively low priority, but I can explore it in my spare time,

---

_Comment by @MichaReiser on 2025-01-24 17:51_

Thank you. Excluding the temp directory might be fine for Ruff. We don't write as many files as you do in uv. It's only a handful tests that make use of temp (although I don't know if cargo or other tools make heavy use of temp)

---

_Comment by @github-actions[bot] on 2025-06-18 12:19_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
mypy_primer (https://github.com/hauntsaninja/mypy_primer)
-     memo fields = ~41MB
+     memo fields = ~45MB

aioredis (https://github.com/aio-libs/aioredis)
-     memo fields = ~54MB
+     memo fields = ~60MB

attrs (https://github.com/python-attrs/attrs)
- TOTAL MEMORY USAGE: ~72MB
+ TOTAL MEMORY USAGE: ~80MB

Expression (https://github.com/cognitedata/Expression)
-     memo fields = ~49MB
+     memo fields = ~54MB

operator (https://github.com/canonical/operator)
- TOTAL MEMORY USAGE: ~97MB
+ TOTAL MEMORY USAGE: ~106MB
-     memo fields = ~80MB
+     memo fields = ~88MB

hydra-zen (https://github.com/mit-ll-responsible-ai/hydra-zen)
- TOTAL MEMORY USAGE: ~80MB
+ TOTAL MEMORY USAGE: ~88MB

mongo-python-driver (https://github.com/mongodb/mongo-python-driver)
-     memo fields = ~171MB
+     memo fields = ~156MB

paasta (https://github.com/yelp/paasta)
-     memo fields = ~171MB
+     memo fields = ~156MB

```
</details>


---

_@zanieb reviewed on 2025-06-18 16:14_

---

_Review comment by @zanieb on `.github/workflows/setup-dev-drive.ps1`:46 on 2025-06-18 16:14_

I actually tried 5 GB then 10 GB and both ran out of space during compilation.

---

_Closed by @MichaReiser on 2025-06-20 06:30_

---

_Reopened by @MichaReiser on 2025-06-20 06:30_

---

_Comment by @codspeed-hq[bot] on 2025-07-03 12:11_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/zb%2Fdev-drive?utm_source=github&utm_medium=comment&utm_content=header)

### Merging #15664 will **not alter performance**

<sub>Comparing <code>zb/dev-drive</code> (40b4aa2) with <code>main</code> (d0f0577)</sub>



### Summary

`✅ 8` untouched  





---

_Comment by @MichaReiser on 2025-12-10 17:38_

I'll close this as it looks pretty stale but please re-open it if you think we should keep this around.

---

_Closed by @MichaReiser on 2025-12-10 17:38_

---
