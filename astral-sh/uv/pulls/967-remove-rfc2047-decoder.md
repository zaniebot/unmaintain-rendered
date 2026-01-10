```yaml
number: 967
title: Remove RFC2047 decoder
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/metadata
created_at: 2024-01-18T19:16:16Z
updated_at: 2024-01-18T20:09:47Z
url: https://github.com/astral-sh/uv/pull/967
synced_at: 2026-01-10T15:39:03Z
```

# Remove RFC2047 decoder

---

_Pull request opened by @charliermarsh on 2024-01-18 19:16_

## Summary

- This was inherited from https://github.com/PyO3/python-pkginfo-rs/blob/d719988323a0cfea86d4737116d7917f30e819e2/src/metadata.rs#LL78C2-L91C26
- ...which introduced this code here: https://github.com/PyO3/python-pkginfo-rs/commit/9cd1d43f7c2306695f53375f50717b4f7fc0b63c
- ...with the originating issue here: https://github.com/PyO3/maturin/issues/612
- ...and the upstream issue here: https://github.com/staktrace/mailparse/issues/50

It seems like the goal was to support Unicode in certain header fields, but I don't think this is necessary for us. We only use `get_first_value` for `Requires-Python`, which has to be ASCII, doesn't it?

In my testing, it seems like the `charset` hack can also be removed. The tests I copied over actually work without it, which makes me a bit skeptical.

The main benefit here is that we get to a remove a _big_ dependency stack, including Chumsky and Stacker and psm which have limited cross-platform support.


---

_Review requested from @BurntSushi by @charliermarsh on 2024-01-18 19:16_

---

_Review requested from @konstin by @charliermarsh on 2024-01-18 19:16_

---

_Label `internal` added by @charliermarsh on 2024-01-18 19:16_

---

_@charliermarsh reviewed on 2024-01-18 19:16_

---

_Review comment by @charliermarsh on `crates/pypi-types/src/metadata.rs`:78 on 2024-01-18 19:16_

This piece I am slightly less confident in...

---

_@charliermarsh reviewed on 2024-01-18 19:21_

---

_Review comment by @charliermarsh on `crates/pypi-types/src/metadata.rs`:78 on 2024-01-18 19:21_

Although this change works fine when resolving `defity==0.1.3`, which was the originating issue for PyO3.

---

_@charliermarsh reviewed on 2024-01-18 19:24_

---

_Review comment by @charliermarsh on `crates/pypi-types/src/metadata.rs`:78 on 2024-01-18 19:24_

Even the author name is resolved correctly.

---

_Review comment by @BurntSushi on `crates/pypi-types/src/metadata.rs`:175 on 2024-01-18 19:27_

What about a test with a non-ASCII package name?

---

_Review comment by @BurntSushi on `crates/pypi-types/src/metadata.rs`:175 on 2024-01-18 19:32_

It looks like maybe RFC 2047 is for reading UTF-8 encoded data in a format like `=E2=82=AC`? So maybe a test needs to include that?

---

_Review comment by @BurntSushi on `crates/pypi-types/src/metadata.rs`:78 on 2024-01-18 19:33_

Hmm it looks like this was lifted from PyO3? https://github.com/PyO3/python-pkginfo-rs/blob/d719988323a0cfea86d4737116d7917f30e819e2/src/metadata.rs#L89

---

_@BurntSushi reviewed on 2024-01-18 19:38_

It looks like it is theory possible to remove `stacker`, but [`rfc2047-decoder` does not re-export `chumsky` features](https://github.com/TornaxO7/rfc2047-decoder/blob/e0ea2817e939047af36edafa6baf37ec12aa4618/Cargo.toml#L21) and [`chumsky` enables the `stacker` dependency by default](https://github.com/zesterer/chumsky/blob/8b8cf0a04b157df30799d4f385ddedc1dca85014/Cargo.toml#L18). So to get `stacker` out while keeping `rfc2047-decoder` in, you'd need to patch `rfc2047-decoder` to re-export the `spill-stack` feature from `chumsky`.

> It seems like the goal was to support Unicode in certain header fields, but I don't think this is necessary for us. We only use get_first_value for Requires-Python, which has to be ASCII, doesn't it?

This part confuses me. `get_first_value` is used for several things. And indeed, without RFC 2047 support, it seems like it might get stuff wrong? But I'm not an RFC 2047 expert. Does it only apply to the _body_? Or also to the header values? If it's the former, then assuming you don't care about the body here, you should be fine without RFC 2047 decoding. But if things like `=E2=82=AC` can appear in header values, then you probably need RFC 2047 decoding.

My intuition here is that 

---

_@charliermarsh reviewed on 2024-01-18 19:51_

---

_Review comment by @charliermarsh on `crates/pypi-types/src/metadata.rs`:78 on 2024-01-18 19:51_

It's linked in the PR summary :)

---

_@charliermarsh reviewed on 2024-01-18 19:59_

---

_Review comment by @charliermarsh on `crates/pypi-types/src/metadata.rs`:175 on 2024-01-18 19:59_

It works correctly as-is -- if I use `Author: =?utf-8?q?=C3=A4_space?= <x@y.org>`, it gets parsed as `author: ä space <x@y.org>`.

The thing that the author pointed out in the linked mailparse issue is that this decoding happens via `mailparse::parse_mail`.

---

_@charliermarsh reviewed on 2024-01-18 20:02_

---

_Review comment by @charliermarsh on `crates/pypi-types/src/metadata.rs`:175 on 2024-01-18 20:02_

Added!

---

_@BurntSushi approved on 2024-01-18 20:05_

Ah okay, so `mailparse` is already doing RFC 2047 decoding? If so, then yeah, this change LGTM.

---

_@charliermarsh reviewed on 2024-01-18 20:06_

---

_Review comment by @charliermarsh on `crates/pypi-types/src/metadata.rs`:175 on 2024-01-18 20:06_

FWIW, name _has_ to be ASCII: https://packaging.python.org/en/latest/specifications/core-metadata/#name

---

_Comment by @charliermarsh on 2024-01-18 20:07_

Yeah, that's why I'm slightly confused as to why this was applied upstream.

---

_Comment by @charliermarsh on 2024-01-18 20:08_

The `mailparse` author does say here: https://github.com/staktrace/mailparse/issues/50#issuecomment-939314410

> It kind of looks like you're passing in utf-8 or other non-ascii data into mailparse (from [here](https://github.com/PyO3/python-pkginfo-rs/blob/17523fac9af6c50080f9e087b9fd271c49bd2ec8/src/metadata.rs#L170)) and then expecting that to not get mangled. But really you're using mailparse in a way that it's not meant to be used.


---

_Comment by @charliermarsh on 2024-01-18 20:09_

Honestly, I think this might've been fixed in mailparse later: https://github.com/staktrace/mailparse/pull/104

---

_Merged by @charliermarsh on 2024-01-18 20:09_

---

_Closed by @charliermarsh on 2024-01-18 20:09_

---

_Branch deleted on 2024-01-18 20:09_

---
