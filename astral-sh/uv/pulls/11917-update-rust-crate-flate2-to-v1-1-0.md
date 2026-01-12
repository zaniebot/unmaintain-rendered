```yaml
number: 11917
title: Update Rust crate flate2 to v1.1.0
type: pull_request
state: merged
author: renovate
labels:
  - internal
assignees: []
merged: true
base: main
head: renovate/flate2-1.x-lockfile
created_at: 2025-03-03T03:16:57Z
updated_at: 2025-03-03T04:22:07Z
url: https://github.com/astral-sh/uv/pull/11917
synced_at: 2026-01-12T16:10:03Z
```

# Update Rust crate flate2 to v1.1.0

---

_@renovate_

This PR contains the following updates:

| Package | Type | Update | Change |
|---|---|---|---|
| [flate2](https://redirect.github.com/rust-lang/flate2-rs) | dependencies | minor | `1.0.35` -> `1.1.0` |
| [flate2](https://redirect.github.com/rust-lang/flate2-rs) | workspace.dependencies | minor | `1.0.35` -> `1.1.0` |

---

### Release Notes

<details>
<summary>rust-lang/flate2-rs (flate2)</summary>

### [`v1.1.0`](https://redirect.github.com/rust-lang/flate2-rs/releases/tag/1.1.0)

[Compare Source](https://redirect.github.com/rust-lang/flate2-rs/compare/1.0.35...1.1.0)

#### What's Changed

-   Fix cfgs by [@&#8203;kornelski](https://redirect.github.com/kornelski) in [https://github.com/rust-lang/flate2-rs/pull/441](https://redirect.github.com/rust-lang/flate2-rs/pull/441)
-   update CI to use new wasi target by [@&#8203;oyvindln](https://redirect.github.com/oyvindln) in [https://github.com/rust-lang/flate2-rs/pull/444](https://redirect.github.com/rust-lang/flate2-rs/pull/444)
-   Implement `Clone` for `CompressError` and `DecompressError` by [@&#8203;mkrasnitski](https://redirect.github.com/mkrasnitski) in [https://github.com/rust-lang/flate2-rs/pull/445](https://redirect.github.com/rust-lang/flate2-rs/pull/445)
-   Update LICENSE-MIT by [@&#8203;maximevtush](https://redirect.github.com/maximevtush) in [https://github.com/rust-lang/flate2-rs/pull/448](https://redirect.github.com/rust-lang/flate2-rs/pull/448)
-   feat: replace custom u16 le parser with existent rust method by [@&#8203;CosminPerRam](https://redirect.github.com/CosminPerRam) in [https://github.com/rust-lang/flate2-rs/pull/450](https://redirect.github.com/rust-lang/flate2-rs/pull/450)
-   Fix CI by [@&#8203;Byron](https://redirect.github.com/Byron) in [https://github.com/rust-lang/flate2-rs/pull/449](https://redirect.github.com/rust-lang/flate2-rs/pull/449)
-   Do not use cloudflare-zlib-sys 0.3.4 by [@&#8203;jongiddy](https://redirect.github.com/jongiddy) in [https://github.com/rust-lang/flate2-rs/pull/451](https://redirect.github.com/rust-lang/flate2-rs/pull/451)
-   Increase minimum compiler version to 1.67 by [@&#8203;jongiddy](https://redirect.github.com/jongiddy) in [https://github.com/rust-lang/flate2-rs/pull/452](https://redirect.github.com/rust-lang/flate2-rs/pull/452)
-   deps: bump miniz_oxide to 0.8.4 by [@&#8203;CosminPerRam](https://redirect.github.com/CosminPerRam) in [https://github.com/rust-lang/flate2-rs/pull/459](https://redirect.github.com/rust-lang/flate2-rs/pull/459)
-   deps(dev): update rand to 0.9 by [@&#8203;CosminPerRam](https://redirect.github.com/CosminPerRam) in [https://github.com/rust-lang/flate2-rs/pull/458](https://redirect.github.com/rust-lang/flate2-rs/pull/458)
-   docs: fix spelling mistake in flate2::zlib::write::ZlibDecoder by [@&#8203;CosminPerRam](https://redirect.github.com/CosminPerRam) in [https://github.com/rust-lang/flate2-rs/pull/455](https://redirect.github.com/rust-lang/flate2-rs/pull/455)
-   feat: remove explicit default impls by [@&#8203;CosminPerRam](https://redirect.github.com/CosminPerRam) in [https://github.com/rust-lang/flate2-rs/pull/457](https://redirect.github.com/rust-lang/flate2-rs/pull/457)
-   Change private bounds from `R: Read` to `R: BufRead` by [@&#8203;JonathanBrouwer](https://redirect.github.com/JonathanBrouwer) in [https://github.com/rust-lang/flate2-rs/pull/453](https://redirect.github.com/rust-lang/flate2-rs/pull/453)
-   feat: replace manual copy loop with rust-provided function by [@&#8203;CosminPerRam](https://redirect.github.com/CosminPerRam) in [https://github.com/rust-lang/flate2-rs/pull/456](https://redirect.github.com/rust-lang/flate2-rs/pull/456)
-   feat: reduce CrcReader::sum calls in GzEncoder::read_footer by [@&#8203;CosminPerRam](https://redirect.github.com/CosminPerRam) in [https://github.com/rust-lang/flate2-rs/pull/454](https://redirect.github.com/rust-lang/flate2-rs/pull/454)
-   feat: remove redundant if guard on option value match by [@&#8203;CosminPerRam](https://redirect.github.com/CosminPerRam) in [https://github.com/rust-lang/flate2-rs/pull/464](https://redirect.github.com/rust-lang/flate2-rs/pull/464)
-   feat: add Error associated type in zio::Ops to handle multiple errors by [@&#8203;CosminPerRam](https://redirect.github.com/CosminPerRam) in [https://github.com/rust-lang/flate2-rs/pull/461](https://redirect.github.com/rust-lang/flate2-rs/pull/461)
-   feat: remove explicit redundant lifetime by [@&#8203;CosminPerRam](https://redirect.github.com/CosminPerRam) in [https://github.com/rust-lang/flate2-rs/pull/465](https://redirect.github.com/rust-lang/flate2-rs/pull/465)
-   feat: impl From<Flush> to MZFlush by [@&#8203;CosminPerRam](https://redirect.github.com/CosminPerRam) in [https://github.com/rust-lang/flate2-rs/pull/462](https://redirect.github.com/rust-lang/flate2-rs/pull/462)
-   upgrade zlib-rs to version `0.4.2` by [@&#8203;folkertdev](https://redirect.github.com/folkertdev) in [https://github.com/rust-lang/flate2-rs/pull/466](https://redirect.github.com/rust-lang/flate2-rs/pull/466)

#### New Contributors

-   [@&#8203;mkrasnitski](https://redirect.github.com/mkrasnitski) made their first contribution in [https://github.com/rust-lang/flate2-rs/pull/445](https://redirect.github.com/rust-lang/flate2-rs/pull/445)
-   [@&#8203;maximevtush](https://redirect.github.com/maximevtush) made their first contribution in [https://github.com/rust-lang/flate2-rs/pull/448](https://redirect.github.com/rust-lang/flate2-rs/pull/448)
-   [@&#8203;CosminPerRam](https://redirect.github.com/CosminPerRam) made their first contribution in [https://github.com/rust-lang/flate2-rs/pull/450](https://redirect.github.com/rust-lang/flate2-rs/pull/450)
-   [@&#8203;JonathanBrouwer](https://redirect.github.com/JonathanBrouwer) made their first contribution in [https://github.com/rust-lang/flate2-rs/pull/453](https://redirect.github.com/rust-lang/flate2-rs/pull/453)

**Full Changelog**: https://github.com/rust-lang/flate2-rs/compare/1.0.35...1.1.0

</details>

---

### Configuration

📅 **Schedule**: Branch creation - "before 4am on Monday" (UTC), Automerge - At any time (no schedule defined).

🚦 **Automerge**: Disabled by config. Please merge this manually once you are satisfied.

♻ **Rebasing**: Whenever PR becomes conflicted, or you tick the rebase/retry checkbox.

🔕 **Ignore**: Close this PR and you won't be reminded about these updates again.

---

 - [ ] <!-- rebase-check -->If you want to rebase/retry this PR, check this box

---

This PR was generated by [Mend Renovate](https://mend.io/renovate/). View the [repository job log](https://developer.mend.io/github/astral-sh/uv).
<!--renovate-debug:eyJjcmVhdGVkSW5WZXIiOiIzOS4xNzYuMiIsInVwZGF0ZWRJblZlciI6IjM5LjE3Ni4yIiwidGFyZ2V0QnJhbmNoIjoibWFpbiIsImxhYmVscyI6WyJpbnRlcm5hbCJdfQ==-->


---

_Label `internal` added by @renovate[bot] on 2025-03-03 03:16_

---

_Merged by @charliermarsh on 2025-03-03 04:22_

---

_Closed by @charliermarsh on 2025-03-03 04:22_

---

_Branch deleted on 2025-03-03 04:22_

---
