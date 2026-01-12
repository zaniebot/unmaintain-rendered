```yaml
number: 1852
title: "feat: allow passing extra config k,v pairs for pyvenv.cfg when creating a venv"
type: pull_request
state: merged
author: samypr100
labels:
  - enhancement
  - virtualenv
assignees: []
merged: true
base: main
head: uv-pyvenv-cfg-custom-fields
created_at: 2024-02-22T03:44:34Z
updated_at: 2024-02-23T03:22:52Z
url: https://github.com/astral-sh/uv/pull/1852
synced_at: 2026-01-12T16:04:45Z
```

# feat: allow passing extra config k,v pairs for pyvenv.cfg when creating a venv

---

_@samypr100_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

This modifies `gourgeist` to allow passing additional k,v pairs to add to the `pyvenv.cfg` file as proposed in #1697.
I made it allow an arbitrary set of pairs (to decouple from `uv` since this is mainly a change to `gourgeist`) , but I can slim it down to just allow just a name and version strings if that's desired.

The `pyvenv.cfg` will also have a `uv = <uv-crate-version>` when a venv is created via `uv venv` ~~and `uv-build = <uv-build-crate-version>` when it's created via `SourceBuild::setup`~~.

Example below via `uv venv`:

```ini
home = ...
implementation = CPython
version_info = 3.12
include-system-site-packages = false
base-prefix = ...
base-exec-prefix = ...
base-executable = ...
uv = 0.1.6
prompt = uv
```

Open to any suggestions, thanks!

Closes #1697 

## Test Plan

Added new test in `tests/venv.rs` called `verify_pyvenv_cfg` to verify that it contains the right uv version string. I didn't see tests configured in `gourgeist` itself, so I didn't add any there.

---

_Marked ready for review by @samypr100 on 2024-02-22 04:03_

---

_Review requested from @konstin by @charliermarsh on 2024-02-22 04:10_

---

_Assigned to @konstin by @zanieb on 2024-02-22 04:23_

---

_Label `enhancement` added by @konstin on 2024-02-22 08:35_

---

_Review comment by @konstin on `crates/gourgeist/src/bare.rs`:236 on 2024-02-22 08:39_

I think gourgeist should not be visible when used through uv, it's confusing to the user otherwise what that `gourgeist` thing is

```suggestion
```

---

_Review comment by @konstin on `crates/gourgeist/src/bare.rs`:266 on 2024-02-22 08:39_

I wouldn't should the 

```suggestion
```

---

_Review comment by @konstin on `crates/uv-build/src/lib.rs`:332 on 2024-02-22 08:54_

I'd propose to always pass the uv version in `uv`, and use `uv-build` as a binary flag. If a package wants special behaviour for specific uv versions, it can check always check the `uv` field this way.

---

_Review comment by @konstin on `crates/gourgeist/src/bare.rs`:248 on 2024-02-22 08:57_

This should not be an io error, can you add a variant to `crate::Error`?

---

_Review comment by @konstin on `crates/gourgeist/src/bare.rs`:269 on 2024-02-22 08:57_

I'd defined our own fields first and then `chain()` the extra fields after it

---

_Review comment by @konstin on `crates/gourgeist/src/bare.rs`:264 on 2024-02-22 09:04_

While your at it, would you mind moving `prompt` also to this list?

---

_@konstin approved on 2024-02-22 09:05_

---

_@samypr100 reviewed on 2024-02-22 13:13_

---

_Review comment by @samypr100 on `crates/gourgeist/src/bare.rs`:236 on 2024-02-22 13:13_

Done in 258e4034a7aec9768ce0cee8121fafa3f44bf6b5

---

_@samypr100 reviewed on 2024-02-22 13:13_

---

_Review comment by @samypr100 on `crates/gourgeist/src/bare.rs`:266 on 2024-02-22 13:13_

Done in 258e4034a7aec9768ce0cee8121fafa3f44bf6b5

