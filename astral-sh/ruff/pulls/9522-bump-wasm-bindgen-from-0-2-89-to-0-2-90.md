```yaml
number: 9522
title: Bump wasm-bindgen from 0.2.89 to 0.2.90
type: pull_request
state: closed
author: dependabot
labels:
  - internal
assignees: []
base: main
head: dependabot/cargo/wasm-bindgen-0.2.90
created_at: 2024-01-15T08:27:27Z
updated_at: 2024-01-15T09:47:40Z
url: https://github.com/astral-sh/ruff/pull/9522
synced_at: 2026-01-12T15:55:29Z
```

# Bump wasm-bindgen from 0.2.89 to 0.2.90

---

_@dependabot_

Bumps [wasm-bindgen](https://github.com/rustwasm/wasm-bindgen) from 0.2.89 to 0.2.90.
<details>
<summary>Changelog</summary>
<p><em>Sourced from <a href="https://github.com/rustwasm/wasm-bindgen/blob/main/CHANGELOG.md">wasm-bindgen's changelog</a>.</em></p>
<blockquote>
<h2><a href="https://github.com/rustwasm/wasm-bindgen/compare/0.2.89...0.2.90">0.2.90</a></h2>
<p>Released 2024-01-06</p>
<h3>Fixed</h3>
<ul>
<li>Fix JS shim default path detection for the no-modules target.
<a href="https://redirect.github.com/rustwasm/wasm-bindgen/pull/3748">#3748</a></li>
</ul>
<h3>Added</h3>
<ul>
<li>
<p>Add bindings for <code>HTMLFormElement.requestSubmit()</code>.
<a href="https://redirect.github.com/rustwasm/wasm-bindgen/pull/3747">#3747</a></p>
</li>
<li>
<p>Add bindings for <code>RTCRtpSender.getCapabilities(DOMString)</code> method, <code>RTCRtpCapabilities</code>, <code>RTCRtpCodecCapability</code> and <code>RTCRtpHeaderExtensionCapability</code>.
<a href="https://redirect.github.com/rustwasm/wasm-bindgen/pull/3737">#3737</a></p>
</li>
<li>
<p>Add bindings for <code>UserActivation</code>.
<a href="https://redirect.github.com/rustwasm/wasm-bindgen/pull/3719">#3719</a></p>
</li>
<li>
<p>Add unstable bindings for the Compression Streams API.
<a href="https://redirect.github.com/rustwasm/wasm-bindgen/pull/3752">#3752</a></p>
</li>
</ul>
<h3>Changed</h3>
<ul>
<li>
<p>Stabilize File System API.
<a href="https://redirect.github.com/rustwasm/wasm-bindgen/pull/3745">#3745</a></p>
</li>
<li>
<p>Stabilize <code>QueuingStrategy</code>.
<a href="https://redirect.github.com/rustwasm/wasm-bindgen/pull/3753">#3753</a></p>
</li>
</ul>
<h3>Fixed</h3>
<ul>
<li>Fixed a compiler error when using <code>#[wasm_bindgen]</code> inside <code>macro_rules!</code>.
<a href="https://redirect.github.com/rustwasm/wasm-bindgen/pull/3725">#3725</a></li>
</ul>
<h3>Removed</h3>
<ul>
<li>Removed Gecko-only <code>InstallTriggerData</code> and Gecko-internal <code>FlexLineGrowthState</code>, <code>GridDeclaration</code>, <code>GridTrackState</code>,
<code>RtcLifecycleEvent</code> and <code>WebrtcGlobalStatisticsReport</code> features.
<a href="https://redirect.github.com/rustwasm/wasm-bindgen/pull/3723">#3723</a></li>
</ul>
</blockquote>
</details>
<details>
<summary>Commits</summary>
<ul>
<li><a href="https://github.com/rustwasm/wasm-bindgen/commit/adcf7786d14943244cbe418a2a17d00430008e2e"><code>adcf778</code></a> Bump versions &amp; update changelog for 0.2.90 release (<a href="https://redirect.github.com/rustwasm/wasm-bindgen/issues/3763">#3763</a>)</li>
<li><a href="https://github.com/rustwasm/wasm-bindgen/commit/a3332e4be6ae3339dd77f37257e3d4d170085099"><code>a3332e4</code></a> CI fix for Rust 1.75 (<a href="https://redirect.github.com/rustwasm/wasm-bindgen/issues/3761">#3761</a>)</li>
<li><a href="https://github.com/rustwasm/wasm-bindgen/commit/c0ee195d7ebbb18b8517d115c4f3bfee4b797a3d"><code>c0ee195</code></a> Stabilize <code>QueuingStrategy</code> (<a href="https://redirect.github.com/rustwasm/wasm-bindgen/issues/3753">#3753</a>)</li>
<li><a href="https://github.com/rustwasm/wasm-bindgen/commit/0069b66e16971116102a2867d56b94224a540f85"><code>0069b66</code></a> Add unstable bindings for the Compression Streams API (<a href="https://redirect.github.com/rustwasm/wasm-bindgen/issues/3752">#3752</a>)</li>
<li><a href="https://github.com/rustwasm/wasm-bindgen/commit/a0707157daf808557aa666b3f349b2ce024ccae5"><code>a070715</code></a> Add bindings for <code>HTMLFormElement.requestSubmit()</code> (<a href="https://redirect.github.com/rustwasm/wasm-bindgen/issues/3747">#3747</a>)</li>
<li><a href="https://github.com/rustwasm/wasm-bindgen/commit/cc9a2195aca533f0bb701e6fb888dca3517470a3"><code>cc9a219</code></a> Fix JS shim default path detection for the no-modules target (<a href="https://redirect.github.com/rustwasm/wasm-bindgen/issues/3748">#3748</a>)</li>
<li><a href="https://github.com/rustwasm/wasm-bindgen/commit/185e9db4d3a61e2917347036c52c28b578f66047"><code>185e9db</code></a> Stabilize File System API (<a href="https://redirect.github.com/rustwasm/wasm-bindgen/issues/3745">#3745</a>)</li>
<li><a href="https://github.com/rustwasm/wasm-bindgen/commit/bb9456735eaa119d0e092aae877bdef59c117d68"><code>bb94567</code></a> Point to component model instead of host bindings (<a href="https://redirect.github.com/rustwasm/wasm-bindgen/issues/3746">#3746</a>)</li>
<li><a href="https://github.com/rustwasm/wasm-bindgen/commit/a5d2539314e0392c87a4d3203e99cf4888050fc0"><code>a5d2539</code></a> Implement <code>RTCRtpSender.getCapabilities</code> method (<a href="https://redirect.github.com/rustwasm/wasm-bindgen/issues/3737">#3737</a>)</li>
<li><a href="https://github.com/rustwasm/wasm-bindgen/commit/cfe3dc28a101a1b4cf7ab7ca6db358eadc7a4063"><code>cfe3dc2</code></a> Add a test runner to two of the typescript tests to actually run them (<a href="https://redirect.github.com/rustwasm/wasm-bindgen/issues/3704">#3704</a>)</li>
<li>Additional commits viewable in <a href="https://github.com/rustwasm/wasm-bindgen/compare/0.2.89...0.2.90">compare view</a></li>
</ul>
</details>
<br />


[![Dependabot compatibility score](https://dependabot-badges.githubapp.com/badges/compatibility_score?dependency-name=wasm-bindgen&package-manager=cargo&previous-version=0.2.89&new-version=0.2.90)](https://docs.github.com/en/github/managing-security-vulnerabilities/about-dependabot-security-updates#about-compatibility-scores)

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

_Label `internal` added by @dependabot[bot] on 2024-01-15 08:27_

---

_Comment by @codspeed-hq[bot] on 2024-01-15 08:35_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/dependabot/cargo/wasm-bindgen-0.2.90)

### Merging #9522 will **not alter performance**

<sub>Comparing <code>dependabot/cargo/wasm-bindgen-0.2.90</code> (abd04ed) with <code>main</code> (1574030)</sub>



### Summary

`✅ 30` untouched benchmarks






---

_Comment by @github-actions[bot] on 2024-01-15 08:46_

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

_Comment by @dependabot[bot] on 2024-01-15 09:31_

Looks like wasm-bindgen is up-to-date now, so this is no longer needed.

---

_Closed by @dependabot[bot] on 2024-01-15 09:31_

---

_Branch deleted on 2024-01-15 09:31_

---
