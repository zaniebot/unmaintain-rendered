```yaml
number: 6689
title: "Add `--app` and `--lib` options to `uv init`"
type: pull_request
state: merged
author: zanieb
labels:
  - cli
assignees: []
merged: true
base: main
head: zb/init-app-lib
created_at: 2024-08-27T14:05:13Z
updated_at: 2024-08-27T18:08:11Z
url: https://github.com/astral-sh/uv/pull/6689
synced_at: 2026-01-12T16:07:29Z
```

# Add `--app` and `--lib` options to `uv init`

---

_@zanieb_

Changes the `uv init` experience with a focus on working for more use-cases out of the box.

- Adds `--app` and `--lib` options to control the created project style
- Changes the default from a library with `src/` and a build backend (`--lib`) to an application that is not packaged (`--app`)
- Hides the `--virtual` option and replaces it with `--package` and `--no-package`
- `--no-package` is not allowed with `--lib` right now, but it could be in the future once we understand a use-case
- Creates a runnable project
   - Applications have a `hello.py` file which you can run with `uv run hello.py`
   - Packaged applications, e.g., `uv init --app --package` create a package and script entrypoint, which you can run with `uv run hello`
   - Libraries provide a demo API function, e.g., `uv run python -c "import name; print(name.hello())"` — this is unchanged

Closes #6471 

---

_Label `cli` added by @zanieb on 2024-08-27 14:05_

---

_Renamed from "zb/init app lib" to "Add `--app` and `--lib` options to `uv init`*" by @zanieb on 2024-08-27 14:06_

---

_Renamed from "Add `--app` and `--lib` options to `uv init`*" to "Add `--app` and `--lib` options to `uv init`" by @zanieb on 2024-08-27 14:06_

---

_Comment by @zanieb on 2024-08-27 15:47_

Instead of `hello.py` I explored `app/__main__.py` and `uv run python -m app`. I'm not sure what's better, but we might want to go with the simplest possible option? Thoughts? Seems nice to have some namespacing? Maybe we can do something different depending on if the directory is empty or not?

---

_@charliermarsh approved on 2024-08-27 16:45_

---

_Review comment by @charliermarsh on `crates/uv-cli/src/lib.rs`:2117 on 2024-08-27 16:45_

`overrides_with`?

---

_@charliermarsh reviewed on 2024-08-27 16:45_

---

_@charliermarsh reviewed on 2024-08-27 16:48_

---

_Review comment by @charliermarsh on `crates/uv-cli/src/lib.rs`:2108 on 2024-08-27 16:48_

"to be built as a Python package" maybe? Lets mention that the package will include a build system?

---

_@zanieb reviewed on 2024-08-27 16:48_

---

_Review comment by @zanieb on `crates/uv-cli/src/lib.rs`:2117 on 2024-08-27 16:48_

```suggestion
    #[arg(long, overrides_with = "package", conflicts_with = "lib")]
```

---

_@zanieb reviewed on 2024-08-27 16:48_

---

_Review comment by @zanieb on `crates/uv-cli/src/lib.rs`:2117 on 2024-08-27 16:48_

Makes sense to me, I think. That's our standard pattern right?

---

_Marked ready for review by @zanieb on 2024-08-27 17:37_

---

_Merged by @zanieb on 2024-08-27 18:08_

---

_Closed by @zanieb on 2024-08-27 18:08_

---

_Branch deleted on 2024-08-27 18:08_

---
