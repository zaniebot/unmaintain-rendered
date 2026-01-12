```yaml
number: 1028
title: Add support for PyPy wheels
type: pull_request
state: merged
author: charliermarsh
labels:
  - enhancement
assignees: []
merged: true
base: main
head: charlie/pypy
created_at: 2024-01-20T22:29:47Z
updated_at: 2024-01-22T14:22:28Z
url: https://github.com/astral-sh/uv/pull/1028
synced_at: 2026-01-12T16:04:22Z
```

# Add support for PyPy wheels

---

_@charliermarsh_

## Summary

This PR adds support for PyPy wheels by changing the compatible tags based on the implementation name and version of the current interpreter.

For now, we only support CPython and PyPy, and explicitly error out when given other interpreters. (Is this right? Should we just fallback to CPython tags...? Or skip the ABI-specific tags for unknown interpreters?) 

The logic is based on https://github.com/pypa/packaging/blob/4d8534061364e3cbfee582192ab81a095ec2db51/src/packaging/tags.py#L247. Note, however, that `packaging` uses the `EXT_SUFFIX` variable from `sysconfig`... Instead, I looked at the way that PyPy formats the tags, and recreated them based on the Python and implementation version. For example, PyPy wheels look like `cchardet-2.1.7-pp37-pypy37_pp73-win_amd64.whl` -- so that's `pp37` for PyPy with Python version 3.7, and then `pypy37_pp73` for PyPy with Python version 3.7 and PyPy version 7.3.

Closes https://github.com/astral-sh/puffin/issues/1013.

## Test Plan

I tested this manually, but I couldn't find macOS universal PyPy wheels... So instead I added `cchardet` to a `requirements.in`, ran `cargo run pip sync requirements.in --index-url https://pypy.kmtea.eu/simple --verbose`, and added logging to verify that the platform tags matched (even if the architecture didn't).


---

_Review requested from @konstin by @charliermarsh on 2024-01-20 22:29_

---

_Label `enhancement` added by @charliermarsh on 2024-01-20 22:29_

---

_@charliermarsh reviewed on 2024-01-20 22:31_

---

_Review comment by @charliermarsh on `crates/platform-tags/src/lib.rs`:257 on 2024-01-20 22:31_

@konstin - Not sure if you have an opinion on this. Should I just skip all the tags that rely on calling `abi_tag`? So just include 3, 4, and 5 in `from_env`, and skip the ones that rely on the ABI and language version?

---

_@charliermarsh reviewed on 2024-01-20 22:31_

---

_Review comment by @charliermarsh on `crates/platform-tags/src/lib.rs`:257 on 2024-01-20 22:31_

I guess we need to read `EXT_SUFFIX` to _exactly_ mimic `packaging`. We could add that to our environment markers...? But not required for this PR.

---

_Review comment by @konstin on `crates/platform-tags/src/lib.rs`:257 on 2024-01-22 08:48_

wdym by "calling abi_tag"?

---

_Review comment by @konstin on `crates/puffin-interpreter/src/interpreter.rs`:202 on 2024-01-22 08:57_

For pypy the implementation version is different from the python version. E.g. i have pypy 7.3.1 installed which implements python 3.10. The current implementation says the tag is `pypy310_pp310`, while it's actually `pypy310_pp73`. It's the version we track in the markers' `implementation_version`.

---

_@konstin reviewed on 2024-01-22 08:58_

---

_Review comment by @charliermarsh on `crates/puffin-interpreter/src/interpreter.rs`:202 on 2024-01-22 13:39_

Sorry, I know this and thought I did it correctly (?), it’s just a typo.

---

_@charliermarsh reviewed on 2024-01-22 13:39_

---

_Review requested from @konstin by @charliermarsh on 2024-01-22 14:15_

---

_@charliermarsh reviewed on 2024-01-22 14:16_

---

_Review comment by @charliermarsh on `crates/platform-tags/src/lib.rs`:257 on 2024-01-22 14:16_

Above, we have cases like:

```rust
// 1. This exact c api version
for platform_tag in &platform_tags {
    tags.push((
        implementation.language_tag(python_version),
        implementation.abi_tag(python_version, implementation_version),
        platform_tag.clone(),
    ));
    tags.push((
        implementation.language_tag(python_version),
        "none".to_string(),
        platform_tag.clone(),
    ));
}
// 2. abi3 and no abi (e.g. executable binary)
if matches!(implementation, Implementation::CPython) {
    // For some reason 3.2 is the minimum python for the cp abi
    for minor in 2..=python_version.1 {
        for platform_tag in &platform_tags {
            tags.push((
                implementation.language_tag((python_version.0, minor)),
                "abi3".to_string(),
                platform_tag.clone(),
            ));
        }
    }
}
// 3. no abi (e.g. executable binary)
for minor in 0..=python_version.1 {
    for platform_tag in &platform_tags {
        tags.push((
            format!("py{}{}", python_version.0, minor),
            "none".to_string(),
            platform_tag.clone(),
        ));
    }
}
// 4. major only
...
```

If we don't know how to create ABI tags (e.g., you're using ironpython), we could skip (1), (2), and (3), but allow (4) and (5) (the no-ABI tags). Right now, we error out saying we don't support them.


---

_@konstin reviewed on 2024-01-22 14:19_

---

_Review comment by @konstin on `crates/platform-tags/src/lib.rs`:257 on 2024-01-22 14:19_

I'm for continuing to error out, otherwise it gets confusing when it seems to work but then doesn't pick up any binary wheels.

---

_@konstin approved on 2024-01-22 14:20_

---

_Merged by @charliermarsh on 2024-01-22 14:22_

---

_Closed by @charliermarsh on 2024-01-22 14:22_

---

_Branch deleted on 2024-01-22 14:22_

---
