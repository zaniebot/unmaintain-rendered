```yaml
number: 13618
title: "add `excluded-dependencies` support"
type: pull_request
state: closed
author: CodeMan62
labels: []
assignees: []
draft: true
base: main
head: excludes-command-add
created_at: 2025-05-23T14:31:31Z
updated_at: 2025-06-18T18:47:04Z
url: https://github.com/astral-sh/uv/pull/13618
synced_at: 2026-01-10T11:10:42Z
```

# add `excluded-dependencies` support

---

_Pull request opened by @CodeMan62 on 2025-05-23 14:31_

## Summary
Fixes #12616 
adds support for `excluded-dependencies` 

## Test Plan
a unit test will be added 


---

_Converted to draft by @CodeMan62 on 2025-05-23 14:31_

---

_Marked ready for review by @CodeMan62 on 2025-05-26 16:46_

---

_Comment by @konstin on 2025-05-26 19:50_

For this feature, we need an integration test (under `creates/uv/tests`), and we need to ensure that commands reading the venv (such as `uv sync --check` and `uv pip list`) and commands displaying dependencies (such as `uv tree`) work correctly and without warnings when dependencies are excluded, both for leaf dependencies and for dependencies with other dependencies.

---

_Comment by @CodeMan62 on 2025-05-26 20:12_

> For this feature, we need an integration test (under `creates/uv/tests`), and we need to ensure that commands reading the venv (such as `uv sync --check` and `uv pip list`) and commands displaying dependencies (such as `uv tree`) work correctly and without warnings when dependencies are excluded, both for leaf dependencies and for dependencies with other dependencies.

i am adding tests and will do remaining stuff

---

_Assigned to @konstin by @zanieb on 2025-05-27 18:28_

---

_Converted to draft by @CodeMan62 on 2025-06-01 05:03_

---

_Comment by @codspeed-hq[bot] on 2025-06-01 05:18_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
<!-- __CODSPEED_WALLTIME_PERFORMANCE_REPORT_COMMENT__ -->

## [CodSpeed Walltime Performance Report](https://codspeed.io/astral-sh/uv/branches/CodeMan62%3Aexcludes-command-add)

### Merging #13618 will **not alter performance**

<sub>Comparing <code>CodeMan62:excludes-command-add</code> (c8991ea) with <code>main</code> (357fc91)</sub>



### Summary

`✅ 12` untouched benchmarks  





---

_Marked ready for review by @CodeMan62 on 2025-06-01 08:45_

---

_Comment by @CodeMan62 on 2025-06-01 08:45_

@konstin everything is done as you told let me know if anything still lefts 

---

_Review comment by @konstin on `crates/uv-cli/src/lib.rs`:1135 on 2025-06-02 08:21_

The CLI args needs descriptions like the other CLI args have.

---

_Review comment by @konstin on `crates/uv-requirements/src/specification.rs`:63 on 2025-06-02 08:21_

type

---

_Review comment by @konstin on `crates/uv-static/src/env_vars.rs`:113 on 2025-06-02 08:22_

This needs a docstring.

---

_Review comment by @konstin on `crates/uv-tool/src/tool.rs`:273 on 2025-06-02 08:23_

Why are you using `r#excludes` or `excludes`?

---

_Review comment by @konstin on `crates/uv-workspace/src/pyproject.rs`:434 on 2025-06-02 08:24_

Should excludes by using the full PEP 508 syntax, or only package names? What are the pros and cons of having just the name vs. full requirements?

---

_Review comment by @konstin on `crates/uv/src/commands/pip/compile.rs`:244 on 2025-06-02 08:24_

Why does this use an underscore?

---

_Review comment by @konstin on `crates/uv/src/commands/project/lock_target.rs`:74 on 2025-06-02 08:25_

That's not excludes

---

_Review comment by @konstin on `crates/uv/src/commands/pip/operations.rs`:112 on 2025-06-02 08:27_

why does this use an underscore?

---

_Review comment by @konstin on `crates/uv/src/lib.rs`:466 on 2025-06-02 08:28_

These are not overrides

---

_Review comment by @konstin on `crates/uv/src/commands/tool/install.rs`:310 on 2025-06-02 08:29_

We should not allow unnamed excludes

---

_Review comment by @konstin on `crates/uv-requirements/src/specification.rs`:285 on 2025-06-02 08:30_

These are not groups

---

_Review comment by @konstin on `crates/uv/tests/it/lock.rs`:1774 on 2025-06-02 08:32_

