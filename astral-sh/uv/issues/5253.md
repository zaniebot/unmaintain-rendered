```yaml
number: 5253
title: "`tree` does not properly handle multiple versions of the same package"
type: issue
state: closed
author: charliermarsh
labels:
  - bug
  - preview
assignees: []
created_at: 2024-07-20T18:27:53Z
updated_at: 2024-08-04T18:33:14Z
url: https://github.com/astral-sh/uv/issues/5253
synced_at: 2026-01-10T04:53:49Z
```

# `tree` does not properly handle multiple versions of the same package

---

_Issue opened by @charliermarsh on 2024-07-20 18:27_

This is possible in `uv tree` (but not `pip tree` typically), since the lockfile can contain multiple versions. Notice the strange result here:

```rust
#[test]
fn multi_version() -> Result<()> {
    let context = TestContext::new("3.12");

    let pyproject_toml = context.temp_dir.child("pyproject.toml");
    pyproject_toml.write_str(
        r#"
        [project]
        name = "project"
        version = "0.1.0"
        # ...
        requires-python = ">=3.12"
        dependencies = [
            "anyio==3.3.0 ; sys_platform == 'win32'",
            "anyio==2.0.0 ; sys_platform != 'win32'",
        ]
    "#,
    )?;

    uv_snapshot!(context.filters(), context.tree(), @r###"
    success: true
    exit_code: 0
    ----- stdout -----
    project v0.1.0
    └── anyio v2.0.0
        ├── idna v3.6
        ├── sniffio v1.3.1
        ├── idna v3.6
        └── sniffio v1.3.1
    └── anyio v3.3.0 (*)
    (*) Package tree already displayed

    ----- stderr -----
    warning: `uv tree` is experimental and may change without warning
    Resolved 5 packages in [TIME]
    "###
    );

    Ok(())
}
```

---

_Label `bug` added by @charliermarsh on 2024-07-20 18:27_

---

_Label `preview` added by @charliermarsh on 2024-07-20 18:27_

---

_Comment by @charliermarsh on 2024-07-20 18:28_

Can we make all the hash maps there keyed on (name, version) instead of just name?

---

_Label `help wanted` added by @charliermarsh on 2024-07-20 18:49_

---

_Label `good first issue` added by @charliermarsh on 2024-07-21 23:16_

---

_Comment by @ChannyClaus on 2024-07-23 02:07_

so is the expected output for the above `pyproject.toml` the below?
```
 project v0.1.0
 ├─── anyio v3.3.0
 │   ├── idna v3.6
 │   └── sniffio v1.3.1
 └── anyio v2.0.0
     ├── idna v3.6
     └── sniffio v1.3.1
```
(relatedly, is there a design doc for `uv`'s higher level entrypoints? i think i have a rough idea around the motivation for it but not quite enough to immediately see what should be the desired outcome here)

---

_Comment by @charliermarsh on 2024-07-23 02:21_

@ChannyClaus -- Yeah, that looks right to me. I don't know that we have a design doc on GitHub, but we're starting to build up the documentation here -- it's still a work-in-progress: https://docs.astral.sh/uv/

---

_Comment by @eth3lbert on 2024-07-23 16:28_

How about displaying only the packages available on the current platform?

---

_Comment by @zanieb on 2024-07-23 16:31_

It seems useful to inspect the lockfile tree as a whole though

---

_Comment by @charliermarsh on 2024-07-23 16:47_

@eth3lbert -- That could be useful but I think it would be a separate flag.

---

_Comment by @eth3lbert on 2024-07-23 16:50_

Fair enough! `cargo tree` also has a `--target` option. By default is the host platform, but can also be set to "all" or other specific values.

---

_Comment by @charliermarsh on 2024-07-28 19:03_

(We could accept the `--python-platform` and `--python-version` flags here.)

---

_Comment by @charliermarsh on 2024-07-29 00:54_

It's actually even harder than this, because we can have a single package at a single version but multiple URLs (https://github.com/astral-sh/uv/issues/5294).

---

_Label `good first issue` removed by @charliermarsh on 2024-07-30 15:44_

---

_Label `help wanted` removed by @charliermarsh on 2024-07-30 15:44_

---

_Assigned to @ibraheemdev by @charliermarsh on 2024-07-30 15:47_

---

_Unassigned @ibraheemdev by @charliermarsh on 2024-08-04 18:15_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-08-04 18:15_

---

_Closed by @charliermarsh on 2024-08-04 18:33_

---

_Closed by @charliermarsh on 2024-08-04 18:33_

---
