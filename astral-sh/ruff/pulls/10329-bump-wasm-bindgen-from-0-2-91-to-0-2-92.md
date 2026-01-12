```yaml
number: 10329
title: Bump wasm-bindgen from 0.2.91 to 0.2.92
type: pull_request
state: merged
author: dependabot
labels:
  - internal
assignees: []
merged: true
base: main
head: dependabot/cargo/wasm-bindgen-0.2.92
created_at: 2024-03-11T08:28:27Z
updated_at: 2024-03-11T09:29:09Z
url: https://github.com/astral-sh/ruff/pull/10329
synced_at: 2026-01-12T15:55:31Z
```

# Bump wasm-bindgen from 0.2.91 to 0.2.92

---

_@dependabot_

Bumps [wasm-bindgen](https://github.com/rustwasm/wasm-bindgen) from 0.2.91 to 0.2.92.
<details>
<summary>Changelog</summary>
<p><em>Sourced from <a href="https://github.com/rustwasm/wasm-bindgen/blob/main/CHANGELOG.md">wasm-bindgen's changelog</a>.</em></p>
<blockquote>
<h2><a href="https://github.com/rustwasm/wasm-bindgen/compare/0.2.91...0.2.92">0.2.92</a></h2>
<p>Released 2024-03-04</p>
<h3>Added</h3>
<ul>
<li>
<p>Add bindings for <code>RTCPeerConnectionIceErrorEvent</code>.
<a href="https://redirect.github.com/rustwasm/wasm-bindgen/pull/3835">#3835</a></p>
</li>
<li>
<p>Add bindings for <code>CanvasState.reset()</code>, affecting <code>CanvasRenderingContext2D</code> and <code>OffscreenCanvasRenderingContext2D</code>.
<a href="https://redirect.github.com/rustwasm/wasm-bindgen/pull/3844">#3844</a></p>
</li>
<li>
<p>Add <code>TryFrom</code> implementations for <code>Number</code>, that allow losslessly converting from 64- and 128-bits numbers.
<a href="https://redirect.github.com/rustwasm/wasm-bindgen/pull/3847">#3847</a></p>
</li>
<li>
<p>Add support for <code>Option&lt;*const T&gt;</code>, <code>Option&lt;*mut T&gt;</code> and <code>NonNull&lt;T&gt;</code>.
<a href="https://redirect.github.com/rustwasm/wasm-bindgen/pull/3852">#3852</a>
<a href="https://redirect.github.com/rustwasm/wasm-bindgen/pull/3857">#3857</a></p>
</li>
<li>
<p>Allow overriding the URL used for headless tests by setting <code>WASM_BINDGEN_TEST_ADDRESS</code>.
<a href="https://redirect.github.com/rustwasm/wasm-bindgen/pull/3861">#3861</a></p>
</li>
</ul>
<h3>Fixed</h3>
<ul>
<li>
<p>Make .wasm output deterministic when using <code>--reference-types</code>.
<a href="https://redirect.github.com/rustwasm/wasm-bindgen/pull/3851">#3851</a></p>
</li>
<li>
<p>Don't allow invalid Unicode scalar values in <code>char</code>.
<a href="https://redirect.github.com/rustwasm/wasm-bindgen/pull/3866">#3866</a></p>
</li>
</ul>
<hr />
</blockquote>
</details>
<details>
<summary>Commits</summary>
<ul>
<li><a href="https://github.com/rustwasm/wasm-bindgen/commit/2a4a4936238cf11e863353e410f453c94e21c7dd"><code>2a4a493</code></a> Prepare v0.2.92 release (<a href="https://redirect.github.com/rustwasm/wasm-bindgen/issues/3869">#3869</a>)</li>
<li><a href="https://github.com/rustwasm/wasm-bindgen/commit/8e992dc906f8cd5ef38479c22c64d4967d8399cf"><code>8e992dc</code></a> Don't allow invalid Unicode scalar values in <code>char</code> (<a href="https://redirect.github.com/rustwasm/wasm-bindgen/issues/3866">#3866</a>)</li>
<li><a href="https://github.com/rustwasm/wasm-bindgen/commit/807bdb48e4d6af82fdb894c0b702e869458591a8"><code>807bdb4</code></a> Revert &quot;Allow using <code>'static</code> lifetimes in functions (<a href="https://redirect.github.com/rustwasm/wasm-bindgen/issues/3856">#3856</a>)&quot; (<a href="https://redirect.github.com/rustwasm/wasm-bindgen/issues/3865">#3865</a>)</li>
<li><a href="https://github.com/rustwasm/wasm-bindgen/commit/983ec579a3dfca7ccb4f63f1f1adaa48f8bb3bec"><code>983ec57</code></a> Add <code>NonNull\&lt;T&gt;</code> as parameter (<a href="https://redirect.github.com/rustwasm/wasm-bindgen/issues/3857">#3857</a>)</li>
<li><a href="https://github.com/rustwasm/wasm-bindgen/commit/0095fa73c9c6b38e12dcbea471a431c592b9c450"><code>0095fa7</code></a> Allow overriding headless test URL (<a href="https://redirect.github.com/rustwasm/wasm-bindgen/issues/3861">#3861</a>)</li>
<li><a href="https://github.com/rustwasm/wasm-bindgen/commit/002307746ee2cbc35d9b904414c7fe6da6c78bbc"><code>0023077</code></a> Update passing-rust-closures-to-js.md (<a href="https://redirect.github.com/rustwasm/wasm-bindgen/issues/3859">#3859</a>)</li>
<li><a href="https://github.com/rustwasm/wasm-bindgen/commit/b9ccb8f3c5e0590ee9934e0a1a88dacde2c53453"><code>b9ccb8f</code></a> Allow using <code>'static</code> lifetimes in functions (<a href="https://redirect.github.com/rustwasm/wasm-bindgen/issues/3856">#3856</a>)</li>
<li><a href="https://github.com/rustwasm/wasm-bindgen/commit/c80bf9a3237a55ebffb9fd7559b7f8681ffa257f"><code>c80bf9a</code></a> Add support for <code>Option\&lt;*const T&gt;</code>, <code>Option\&lt;*mut T&gt;</code> and <code>NonNull\&lt;T&gt;</code> (<a href="https://redirect.github.com/rustwasm/wasm-bindgen/issues/3852">#3852</a>)</li>
<li><a href="https://github.com/rustwasm/wasm-bindgen/commit/0c09e154cf37b7e7f2308787d5186056cf85b533"><code>0c09e15</code></a> Fix CI (<a href="https://redirect.github.com/rustwasm/wasm-bindgen/issues/3853">#3853</a>)</li>
<li><a href="https://github.com/rustwasm/wasm-bindgen/commit/557e2e65188470e578f19a8364d7e6b23eec12fb"><code>557e2e6</code></a> Introduce impl TryFrom for Number that succeeds iff the value is within the s...</li>
<li>Additional commits viewable in <a href="https://github.com/rustwasm/wasm-bindgen/compare/0.2.91...0.2.92">compare view</a></li>
</ul>
</details>
<br />


[![Dependabot compatibility score](https://dependabot-badges.githubapp.com/badges/compatibility_score?dependency-name=wasm-bindgen&package-manager=cargo&previous-version=0.2.91&new-version=0.2.92)](https://docs.github.com/en/github/managing-security-vulnerabilities/about-dependabot-security-updates#about-compatibility-scores)

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

_Label `internal` added by @dependabot[bot] on 2024-03-11 08:28_

---

_Comment by @github-actions[bot] on 2024-03-11 08:47_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Merged by @AlexWaygood on 2024-03-11 09:29_

---

_Closed by @AlexWaygood on 2024-03-11 09:29_

---

_Branch deleted on 2024-03-11 09:29_

---