The test is failing for me:

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ Snapshot Summary ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Snapshot: lock_project_with_excluded_dependencies
Source: crates/uv/tests/it/lock.rs:1791
────────────────────────────────────────────────────────────────────────────────
Expression: snapshot
────────────────────────────────────────────────────────────────────────────────
-old snapshot
+new results
────────────┬───────────────────────────────────────────────────────────────────
    1     1 │ exit_code: 0
    2     2 │ ----- stdout -----
    3     3 │ 
    4     4 │ ----- stderr -----
    5       │-Resolved 5 packages in [TIME]
          5 │+warning: Failed to parse `pyproject.toml` during settings discovery:
          6 │+  TOML parse error at line 9, column 9
          7 │+    |
          8 │+  9 |         excluded-dependencies = ["pytest"]
          9 │+    |         ^^^^^^^^^^^^^^^^^^^^^
         10 │+  unknown field `excluded-dependencies`, expected one of `required-version`, `native-tls`, `offline`, `no-cache`, `cache-dir`, `preview`, `python-preference`, `python-downloads`, `concurrent-downloads`, `concurrent-builds`, `concurrent-installs`, `index`, `index-url`, `extra-index-url`, `no-index`, `find-links`, `index-strategy`, `keyring-provider`, `allow-insecure-host`, `resolution`, `prerelease`, `fork-strategy`, `dependency-metadata`, `config-settings`, `no-build-isolation`, `no-build-isolation-package`, `exclude-newer`, `link-mode`, `compile-bytecode`, `no-sources`, `upgrade`, `upgrade-package`, `reinstall`, `reinstall-package`, `no-build`, `no-build-package`, `no-binary`, `no-binary-package`, `python-install-mirror`, `pypy-install-mirror`, `python-downloads-json-url`, `publish-url`, `trusted-publishing`, `check-url`, `add-bounds`, `pip`, `cache-keys`, `override-dependencies`, `exclude-dependencies`, `constraint-dependencies`, `build-constraint-dependencies`, `environments`, `required-environments`, `conflicts`, `workspace`, `sources`, `managed`, `package`, `default-groups`, `dev-dependencies`, `build-backend`
         11 │+
         12 │+Resolved 11 packages in [TIME]
────────────┴───────────────────────────────────────────────────────────────────
```

---

_@konstin reviewed on 2025-06-02 08:33_

Please ensure that you are using the correct names for variable and in strings, that new items follow the existing docstring conventions and that the tests pass.

---

_Review comment by @zanieb on `crates/uv-workspace/src/pyproject.rs`:434 on 2025-06-02 15:56_

I feel like we definitely only want the package name?

---

_@zanieb reviewed on 2025-06-02 15:57_

---

_@CodeMan62 reviewed on 2025-06-02 16:17_

---

_Review comment by @CodeMan62 on `crates/uv-workspace/src/pyproject.rs`:434 on 2025-06-02 16:17_

yes we want the package name i will change it here i am reviewing all the reviews once all will be done i will make a final commit 

---

_Converted to draft by @zanieb on 2025-06-02 18:29_

---

_@CodeMan62 reviewed on 2025-06-06 03:41_

---

_Review comment by @CodeMan62 on `crates/uv-cli/src/lib.rs`:1135 on 2025-06-06 03:41_

Done

---

_@CodeMan62 reviewed on 2025-06-06 03:44_

---

_Review comment by @CodeMan62 on `crates/uv-requirements/src/specification.rs`:285 on 2025-06-06 03:44_

my mistake but done now 

---

_@CodeMan62 reviewed on 2025-06-06 03:49_

---

_Review comment by @CodeMan62 on `crates/uv-tool/src/tool.rs`:273 on 2025-06-06 03:49_

because the same things is used in overrides and others ??

---

_Comment by @CodeMan62 on 2025-06-11 17:28_

I want to say that only something left when the work will be completely done by my end i will request review from team 

---

_Comment by @CodeMan62 on 2025-06-15 17:49_

everything is done by my end but I can't load these conflicts locally can help in that @konstin 

---

_Comment by @konstin on 2025-06-16 10:18_

To solve them, you need to update your local main branch, then either merge main into your feature branch, or rebase your branch on the main branch. This is unfortunately a complex feature and hard to get right, and it touches a lot of the codebase, so merge conflicts happen easily.

---

_Comment by @CodeMan62 on 2025-06-16 17:02_

Thanks @konstin for this guide i will do it asap and then we will be good to go sorry for late 

---

_Comment by @zanieb on 2025-06-16 17:06_

How did the scope of this increase from a `excluded-dependencies` field of package names in the `pyproject.toml` to `--excludes` files and environment variables? Was that discussed somewhere I missed?

---

_Comment by @CodeMan62 on 2025-06-16 17:14_

NO this was not discussed but is this not worth having it @zanieb ??

---

_Comment by @zanieb on 2025-06-16 17:18_

It's not clear to me yet. There aren't obvious use-cases for doing this from files like there are for constraints. I don't think we should increase the scope of the API without clear justification and prior discussion. 

---

_Comment by @CodeMan62 on 2025-06-16 17:20_

i think this make sense. now please make it clear what we want here to be implemented 

---

_Comment by @CodeMan62 on 2025-06-18 18:47_

Hey @konstin  as @zanieb  we only want a `excluded-dependencies` field of package names in the `pyproject.toml` but this PR have alot more things so i have created another PR please review that one closing this one also one things i wanna say is incase you guys wants to `--excludes` files and environment variables please create a issue and tag me i will implement that thanks 
new PR #14135 

---

_Closed by @CodeMan62 on 2025-06-18 18:47_

---
