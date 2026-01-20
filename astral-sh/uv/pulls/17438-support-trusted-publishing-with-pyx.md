```yaml
number: 17438
title: Support Trusted Publishing with pyx
type: pull_request
state: merged
author: woodruffw
labels:
  - enhancement
  - registry
assignees: []
merged: true
base: main
head: ww/pyx-tp-svc
created_at: 2026-01-13T15:59:46Z
updated_at: 2026-01-20T22:18:27Z
url: https://github.com/astral-sh/uv/pull/17438
synced_at: 2026-01-20T22:53:04Z
```

# Support Trusted Publishing with pyx

---

_@woodruffw_

## Summary

~~WIP.~~ This follows #17418 and adds a `PyxPublishingService` that speaks the pyx-specific APIs for Trusted Publishing.

TODOs:

- [x] Needs publishing integration tests (see below).
- [x] Needs changes to `OidcTokenClaims` (these are currently GitHub-specific, they need to become an enum over various supported platforms).

## Test Plan

I'll add new publishing integration tests for the following scenarios:

- [x] Trusted Publishing between GitLab CI/CD <-> PyPI: #17443 
- [x] Trusted Publishing between GitHub Actions <-> pyx
- [x] Trusted Publishing between GitLab CI/CD <-> pyx

---

_Assigned to @woodruffw by @woodruffw on 2026-01-13 15:59_

---

_@woodruffw reviewed on 2026-01-13 22:45_

---

_Review comment by @woodruffw on `crates/uv-publish/src/trusted_publishing.rs`:88 on 2026-01-13 22:45_

Flagging: normally I'd consider this an anti-pattern (in terms of untagged enums taking longer to match/producing suboptimal errors on failure), but the context here _might_ make it acceptable:

1. We're in an error path on an already "cold" flow (uploading)
2. The overwhelming majority of users are probably using GitHub publishers, so serde won't need to backtrack for them

OTOH, I could make this explicitly tagged by adding a custom `Deserialize` that peeks the `iss` and dispatches based on that. That wouldn't be hard, AFAIK there's just no way to do it with serde's derive APIs.

---

_@woodruffw reviewed on 2026-01-15 22:38_

---

_Review comment by @woodruffw on `scripts/publish/test_publish.py`:434 on 2026-01-15 22:38_

NOTE: I've removed the previous strategy of polling the index for a "fresh" version to test publishing with, in favor of this "timestamp" strategy where we pick a monotonically increasing (but not contiguous) version.

The main advantage of this is that we don't need to block/retry on the index to get a fresh version, i.e. we spend less time doing something unrelated to the main test functionality itself. The downside is that the versions are a little less nice.



---

_@woodruffw reviewed on 2026-01-15 22:38_

---

_Review comment by @woodruffw on `scripts/publish/test_publish.py`:788 on 2026-01-15 22:38_

The log volume was a bit extreme (and not helpful), so I've dropped it back down to `INFO`.

---

_Marked ready for review by @woodruffw on 2026-01-16 03:02_

---

_Review requested from @zanieb by @woodruffw on 2026-01-16 03:16_

---

_Review requested from @konstin by @woodruffw on 2026-01-16 03:16_

---

_Label `registry` added by @woodruffw on 2026-01-16 03:17_

---

_Review comment by @konstin on `scripts/publish/test_publish.py`:434 on 2026-01-19 11:05_

Do we still need the `fresh_version_strategy` flag? It's not used anymore 

---

_@konstin reviewed on 2026-01-19 11:05_

---

_Review comment by @konstin on `crates/uv-publish/src/trusted_publishing/pyx.rs`:89 on 2026-01-19 11:19_

Note that `path_segments` is sensitive to trailing slashes:

```rust
use std::str::FromStr;
use url::Url;

fn main() -> Result<(), ()> {
    dbg!(
        Url::from_str("https://pyx.dev/v1/upload/workspace_name/registry_name")
            .unwrap()
            .path_segments()
            .map_or(Vec::new(), Iterator::collect)
    );
    dbg!(
        Url::from_str("https://pyx.dev/v1/upload/workspace_name/registry_name/")
            .unwrap()
            .path_segments()
            .map_or(Vec::new(), Iterator::collect)
    );
    Ok(())
}
```

```
[src/main.rs:5:5] Url::from_str("https://pyx.dev/v1/upload/workspace_name/registry_name").unwrap().path_segments().map_or(Vec::new(),
Iterator::collect) = [
    "v1",
    "upload",
    "workspace_name",
    "registry_name",
]
[src/main.rs:11:5] Url::from_str("https://pyx.dev/v1/upload/workspace_name/registry_name/").unwrap().path_segments().map_or(Vec::new(),
Iterator::collect) = [
    "v1",
    "upload",
    "workspace_name",
    "registry_name",
    "",
]
```

If pyx doesn't allow trailing slashes, the URL is invalid, though maybe we want a separate error for this, it sounds easy to get wrong? We had those cases with trailing slashes for PyPI too.

---

_Review comment by @konstin on `crates/uv-publish/src/lib.rs`:467 on 2026-01-19 11:43_

nit: If we make `get_token` as method on the trait with default implementation, we can skip the async-traint-in-box and do:

```rust
                PyxPublishingService::new(registry, client)
                    .get_token()
                    .await
                    .map_err(Box::new)?
```

---

_Review comment by @konstin on `crates/uv-publish/src/trusted_publishing.rs`:88 on 2026-01-19 11:47_

This sounds totally fine to me in the error path, it might even help in some fringe token mixup scenarios.

---

_@konstin approved on 2026-01-19 11:49_

---

_Label `enhancement` added by @konstin on 2026-01-19 11:49_

---

_@woodruffw reviewed on 2026-01-20 15:16_

---

_Review comment by @woodruffw on `scripts/publish/test_publish.py`:434 on 2026-01-20 15:16_

Nope, it's safe to remove. Will do!

---

_@woodruffw reviewed on 2026-01-20 15:17_

---

_Review comment by @woodruffw on `crates/uv-publish/src/trusted_publishing/pyx.rs`:89 on 2026-01-20 15:17_

Thanks for catching this! We do allow trailing slashes on pyx's upload endpoint, so I'll accommodate here.

---

_@woodruffw reviewed on 2026-01-20 17:22_

---

_Review comment by @woodruffw on `crates/uv-publish/src/lib.rs`:467 on 2026-01-20 17:22_

Oh yeah, that's way clearer. I'll refactor!

---

_Merged by @woodruffw on 2026-01-20 22:18_

---

_Closed by @woodruffw on 2026-01-20 22:18_

---

_Branch deleted on 2026-01-20 22:18_

---
