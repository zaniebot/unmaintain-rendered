---
number: 16871
title: "No GitHub attestations found for aqua:astral-sh/uv@0.9.11 when installing UV through mise"
type: issue
state: closed
author: GuilhermePSC
labels:
  - bug
assignees: []
created_at: 2025-11-27T10:16:47Z
updated_at: 2025-11-30T15:12:18Z
url: https://github.com/astral-sh/uv/issues/16871
synced_at: 2026-01-07T13:12:19-06:00
---

# No GitHub attestations found for aqua:astral-sh/uv@0.9.11 when installing UV through mise

---

_Issue opened by @GuilhermePSC on 2025-11-27 10:16_

### Summary

Minimal reproduction steps:

- Using `ubuntu:latest`, install [mise](https://mise.jdx.dev/installing-mise.html) and have this `mise.toml`:
```
[tools]
uv = "0.9.11"
```
- `mise install`

You will get this error:
```
mise uv@0.9.11       download uv-x86_64-unknown-linux-musl.tar.gz
mise uv@0.9.11       verify GitHub attestations
mise ERROR Failed to install aqua:astral-sh/uv@0.9.11: 
   0: No GitHub attestations found for aqua:astral-sh/uv@0.9.11, but attestations are expected per aqua registry configuration

Location:
   src/backend/aqua.rs:704

Backtrace omitted. Run with RUST_BACKTRACE=1 environment variable to display it.
Run with RUST_BACKTRACE=full to include source snippets.
mise ERROR Run with --verbose or MISE_VERBOSE=1 for more information
```

I think this might be related to what's mentioned in the release notes of [0.9.11](https://github.com/astral-sh/uv/releases/tag/0.9.11)?
> Due to rate limiting during https://github.com/astral-sh/uv/pull/16770, this release [was partially published](https://github.com/astral-sh/uv/actions/runs/19553586192) and manually finished. Consequently, crates.io temporarily did not include all of the artifacts and the GitHub Release was published by a maintainer instead of GitHub Actions. The artifacts from GitHub Actions were used without alteration. There should be no consequences from this; we just want to be transparent about the provenance of the artifacts.

Maybe there were consequences after all? 😅 

I tested uv 0.9.13 (latest at the time of typing) and it works fine.

### Platform

macOS 15.7.1 arm64

### Version

uv 0.9.11

### Python version

_No response_

---

_Label `bug` added by @GuilhermePSC on 2025-11-27 10:16_

---

_Comment by @risu729 on 2025-11-27 11:19_

Just to clarify, this is not a mise-specific issue.

```
$ xh -d https://github.com/astral-sh/uv/releases/download/0.9.11/uv-x86_64-unknown-linux-musl.tar.gz
$ gh attestation verify uv-x86_64-unknown-linux-musl.tar.gz --repo astral-sh/uv
Loaded digest sha256:5cc06fe71374a8883aeb2c83a141a4b5fac8584ee894ba31c5792254508b4e9a for file://uv-x86_64-unknown-linux-musl.tar.gz
✗ Loading attestations from GitHub API failed

Error: HTTP 404: Not Found (https://api.github.com/repos/astral-sh/uv/attestations/sha256:5cc06fe71374a8883aeb2c83a141a4b5fac8584ee894ba31c5792254508b4e9a?per_page=30&predicate_type=https://slsa.dev/provenance/v1)
```

However, the reason is obvious: those assets were published by a maintainer. I think we just need to ignore GitHub attestations for this version.
I opened https://github.com/aquaproj/aqua-registry/pull/44830 to fix this.

---

_Comment by @zanieb on 2025-11-27 13:25_

Sorry about that. I'll update the note.

---

_Comment by @risu729 on 2025-11-27 14:47_

Thank you so much!

---

_Closed by @zanieb on 2025-11-30 15:12_

---
