```yaml
number: 4655
title: "implements `--package` for `pip tree`"
type: pull_request
state: merged
author: ChannyClaus
labels: []
assignees: []
merged: true
base: main
head: pip-tree-package
created_at: 2024-06-29T23:33:34Z
updated_at: 2024-07-01T21:13:00Z
url: https://github.com/astral-sh/uv/pull/4655
synced_at: 2026-01-12T16:06:22Z
```

# implements `--package` for `pip tree`

---

_@ChannyClaus_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary
partially resolves https://github.com/astral-sh/uv/issues/4439

<!-- What's the purpose of the change? What does it do, and why? -->

## Test Plan
the existing tests pass + added a couple of tests to ensure `--package` behaves as expected.
<!-- How was it tested? -->


---

_@ibraheemdev reviewed on 2024-07-01 16:48_

---

_Review comment by @ibraheemdev on `crates/uv/src/commands/pip/tree.rs`:283 on 2024-07-01 16:48_

Why are we adding newlines? We don't do this in the regular output.

---

_@ChannyClaus reviewed on 2024-07-01 18:10_

---

_Review comment by @ChannyClaus on `crates/uv/src/commands/pip/tree.rs`:283 on 2024-07-01 18:10_

`cargo tree` seems to do this with `-p`, presumably so that it's clear each output block corresponds to a different `--package` value.

```
$ cargo tree -p futures-util -p tracing
futures-util v0.3.30
├── futures-core v0.3.30
├── futures-task v0.3.30
├── pin-project-lite v0.2.14
└── pin-utils v0.1.0

tracing v0.1.40
├── log v0.4.21
├── pin-project-lite v0.2.14
└── tracing-core v0.1.32
    └── once_cell v1.19.0
```

happy to revert this, i just thought it was helpful to look at with an empty line in-between.

---

_@zanieb reviewed on 2024-07-01 18:43_

---

_Review comment by @zanieb on `crates/uv/src/commands/pip/tree.rs`:283 on 2024-07-01 18:43_

This seems sensible to me.

---

_Comment by @ibraheemdev on 2024-07-01 19:23_

Looks like your merge got messed up somewhere and you ended up overwriting some changes. Because this is a small PR I would suggest reapplying your changes to `pip/tree.rs` manually with the latest changes.

---

_@ibraheemdev reviewed on 2024-07-01 20:33_

---

_Review comment by @ibraheemdev on `crates/uv/src/commands/pip/tree.rs`:119 on 2024-07-01 20:33_

You can use `SitePackages::get_packages` instead of recreating that map here. See https://github.com/astral-sh/uv/pull/4702/files#diff-1dce93dfbaf1f210c15966453b85cf8268055cf502820abf10915fea1a7c2f45.

---

_Review comment by @ibraheemdev on `crates/uv/src/commands/pip/tree.rs`:292 on 2024-07-01 20:35_

```suggestion
                for installed_dist in self.site_packages.get_packages(&package) {
                    path.clear();
                    lines.extend(self.visit(installed_dist, &mut visited, &mut path)?);
                }
```

---

_@ibraheemdev reviewed on 2024-07-01 20:35_

---

_Review comment by @ibraheemdev on `crates/uv/src/commands/pip/tree.rs`:119 on 2024-07-01 20:53_

```suggestion
```

---

_Review comment by @ibraheemdev on `crates/uv/src/commands/pip/tree.rs`:290 on 2024-07-01 20:53_

```suggestion
                for installed_dist in self.site_packages.get_packages(package) {
```

---

_@ibraheemdev reviewed on 2024-07-01 20:53_

CI should pass after this.

---

_@ChannyClaus reviewed on 2024-07-01 20:53_

---

_Review comment by @ChannyClaus on `crates/uv/src/commands/pip/tree.rs`:119 on 2024-07-01 20:53_

hmm, is there a reason that `get_packages` returns a vector? specifically, are there times when we expect a package name to map to multiple packages? (i thought there can never be two versions of the same package, or at least that's not supposed to happen)

---

_Review comment by @ibraheemdev on `crates/uv/src/commands/pip/tree.rs`:119 on 2024-07-01 21:02_

It is technically possible, though not expected in any real word usage. See: https://github.com/jaraco/pip-run?tab=readme-ov-file#experiments-and-testing.

---

_@ibraheemdev reviewed on 2024-07-01 21:02_

---

_Comment by @ibraheemdev on 2024-07-01 21:05_

Thanks!

---

_Merged by @ibraheemdev on 2024-07-01 21:12_

---

_Closed by @ibraheemdev on 2024-07-01 21:13_

---
