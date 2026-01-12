```yaml
number: 791
title: Add assertions of expected scenario results
type: pull_request
state: merged
author: zanieb
labels: []
assignees: []
merged: true
base: main
head: zb/expected-assert
created_at: 2024-01-04T22:15:30Z
updated_at: 2024-01-05T16:32:38Z
url: https://github.com/astral-sh/uv/pull/791
synced_at: 2026-01-12T16:04:12Z
```

# Add assertions of expected scenario results

---

_@zanieb_

Uses new metadata added in https://github.com/zanieb/packse/pull/61 to assert that resolution succeeded or failed _and_ that the installed package versions match the expected result.

---

_@zanieb reviewed on 2024-01-05 03:29_

---

_Review comment by @zanieb on `crates/puffin-cli/tests/pip_install_scenarios.rs`:215 on 2024-01-05 03:29_

This was a minor whoopsie from #790 which required new template logic for the global `--prerelease` flag.

---

_@zanieb reviewed on 2024-01-05 03:29_

---

_Review comment by @zanieb on `scripts/scenarios/template.mustache`:111 on 2024-01-05 03:29_

If not satisfiable, assert each requested package is not installed.

---

_Review comment by @zanieb on `scripts/scenarios/update.py`:164 on 2024-01-05 03:31_

This is a little silly feeling, but seemingly the cleanest way to render it in mustache.

I guess we might want to improve data model over in `packse inspect` eventually so it fulfills these requirements.

---

_@zanieb reviewed on 2024-01-05 03:31_

---

_Review comment by @konstin on `scripts/scenarios/template.mustache`:33 on 2024-01-05 13:25_

nit: We don't need the stronger bound here

```suggestion
    package: &str,
    version: &str,
```

---

_@konstin approved on 2024-01-05 13:27_

---

_Review comment by @zanieb on `scripts/scenarios/template.mustache`:33 on 2024-01-05 15:43_

We do! Probably my naivety but 

```
error[E0521]: borrowed data escapes outside of function
  --> crates/puffin-cli/tests/pip_install_scenarios.rs:29:5
   |
28 |   fn assert_installed(venv: &Path, package: &str, version: &str, temp_dir: &Path) {
   |                                                   -------  - let's call the lifetime of this reference `'1`
   |                                                   |
   |                                                   `version` is a reference that is only valid in the function body
29 | /     assert_command(
30 | |         venv,
31 | |         format!("import {package} as package; print(package.__version__, end='')").as_str(),
32 | |         temp_dir,
33 | |     )
34 | |     .success()
35 | |     .stdout(version);
   | |                    ^
   | |                    |
   | |____________________`version` escapes the function body here
   |                      argument requires that `'1` must outlive `'static`

For more information about this error, try `rustc --explain E0521`.
``` 

---

_@zanieb reviewed on 2024-01-05 15:43_

---

_Merged by @zanieb on 2024-01-05 16:32_

---

_Closed by @zanieb on 2024-01-05 16:32_

---

_Branch deleted on 2024-01-05 16:32_

---
