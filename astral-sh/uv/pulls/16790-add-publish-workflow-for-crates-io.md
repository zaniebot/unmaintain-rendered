```yaml
number: 16790
title: Add publish workflow for crates.io
type: pull_request
state: merged
author: zanieb
labels: []
assignees: []
merged: true
base: zb/crates
head: zb/creates-publish
created_at: 2025-11-20T15:35:56Z
updated_at: 2025-11-20T17:52:42Z
url: https://github.com/astral-sh/uv/pull/16790
synced_at: 2026-01-12T16:12:26Z
```

# Add publish workflow for crates.io

---

_@zanieb_

Extends #16770 by adding a publish workflow. I verified it with `cargo publish --workspace --dry-run` which led to some fixes adding version pins to the root `Cargo.toml`.

---

_@github-advanced-security[bot] reviewed on 2025-11-20 15:38_

---

_Review comment by @github-advanced-security[bot] on `.github/workflows/publish-crates.yml`:26 on 2025-11-20 15:38_

credential persistence through GitHub Actions artifacts

[Show more details](https://github.com/astral-sh/uv/security/code-scanning/178)

---

_Review comment by @github-advanced-security[bot] on `.github/workflows/release.yml`:230 on 2025-11-20 15:38_

secrets unconditionally inherited by called workflow

[Show more details](https://github.com/astral-sh/uv/security/code-scanning/177)

---

_Review comment by @github-advanced-security[bot] on `.github/workflows/publish-crates.yml`:37 on 2025-11-20 15:38_

prefer trusted publishing for authentication

[Show more details](https://github.com/astral-sh/uv/security/code-scanning/179)

---

_@Gankra reviewed on 2025-11-20 15:52_

---

_Review comment by @Gankra on `.github/workflows/release.yml`:229 on 2025-11-20 15:52_

just noting informationally: we're defaulting here to not publishing prereleases to crates.io. That's probably good/fine. It's configurable:

https://axodotdev.github.io/cargo-dist/book/reference/config.html?highlight=publish-#publish-prereleases

---

_@github-advanced-security[bot] reviewed on 2025-11-20 17:17_

---

_Review comment by @github-advanced-security[bot] on `.github/workflows/publish-crates.yml`:37 on 2025-11-20 17:17_

prefer trusted publishing for authentication

[Show more details](https://github.com/astral-sh/uv/security/code-scanning/180)

---

_@github-advanced-security[bot] reviewed on 2025-11-20 17:17_

---

_Review comment by @github-advanced-security[bot] on `.github/workflows/publish-crates.yml`:30 on 2025-11-20 17:17_

prefer trusted publishing for authentication

[Show more details](https://github.com/astral-sh/uv/security/code-scanning/181)

---

_Review requested from @Gankra by @zanieb on 2025-11-20 17:28_

---

_@Gankra approved on 2025-11-20 17:48_

Looks right to me. I think if you're actually lockstep-versioning everything you can potentially use `version.workspace = true` in some places too, but I don't think it matters much now that we have the tooling to edit it.

---

_Merged by @zanieb on 2025-11-20 17:52_

---

_Closed by @zanieb on 2025-11-20 17:52_

---

_Branch deleted on 2025-11-20 17:52_

---
