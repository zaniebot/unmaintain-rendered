```yaml
number: 4533
title: "Make `.egg-info` filename parsing spec compliant"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
  - compatibility
assignees: []
merged: true
base: main
head: charlie/egg
created_at: 2024-06-25T23:15:18Z
updated_at: 2024-06-25T23:49:44Z
url: https://github.com/astral-sh/uv/pull/4533
synced_at: 2026-01-10T13:48:28Z
```

# Make `.egg-info` filename parsing spec compliant

---

_Pull request opened by @charliermarsh on 2024-06-25 23:15_

## Summary

It turns out that `.egg-info` files and directories can _both_ have up to four segments in the filename: https://setuptools.pypa.io/en/latest/deprecated/python_eggs.html#filename-embedded-metadata. This PR upgrades the parsing and now uses the same parsing for files and directories.

Closes https://github.com/astral-sh/uv/issues/4532.


---

_Label `bug` added by @charliermarsh on 2024-06-25 23:15_

---

_Label `compatibility` added by @charliermarsh on 2024-06-25 23:15_

---

_Comment by @charliermarsh on 2024-06-25 23:15_

\cc @samypr100 

---

_Review comment by @samypr100 on `crates/uv/tests/pip_freeze.rs`:296 on 2024-06-25 23:39_

```suggestion
}

/// Show a set of `.egg-info` files in a virtual environment.
#[test]
fn freeze_with_egg_info_file() -> Result<()> {
    let context = TestContext::new("3.11");

    let site_packages = ChildPath::new(context.site_packages());

    // Manually create a `.egg-info` file with python version.
    site_packages
        .child("pycurl-7.45.1-py3.11.egg-info")
        .write_str(indoc! {"
            Metadata-Version: 1.1
            Name: pycurl
            Version: 7.45.1
        "})?;

    // Manually create another `.egg-info` file with no python version.
    site_packages
        .child("vtk-9.2.6.egg-info")
        .write_str(indoc! {"
            Metadata-Version: 1.1
            Name: vtk
            Version: 9.2.6
        "})?;

    // Run `pip freeze`.
    uv_snapshot!(context.filters(), command(&context), @r###"
    success: true
    exit_code: 0
    ----- stdout -----
    pycurl==7.45.1
    vtk==9.2.6

    ----- stderr -----
    "###);

    Ok(())
}
```

Maybe it's worth adding another test for .egg-info files?

---

_@samypr100 reviewed on 2024-06-25 23:39_

---

_@charliermarsh reviewed on 2024-06-25 23:39_

---

_Review comment by @charliermarsh on `crates/uv/tests/pip_freeze.rs`:296 on 2024-06-25 23:39_

Good idea.

---

_Merged by @charliermarsh on 2024-06-25 23:49_

---

_Closed by @charliermarsh on 2024-06-25 23:49_

---

_Branch deleted on 2024-06-25 23:49_

---
