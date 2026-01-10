```yaml
number: 9549
title: Ignore dynamic version in source dist
type: pull_request
state: merged
author: konstin
labels:
  - enhancement
  - performance
assignees: []
merged: true
base: main
head: konsti/allow-dynamic-version-in-source-dist
created_at: 2024-12-01T09:05:48Z
updated_at: 2024-12-04T11:40:32Z
url: https://github.com/astral-sh/uv/pull/9549
synced_at: 2026-01-10T12:00:00Z
```

# Ignore dynamic version in source dist

---

_Pull request opened by @konstin on 2024-12-01 09:05_

When encountering `dynamic = ["version"]` in the pyproject.toml of a source dist, we can ignore that and treat it as a statically known metadata distribution, since the filename tells us the version and that version must not change on build.

This fixed locking PyGObject 3.50.0 from `pygobject-3.50.0.tar.gz` (minimized):

```toml
[project]
name = "PyGObject"
description = "Python bindings for GObject Introspection"
requires-python = ">=3.9, <4.0"
dependencies = [
    "pycairo>=1.16"
]
dynamic = ["version"]
```

Afterwards, `uv add --no-sync toga` passes on Ubuntu 24.04 without the pygobject build deps, when previously it needed `{ name = "pygobject", version = "3.50.0", requires-dist = [], requires-python = ">=3.9" }`.

I've added a check that source distribution versions are respected after build.

Fixes #9548

---

_@charliermarsh approved on 2024-12-01 12:41_

This makes sense.

---

_Comment by @charliermarsh on 2024-12-01 13:47_

Works on macOS (and fails with released `uv`).

---

_Marked ready for review by @charliermarsh on 2024-12-02 19:34_

---

_Comment by @charliermarsh on 2024-12-02 21:48_

I think the only downside here is that if the built version _doesn't_ match the declared version, we no longer catch it. Today, we'd raise an error. After this PR, we'd assume the declared version is correct.

---

_Comment by @konstin on 2024-12-04 09:39_

At least in `uv build`, we weren't checking version matching at all: https://github.com/astral-sh/uv/pull/9633 (though that PR also only checks filenames, not pyproject.toml and METADATA, too; the filename mismatch is something that the installer wouldn't catch).

---

_Label `enhancement` added by @konstin on 2024-12-04 09:39_

---

_Label `performance` added by @konstin on 2024-12-04 09:39_

---

_Comment by @konstin on 2024-12-04 11:08_

We're also missing the check with static metadata atm, let me see how hard that is.

```rust
#[test]
fn test_version_sdist_wrong_version() -> Result<()> {
    let context = TestContext::new("3.12");

    let pyproject_toml = r#"
    [project]
    name = "foo"
    requires-python = ">=3.9"
    dependencies = []
    version = "10.11.12"
    "#;

    let setup_py = indoc! {r#"
    from setuptools import setup

    setup(name="foo", version="10.11.12")
    "#};

    let source_dist = context.temp_dir.child("foo-1.2.3.tar.gz");
    // Flush and close after finishing.
    {
        let file = File::create(source_dist.path())?;
        let enc = GzEncoder::new(file, flate2::Compression::default());
        let mut tar = tar::Builder::new(enc);

        for (path, contents) in [
            ("foo-1.2.3/pyproject.toml", pyproject_toml),
            ("foo-1.2.3/setup.py", setup_py),
        ] {
            let mut header = tar::Header::new_gnu();
            header.set_size(contents.len() as u64);
            header.set_mode(0o644);
            header.set_cksum();
            tar.append_data(&mut header, path, Cursor::new(contents))?;
        }
        tar.finish()?;
    }

    uv_snapshot!(context.filters(), context
        .pip_install()
        .arg(source_dist.path()), @r###"
    success: true
    exit_code: 0
    ----- stdout -----

    ----- stderr -----
    Resolved 1 package in [TIME]
    Prepared 1 package in [TIME]
    Installed 1 package in [TIME]
     + foo==10.11.12 (from file://[TEMP_DIR]/foo-1.2.3.tar.gz)
    "###
    );

    Ok(())
}
```

---

_Comment by @konstin on 2024-12-04 11:31_

I've added a check and a test

---

_Merged by @konstin on 2024-12-04 11:40_

---

_Closed by @konstin on 2024-12-04 11:40_

---

_Branch deleted on 2024-12-04 11:40_

---