---

_@samypr100 reviewed on 2024-02-22 13:13_

---

_Review comment by @samypr100 on `crates/gourgeist/src/bare.rs`:269 on 2024-02-22 13:13_

Done in 258e4034a7aec9768ce0cee8121fafa3f44bf6b5

---

_@samypr100 reviewed on 2024-02-22 13:13_

---

_Review comment by @samypr100 on `crates/gourgeist/src/bare.rs`:264 on 2024-02-22 13:13_

Done in 258e4034a7aec9768ce0cee8121fafa3f44bf6b5

---

_@samypr100 reviewed on 2024-02-22 13:15_

---

_Review comment by @samypr100 on `crates/gourgeist/src/bare.rs`:248 on 2024-02-22 13:15_

I could add to crate::Error, but return type expects `io::Result<VenvPaths>` instead of allowing crate::Error. Thoughts? Should the signature change?

---

_@samypr100 reviewed on 2024-02-22 13:27_

---

_Review comment by @samypr100 on `crates/uv-build/src/lib.rs`:332 on 2024-02-22 13:27_

Should I remove this instead to simplify?

---

_@samypr100 reviewed on 2024-02-22 13:30_

---

_Review comment by @samypr100 on `crates/uv-build/src/lib.rs`:332 on 2024-02-22 13:30_

I removed meanwhile in 54f6891a7d99554622c5dfb937c22ee6e9b300bb if you think that's better for now

---

_@konstin reviewed on 2024-02-22 13:31_

---

_Review comment by @konstin on `crates/gourgeist/src/bare.rs`:248 on 2024-02-22 13:31_

Yes, it should change to `Result<VenvPaths, Error>` where `Error` is `crate::Error`. We have per-crate enum error types in most of uv which we use when more then one error can be returned from a function.

---

_@konstin reviewed on 2024-02-22 13:32_

---

_Review comment by @konstin on `crates/gourgeist/src/bare.rs`:264 on 2024-02-22 13:32_

thanks!


---

_Label `virtualenv` added by @zanieb on 2024-02-22 14:50_

---

_@samypr100 reviewed on 2024-02-22 16:16_

---

_Review comment by @samypr100 on `crates/gourgeist/src/bare.rs`:248 on 2024-02-22 16:16_

Done in 64031502338b101f3993e17475c00853a9575562

---

_@konstin reviewed on 2024-02-22 19:39_

---

_Review comment by @konstin on `crates/uv-build/src/lib.rs`:332 on 2024-02-22 19:39_

Either is fine by me

---

_Merged by @konstin on 2024-02-22 19:39_

---

_Closed by @konstin on 2024-02-22 19:39_

---

_Comment by @konstin on 2024-02-22 19:39_

Thanks!

---

_Branch deleted on 2024-02-22 19:53_

---

_Comment by @henryiii on 2024-02-22 21:06_

Shouldn’t `gourgeist` still be there too? I am checking for that in nox (though not merged yet due to the coverage CI being broken). I can check for either one easily enough.

---

_Comment by @konstin on 2024-02-22 21:13_

I think the "API" should be uv with gourgeist being more an implementation detail. What's your use case for checking who created the venv?

---

_Comment by @samypr100 on 2024-02-22 21:44_

@henryiii  Can you look for `uv` starting with `0.1.9` instead? I believe that's what you desired for `nox` based on your issue.

---

_Comment by @henryiii on 2024-02-22 21:45_

I can look for either one. I need to know if we need to regenerate the environment if a user changes the provider. This is especially important for uv vs. venv/virtualenv, since we are installing pip into those environments but not uv.

---

_Comment by @henryiii on 2024-02-22 21:48_

Technically, since it didn't matter before (unless someone switched venv or virtualenv with conda or mamba, which would have been a mess but not something very common), the mechanism is weirdly opt-in via a secret environment variable, but I think I can fix that.

---
