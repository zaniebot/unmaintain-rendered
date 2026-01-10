```yaml
number: 11088
title: Percent-decode URLs in canonical comparisons
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/percent
created_at: 2025-01-30T03:44:16Z
updated_at: 2025-01-31T20:45:50Z
url: https://github.com/astral-sh/uv/pull/11088
synced_at: 2026-01-10T11:10:34Z
```

# Percent-decode URLs in canonical comparisons

---

_Pull request opened by @charliermarsh on 2025-01-30 03:44_

## Summary

This PR adds an additional normalization step to `CanonicalUrl` whereby we now percent-decode the path, to ensure that (e.g.) `torch-2.5.1%2Bcpu.cxx11.abi-cp39-cp39-linux_x86_64.whl` and `torch-2.5.1+cpu.cxx11.abi-cp39-cp39-linux_x86_64.whl` are considered equal. Further, when generating the "reinstall" report, we use the canonical URL rather than the verbatim URL.

In making this change, I also learned that we don't apply any of the normalization passes to `file://` URLs. I inadvertently removed it in https://github.com/astral-sh/uv/pull/3010/commits/93d606aba20c39149390a9191879e82194ea4e91, since setting the password or URL on ` file://` URL errors -- but now suppress those errors anyway.

Closes https://github.com/astral-sh/uv/issues/11082.

## Test Plan

- Downloaded a [PyTorch wheel](https://download.pytorch.org/whl/cpu-cxx11-abi/torch-2.5.1%2Bcpu.cxx11.abi-cp39-cp39-linux_x86_64.whl)
- `python3.9 -m pip install torch-2.5.1+cpu.cxx11.abi-cp39-cp39-linux_x86_64.whl --platform linux_x86_64 --target foo --no-deps`
- `cargo run pip install torch-2.5.1+cpu.cxx11.abi-cp39-cp39-linux_x86_64.whl --python-platform linux --python-version 3.9 --target foo --no-deps`
- Verified that the package had the `~` symbol for the reinstall.

---

_Label `bug` added by @charliermarsh on 2025-01-30 03:44_

---

_Review requested from @zanieb by @charliermarsh on 2025-01-30 03:44_

---

_Review requested from @konstin by @charliermarsh on 2025-01-30 03:44_

---

_@charliermarsh reviewed on 2025-01-30 03:46_

---

_Review comment by @charliermarsh on `crates/uv-cache-key/src/canonical_url.rs`:90 on 2025-01-30 03:46_

Hmm, this actually might be wrong. What if a slash is percent-encoded? We'd be changing it to a path segment.

I guess we could go the other way -- always percent-encode the URL?

---

_@charliermarsh reviewed on 2025-01-30 03:50_

---

_Review comment by @charliermarsh on `crates/uv-cache-key/src/canonical_url.rs`:90 on 2025-01-30 03:50_

But that's also not quite right... Because if the URL is already percent-encoded, we'd be re-encoding it. It's not idempotent.

---

_@charliermarsh reviewed on 2025-01-30 03:51_

---

_Review comment by @charliermarsh on `crates/uv-cache-key/src/canonical_url.rs`:90 on 2025-01-30 03:51_

I guess we want to percent-decode each path segment, but _not_ slashes?

---

_@charliermarsh reviewed on 2025-01-30 03:55_

---

_Review comment by @charliermarsh on `crates/uv-cache-key/src/canonical_url.rs`:90 on 2025-01-30 03:55_

Okay, I'm actually not totally sure how to do this.

---

_@konstin reviewed on 2025-01-30 10:43_

---

_Review comment by @konstin on `crates/uv-cache-key/src/canonical_url.rs`:90 on 2025-01-30 10:43_

I tried figuring this out with https://url.spec.whatwg.org but it's still unclear how to handle file urls vs. https urls. If we're eager there is a complete decoding algorithm there though.

---

_Review comment by @BurntSushi on `crates/uv-cache-key/src/canonical_url.rs`:90 on 2025-01-30 17:42_

My first inclination here was to wonder why the `url` crate wasn't handling this for us automatically, or at least, provide an option to do so. It looks like [`+` is actually somewhat special](https://github.com/servo/rust-url/issues/695). For example, if you parse `https://burntsushi.net/foo bar` and `https://burntsushi.net/foo%20bar` into a `Url`, then the resulting `Url` values compare as equivalent. But if you parse `https://burntsushi.net/foo+bar` and `https://burntsushi.net/foo%2Bbar` into `Url` values, then they are _not_ equivalent.

Decoding each path segment on its own seems plausible, but it seems like that could also bork things if URL parsing decodes them? But no, I don't think it does:

```rust
use url::Url;

fn main() {
    let url1: Url = "https://burntsushi.net/foo%2520bar".parse().unwrap();
    assert_eq!(url1.path(), "/foo%2520bar");
}
```

The above assert passes.

So hmmm, without reading the URL spec, it seems like decoding each segment should be fine?

---

_@BurntSushi reviewed on 2025-01-30 17:42_

---

_@charliermarsh reviewed on 2025-01-30 17:47_

---

_Review comment by @charliermarsh on `crates/uv-cache-key/src/canonical_url.rs`:90 on 2025-01-30 17:47_

What if the segment contains a percent-encoded slash, though?

---

_@charliermarsh reviewed on 2025-01-30 17:47_

---

_Review comment by @charliermarsh on `crates/uv-cache-key/src/canonical_url.rs`:90 on 2025-01-30 17:47_

Honestly, we could also consider just decoding `+`, if it's "special" (or something like that).

---

_@BurntSushi reviewed on 2025-01-30 18:12_

---

_Review comment by @BurntSushi on `crates/uv-cache-key/src/canonical_url.rs`:90 on 2025-01-30 18:12_

I think the `url` crate helps you here: https://docs.rs/url/latest/url/struct.PathSegmentsMut.html#method.extend

---

_@BurntSushi reviewed on 2025-01-30 18:12_

---

_Review comment by @BurntSushi on `crates/uv-cache-key/src/canonical_url.rs`:90 on 2025-01-30 18:12_

> Each segment is percent-encoded like in Url::parse or Url::join, except that % and / characters are also encoded (to %25 and %2F). This is unlike Url::parse where % is left as-is in case some of the input is already percent-encoded, and / denotes a path segment separator.)

---

_@konstin reviewed on 2025-01-30 20:16_

---

_Review comment by @konstin on `crates/uv-cache-key/src/canonical_url.rs`:90 on 2025-01-30 20:16_

from the spec it also seems that file:// and https:// have different rules, so we should test that our solution works for both 

---

_@BurntSushi approved on 2025-01-31 20:37_

LGTM!

---

_Merged by @charliermarsh on 2025-01-31 20:45_

---

_Closed by @charliermarsh on 2025-01-31 20:45_

---

_Branch deleted on 2025-01-31 20:45_

---
