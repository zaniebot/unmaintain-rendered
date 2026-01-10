```yaml
number: 6616
title: Parses wheels WHEEL and METADATA files content as email messages
type: pull_request
state: merged
author: Coruscant11
labels:
  - bug
  - compatibility
assignees: []
merged: true
base: main
head: fix/wheel-metadata-parsing
created_at: 2024-08-25T19:56:23Z
updated_at: 2024-08-25T22:31:20Z
url: https://github.com/astral-sh/uv/pull/6616
synced_at: 2026-01-10T13:09:51Z
```

# Parses wheels WHEEL and METADATA files content as email messages

---

_Pull request opened by @Coruscant11 on 2024-08-25 19:56_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Fixes: #6615 
Currently, some packages are not installable with `uv`, like `ziglang` on Linux.
Everything is described in the issue! 😄 

<!-- What's the purpose of the change? What does it do, and why? -->

## Test Plan

<!-- How was it tested? -->
I added a unit test for the problematic use case.
I also checked that previous unit test are still running in order to ensure the backward compatibility.


---

_Renamed from "Parses wheel metadata files content as email header" to "Parses wheel WHEEL and METADATA files content as email message" by @Coruscant11 on 2024-08-25 19:56_

---

_Renamed from "Parses wheel WHEEL and METADATA files content as email message" to "Parses wheels WHEEL and METADATA files content as email message" by @Coruscant11 on 2024-08-25 19:56_

---

_Renamed from "Parses wheels WHEEL and METADATA files content as email message" to "Parses wheels WHEEL and METADATA files content as email messages" by @Coruscant11 on 2024-08-25 19:57_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-08-25 20:06_

---

_Comment by @charliermarsh on 2024-08-25 20:08_

Thanks for investigating this. It seems like a reasonable change to me.

---

_Label `bug` added by @charliermarsh on 2024-08-25 20:08_

---

_Comment by @charliermarsh on 2024-08-25 20:10_

Is it possible to use `mailparse` instead? Or change our uses of `mailparse` to use `mail-parser`? We already have this dependency elsewhere.

---

_Comment by @Coruscant11 on 2024-08-25 20:11_

> Is it possible to use `mailparse` instead? Or change our uses of `mailparse` to use `mail-parser`? We already have this dependency elsewhere.

Of course! Let me fix this quickly 😄 
Thanks for the fast review!

---

_Comment by @charliermarsh on 2024-08-25 20:12_

(It might be easiest to use `mailparse` to start, and then consider migrating to `mail-parser` in a separate PR. I honestly can't remember why we use `mailparse` -- I think it's just a smaller dependency -- but I'm open to switching. Take a look at `crates/pypi-types/src/metadata.rs` for an example of how we use it today... I think it's the same problem, roughly?)

---

_Comment by @Coruscant11 on 2024-08-25 20:25_

> (It might be easiest to use `mailparse` to start, and then consider migrating to `mail-parser` in a separate PR. I honestly can't remember why we use `mailparse` -- I think it's just a smaller dependency -- but I'm open to switching. Take a look at `crates/pypi-types/src/metadata.rs` for an example of how we use it today... I think it's the same problem, roughly?)

Alright! I volunteer to look into switching from `mailparse` to `mail-parser` in a future PR if you want!
But I will also look if a change is really needed in a first place, `mailparse` seems much more downloaded 😄
But maybe `mail-parser` is faster, and seems to be heavily tested and benchmarked. 

---

_Comment by @charliermarsh on 2024-08-25 20:35_

It might be fun to run a small benchmark (see, e.g., `crates/bench/benches/distribution_filename.rs`) to compare the two on some representative files :)

---

_Comment by @Coruscant11 on 2024-08-25 20:39_

> It might be fun to run a small benchmark (see, e.g., `crates/bench/benches/distribution_filename.rs`) to compare the two on some representative files :)

Thank for the link! Will use that.
I will also try to benchmark the two crates in different machines, I have x86_64 and arm64 computers, Intel, AMD, M3 Pro... can be fun!

If I could just trust the running time of the unit test inside `install-wheel-rs`, `mail-parser` seems around 0.008 seconds faster 😆 
Will try to check this more in details in a next PR then 😄 

---

_Comment by @charliermarsh on 2024-08-25 20:46_

Thanks, this is much appreciated!

---

_Review comment by @konstin on `crates/install-wheel-rs/Cargo.toml`:36 on 2024-08-25 20:49_

```suggestion
mailparse = { workspace = true }
```

---

_@konstin approved on 2024-08-25 20:49_

---

_@charliermarsh approved on 2024-08-25 20:50_

---

_@charliermarsh reviewed on 2024-08-25 20:51_

---

_Review comment by @charliermarsh on `crates/install-wheel-rs/src/wheel.rs`:819 on 2024-08-25 20:51_

I thought we could maybe avoid these trims now that we're using `mailparse` but looks like whitespace is retained for the keys at least. We could probably avoid _allocating_ here though if there isn't leading / trailing whitespace. Micro-opt for the future.

---

_Label `compatibility` added by @charliermarsh on 2024-08-25 20:51_

---

_@Coruscant11 reviewed on 2024-08-25 20:53_

---

_Review comment by @Coruscant11 on `crates/install-wheel-rs/src/wheel.rs`:819 on 2024-08-25 20:53_

Yes, the name trim can be avoided. I will update this 🙂 
Should we keep it for the values?
I would say it would prevent badly formatted values like we say in the ziglang package

---

_Review comment by @Coruscant11 on `crates/install-wheel-rs/src/wheel.rs`:819 on 2024-08-25 20:54_

By the way, contrary to `mail-parser`, the `get_key()` function is returning a String -> useless `to_string()` below

---

_@Coruscant11 reviewed on 2024-08-25 20:54_

---

_Review comment by @charliermarsh on `crates/install-wheel-rs/src/wheel.rs`:819 on 2024-08-25 20:58_

I think we should keep the trim functionality, but we can maybe avoid the extra `to_string` if there's nothing to trim.

---

_@charliermarsh reviewed on 2024-08-25 20:58_

---

_@charliermarsh reviewed on 2024-08-25 20:59_

---

_Review comment by @charliermarsh on `crates/install-wheel-rs/src/wheel.rs`:819 on 2024-08-25 20:59_

In other words: lets keep `trim` for both. But we _could_ skip `.to_string` if `name == name.trim()`, to avoid the allocation. But maybe more trouble than it's worth, I defer to you :)

---

_@Coruscant11 reviewed on 2024-08-25 20:59_

---

_Review comment by @Coruscant11 on `crates/install-wheel-rs/Cargo.toml`:36 on 2024-08-25 20:59_

Thanks @konstin !

---

_@Coruscant11 reviewed on 2024-08-25 21:07_

---

_Review comment by @Coruscant11 on `crates/install-wheel-rs/src/wheel.rs`:819 on 2024-08-25 21:07_

Updated! Do you think I should extract the trim logic to an other function?

---

_@charliermarsh approved on 2024-08-25 22:30_

---

_Merged by @charliermarsh on 2024-08-25 22:31_

---

_Closed by @charliermarsh on 2024-08-25 22:31_

---

_Comment by @charliermarsh on 2024-08-25 22:31_

Looks good, thanks!

---
