```yaml
number: 8344
title: Bump uuid from 1.4.1 to 1.5.0
type: pull_request
state: merged
author: dependabot
labels:
  - internal
assignees: []
merged: true
base: main
head: dependabot/cargo/uuid-1.5.0
created_at: 2023-10-30T08:30:08Z
updated_at: 2023-10-30T09:04:50Z
url: https://github.com/astral-sh/ruff/pull/8344
synced_at: 2026-01-12T15:55:26Z
```

# Bump uuid from 1.4.1 to 1.5.0

---

_@dependabot_

Bumps [uuid](https://github.com/uuid-rs/uuid) from 1.4.1 to 1.5.0.
<details>
<summary>Release notes</summary>
<p><em>Sourced from <a href="https://github.com/uuid-rs/uuid/releases">uuid's releases</a>.</em></p>
<blockquote>
<h2>1.5.0</h2>
<h2>What's Changed</h2>
<ul>
<li>Add impl From<!-- raw HTML omitted --> for String under the std feature flag by <a href="https://github.com/brahms116"><code>@​brahms116</code></a> in <a href="https://redirect.github.com/uuid-rs/uuid/pull/700">uuid-rs/uuid#700</a></li>
<li>Remove dead link to templates by <a href="https://github.com/KodrAus"><code>@​KodrAus</code></a> in <a href="https://redirect.github.com/uuid-rs/uuid/pull/704">uuid-rs/uuid#704</a></li>
<li>make ClockSequence wrap correctly by <a href="https://github.com/fef1312"><code>@​fef1312</code></a> in <a href="https://redirect.github.com/uuid-rs/uuid/pull/705">uuid-rs/uuid#705</a></li>
<li>Track MSRV in Cargo.toml by <a href="https://github.com/KodrAus"><code>@​KodrAus</code></a> in <a href="https://redirect.github.com/uuid-rs/uuid/pull/706">uuid-rs/uuid#706</a></li>
<li>Support converting between Uuid and vec by <a href="https://github.com/KodrAus"><code>@​KodrAus</code></a> in <a href="https://redirect.github.com/uuid-rs/uuid/pull/703">uuid-rs/uuid#703</a></li>
<li>Replace MIPS with Miri and add clippy to CI by <a href="https://github.com/KodrAus"><code>@​KodrAus</code></a> in <a href="https://redirect.github.com/uuid-rs/uuid/pull/712">uuid-rs/uuid#712</a></li>
<li>Added <code>bytemuck</code> support by <a href="https://github.com/John-Toohey"><code>@​John-Toohey</code></a> in <a href="https://redirect.github.com/uuid-rs/uuid/pull/711">uuid-rs/uuid#711</a></li>
<li>Prepare for 1.5.0 release by <a href="https://github.com/KodrAus"><code>@​KodrAus</code></a> in <a href="https://redirect.github.com/uuid-rs/uuid/pull/713">uuid-rs/uuid#713</a></li>
</ul>
<h2>New Contributors</h2>
<ul>
<li><a href="https://github.com/brahms116"><code>@​brahms116</code></a> made their first contribution in <a href="https://redirect.github.com/uuid-rs/uuid/pull/700">uuid-rs/uuid#700</a></li>
<li><a href="https://github.com/fef1312"><code>@​fef1312</code></a> made their first contribution in <a href="https://redirect.github.com/uuid-rs/uuid/pull/705">uuid-rs/uuid#705</a></li>
<li><a href="https://github.com/John-Toohey"><code>@​John-Toohey</code></a> made their first contribution in <a href="https://redirect.github.com/uuid-rs/uuid/pull/711">uuid-rs/uuid#711</a></li>
</ul>
<p><strong>Full Changelog</strong>: <a href="https://github.com/uuid-rs/uuid/compare/1.4.1...1.5.0">https://github.com/uuid-rs/uuid/compare/1.4.1...1.5.0</a></p>
</blockquote>
</details>
<details>
<summary>Commits</summary>
<ul>
<li><a href="https://github.com/uuid-rs/uuid/commit/e68b0108fa295ce0b742a4990d08421857286ebc"><code>e68b010</code></a> Merge pull request <a href="https://redirect.github.com/uuid-rs/uuid/issues/713">#713</a> from uuid-rs/cargo/1.5.0</li>
<li><a href="https://github.com/uuid-rs/uuid/commit/b1cc27a118e97c27bdb950fa753ecc5943c5df5c"><code>b1cc27a</code></a> prepare for 1.5.0 release</li>
<li><a href="https://github.com/uuid-rs/uuid/commit/b8ebdee9b0e8a31fa2e0fcfd2bdd848418fc189f"><code>b8ebdee</code></a> Merge pull request <a href="https://redirect.github.com/uuid-rs/uuid/issues/711">#711</a> from John-Toohey/bytemuck</li>
<li><a href="https://github.com/uuid-rs/uuid/commit/2dad70d3c7dd21d8bda1fc1cf5e04e7cc3dffb85"><code>2dad70d</code></a> Added the <code>bytemuck</code> optional dependency to <code>lib.rs</code> documentation</li>
<li><a href="https://github.com/uuid-rs/uuid/commit/bcf2b58997a0a87bc11eb276d7f1f6e93979c108"><code>bcf2b58</code></a> Added Bytemuck to .github/workflows/ci.yml::env::DEP_FEATURES</li>
<li><a href="https://github.com/uuid-rs/uuid/commit/a8d2e1d4bf0a0867701a2369308eb7a7b845ff94"><code>a8d2e1d</code></a> Merge pull request <a href="https://redirect.github.com/uuid-rs/uuid/issues/712">#712</a> from uuid-rs/ci/miri-clippy</li>
<li><a href="https://github.com/uuid-rs/uuid/commit/0c5b2dfebdbfc950ddd5d69900d814dabce5f754"><code>0c5b2df</code></a> fix up a clippy warning</li>
<li><a href="https://github.com/uuid-rs/uuid/commit/1d4bd6e5b26fa63e6f674f1e13b02f086f500f63"><code>1d4bd6e</code></a> Merge pull request <a href="https://redirect.github.com/uuid-rs/uuid/issues/703">#703</a> from uuid-rs/feat/convert-to-vec</li>
<li><a href="https://github.com/uuid-rs/uuid/commit/52b3fbc04ab867778931f2a6e5061b8e1b15b681"><code>52b3fbc</code></a> replace MIPS with Miri and add clippy to CI</li>
<li><a href="https://github.com/uuid-rs/uuid/commit/3833d095c15c8b524649cc417a38db14ef677815"><code>3833d09</code></a> Make the bytemuck dependency look more like the other dependencies</li>
<li>Additional commits viewable in <a href="https://github.com/uuid-rs/uuid/compare/1.4.1...1.5.0">compare view</a></li>
</ul>
</details>
<br />


[![Dependabot compatibility score](https://dependabot-badges.githubapp.com/badges/compatibility_score?dependency-name=uuid&package-manager=cargo&previous-version=1.4.1&new-version=1.5.0)](https://docs.github.com/en/github/managing-security-vulnerabilities/about-dependabot-security-updates#about-compatibility-scores)

Dependabot will resolve any conflicts with this PR as long as you don't alter it yourself. You can also trigger a rebase manually by commenting `@dependabot rebase`.

[//]: # (dependabot-automerge-start)
[//]: # (dependabot-automerge-end)

---

<details>
<summary>Dependabot commands and options</summary>
<br />

You can trigger Dependabot actions by commenting on this PR:
- `@dependabot rebase` will rebase this PR
- `@dependabot recreate` will recreate this PR, overwriting any edits that have been made to it
- `@dependabot merge` will merge this PR after your CI passes on it
- `@dependabot squash and merge` will squash and merge this PR after your CI passes on it
- `@dependabot cancel merge` will cancel a previously requested merge and block automerging
- `@dependabot reopen` will reopen this PR if it is closed
- `@dependabot close` will close this PR and stop Dependabot recreating it. You can achieve the same result by closing it manually
- `@dependabot show <dependency name> ignore conditions` will show all of the ignore conditions of the specified dependency
- `@dependabot ignore this major version` will close this PR and stop Dependabot creating any more for this major version (unless you reopen the PR or upgrade to it yourself)
- `@dependabot ignore this minor version` will close this PR and stop Dependabot creating any more for this minor version (unless you reopen the PR or upgrade to it yourself)
- `@dependabot ignore this dependency` will close this PR and stop Dependabot creating any more for this dependency (unless you reopen the PR or upgrade to it yourself)


</details>

---

_Label `internal` added by @dependabot[bot] on 2023-10-30 08:30_

---

_Comment by @github-actions[bot] on 2023-10-30 08:48_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no linter changes.

✅ ecosystem check detected no format changes.



---

_Merged by @MichaReiser on 2023-10-30 09:04_

---

_Closed by @MichaReiser on 2023-10-30 09:04_

---

_Branch deleted on 2023-10-30 09:04_

---
