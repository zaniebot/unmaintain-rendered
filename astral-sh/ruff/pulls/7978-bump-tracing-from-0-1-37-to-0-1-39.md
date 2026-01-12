```yaml
number: 7978
title: Bump tracing from 0.1.37 to 0.1.39
type: pull_request
state: merged
author: dependabot
labels:
  - internal
assignees: []
merged: true
base: main
head: dependabot/cargo/tracing-0.1.39
created_at: 2023-10-16T08:34:01Z
updated_at: 2023-10-17T07:53:17Z
url: https://github.com/astral-sh/ruff/pull/7978
synced_at: 2026-01-12T02:32:41Z
```

# Bump tracing from 0.1.37 to 0.1.39

---

_Pull request opened by @dependabot on 2023-10-16 08:34_

Bumps [tracing](https://github.com/tokio-rs/tracing) from 0.1.37 to 0.1.39.
<details>
<summary>Release notes</summary>
<p><em>Sourced from <a href="https://github.com/tokio-rs/tracing/releases">tracing's releases</a>.</em></p>
<blockquote>
<h2>tracing 0.1.39</h2>
<p>This release adds several additional features to the <code>tracing</code> macros. In
addition, it updates the <code>tracing-core</code> dependency to [v0.1.32][core-0.1.32] and
the <code>tracing-attributes</code> dependency to [v0.1.27][attrs-0.1.27].</p>
<h3>Added</h3>
<ul>
<li>Allow constant field names in macros (<a href="https://redirect.github.com/tokio-rs/tracing/issues/2617">#2617</a>)</li>
<li>Allow setting event names in macros (<a href="https://redirect.github.com/tokio-rs/tracing/issues/2699">#2699</a>)</li>
<li><strong>core</strong>: Allow <code>ValueSet</code>s of any length (<a href="https://redirect.github.com/tokio-rs/tracing/issues/2508">#2508</a>)</li>
</ul>
<h3>Changed</h3>
<ul>
<li><code>tracing-attributes</code>: updated to [0.1.27][attrs-0.1.27]</li>
<li><code>tracing-core</code>: updated to [0.1.32][core-0.1.32]</li>
<li><strong>attributes</strong>: Bump minimum version of proc-macro2 to 1.0.60 (<a href="https://redirect.github.com/tokio-rs/tracing/issues/2732">#2732</a>)</li>
<li><strong>attributes</strong>: Generate less dead code for async block return type hint (<a href="https://redirect.github.com/tokio-rs/tracing/issues/2709">#2709</a>)</li>
</ul>
<h3>Fixed</h3>
<ul>
<li>Use fully qualified names in macros for items exported from std prelude
(<a href="https://redirect.github.com/tokio-rs/tracing/issues/2621">#2621</a>, <a href="https://redirect.github.com/tokio-rs/tracing/issues/2757">#2757</a>)</li>
<li><strong>attributes</strong>: Allow [<code>clippy::let_with_type_underscore</code>] in macro-generated
code (<a href="https://redirect.github.com/tokio-rs/tracing/issues/2609">#2609</a>)</li>
<li><strong>attributes</strong>: Allow <code>unknown_lints</code> in macro-generated code (<a href="https://redirect.github.com/tokio-rs/tracing/issues/2626">#2626</a>)</li>
<li><strong>attributes</strong>: Fix a compilation error in <code>#[instrument]</code> when the <code>&quot;log&quot;</code>
feature is enabled (<a href="https://redirect.github.com/tokio-rs/tracing/issues/2599">#2599</a>)</li>
</ul>
<h3>Documented</h3>
<ul>
<li>Add <code>axum-insights</code> to relevant crates. (<a href="https://redirect.github.com/tokio-rs/tracing/issues/2713">#2713</a>)</li>
<li>Fix link to RAI pattern crate documentation (<a href="https://redirect.github.com/tokio-rs/tracing/issues/2612">#2612</a>)</li>
<li>Fix docs typos and warnings (<a href="https://redirect.github.com/tokio-rs/tracing/issues/2581">#2581</a>)</li>
<li>Add <code>clippy-tracing</code> to related crates (<a href="https://redirect.github.com/tokio-rs/tracing/issues/2628">#2628</a>)</li>
<li>Add <code>tracing-cloudwatch</code> to related crates (<a href="https://redirect.github.com/tokio-rs/tracing/issues/2667">#2667</a>)</li>
<li>Fix deadlink to <code>tracing-etw</code> repo (<a href="https://redirect.github.com/tokio-rs/tracing/issues/2602">#2602</a>)</li>
</ul>
<p><a href="https://redirect.github.com/tokio-rs/tracing/issues/2617">#2617</a>: <a href="https://redirect.github.com/tokio-rs/tracing/pull/2617">tokio-rs/tracing#2617</a>
<a href="https://redirect.github.com/tokio-rs/tracing/issues/2699">#2699</a>: <a href="https://redirect.github.com/tokio-rs/tracing/pull/2699">tokio-rs/tracing#2699</a>
<a href="https://redirect.github.com/tokio-rs/tracing/issues/2508">#2508</a>: <a href="https://redirect.github.com/tokio-rs/tracing/pull/2508">tokio-rs/tracing#2508</a>
<a href="https://redirect.github.com/tokio-rs/tracing/issues/2621">#2621</a>: <a href="https://redirect.github.com/tokio-rs/tracing/pull/2621">tokio-rs/tracing#2621</a>
<a href="https://redirect.github.com/tokio-rs/tracing/issues/2713">#2713</a>: <a href="https://redirect.github.com/tokio-rs/tracing/pull/2713">tokio-rs/tracing#2713</a>
<a href="https://redirect.github.com/tokio-rs/tracing/issues/2581">#2581</a>: <a href="https://redirect.github.com/tokio-rs/tracing/pull/2581">tokio-rs/tracing#2581</a>
<a href="https://redirect.github.com/tokio-rs/tracing/issues/2628">#2628</a>: <a href="https://redirect.github.com/tokio-rs/tracing/pull/2628">tokio-rs/tracing#2628</a>
<a href="https://redirect.github.com/tokio-rs/tracing/issues/2667">#2667</a>: <a href="https://redirect.github.com/tokio-rs/tracing/pull/2667">tokio-rs/tracing#2667</a>
<a href="https://redirect.github.com/tokio-rs/tracing/issues/2602">#2602</a>: <a href="https://redirect.github.com/tokio-rs/tracing/pull/2602">tokio-rs/tracing#2602</a>
<a href="https://redirect.github.com/tokio-rs/tracing/issues/2626">#2626</a>: <a href="https://redirect.github.com/tokio-rs/tracing/pull/2626">tokio-rs/tracing#2626</a>
<a href="https://redirect.github.com/tokio-rs/tracing/issues/2757">#2757</a>: <a href="https://redirect.github.com/tokio-rs/tracing/pull/2757">tokio-rs/tracing#2757</a>
<a href="https://redirect.github.com/tokio-rs/tracing/issues/2732">#2732</a>: <a href="https://redirect.github.com/tokio-rs/tracing/pull/2732">tokio-rs/tracing#2732</a>
<a href="https://redirect.github.com/tokio-rs/tracing/issues/2709">#2709</a>: <a href="https://redirect.github.com/tokio-rs/tracing/pull/2709">tokio-rs/tracing#2709</a></p>
<!-- raw HTML omitted -->
</blockquote>
<p>... (truncated)</p>
</details>
<details>
<summary>Commits</summary>
<ul>
<li><a href="https://github.com/tokio-rs/tracing/commit/4b99457c876d7a9d1865b4d3f5cefb87cd522750"><code>4b99457</code></a> chore: prepare tracing 0.1.39 (<a href="https://redirect.github.com/tokio-rs/tracing/issues/2755">#2755</a>)</li>
<li><a href="https://github.com/tokio-rs/tracing/commit/b2a5e11e242e53e96adec1023687760140f7e65c"><code>b2a5e11</code></a> tracing: update core to v0.1.31 and attributes to v0.1.27</li>
<li><a href="https://github.com/tokio-rs/tracing/commit/3825a50c1afbbbebf4339c947f489b0f923dd450"><code>3825a50</code></a> tracing: use full path when calling <code>format_args!</code> (<a href="https://redirect.github.com/tokio-rs/tracing/issues/2757">#2757</a>)</li>
<li><a href="https://github.com/tokio-rs/tracing/commit/c4b2a56937dd40aaa2e2991636eca6748353201f"><code>c4b2a56</code></a> chore: prepare tracing-core 0.1.32 (<a href="https://redirect.github.com/tokio-rs/tracing/issues/2754">#2754</a>)</li>
<li><a href="https://github.com/tokio-rs/tracing/commit/2502f19d934b092fc01bb7493eacbb16b4038bf3"><code>2502f19</code></a> chore: prepare tracing-attributes 0.1.27 (<a href="https://redirect.github.com/tokio-rs/tracing/issues/2756">#2756</a>)</li>
<li><a href="https://github.com/tokio-rs/tracing/commit/90487620d8fd4b0a44e6a0385bda3643bf6f19a2"><code>9048762</code></a> Revert &quot;log: update to env_logger 0.10 to fix GHSA-g98v-hv3f-hcfr (<a href="https://redirect.github.com/tokio-rs/tracing/issues/2740">#2740</a>)&quot; (#...</li>
<li><a href="https://github.com/tokio-rs/tracing/commit/6ba5af2ce2a814fea521d2d0a5ea1e88d7b6011e"><code>6ba5af2</code></a> docs: remove mention of <code>Registration</code> on v0.1.x (<a href="https://redirect.github.com/tokio-rs/tracing/issues/2753">#2753</a>)</li>
<li><a href="https://github.com/tokio-rs/tracing/commit/11aac9a07c4aac35b12ea82d8a5b1cb00a118928"><code>11aac9a</code></a> log: deprecate <code>env_logger</code> in favor of <code>tracing_subscriber::fmt::Subscriber</code>...</li>
<li><a href="https://github.com/tokio-rs/tracing/commit/2f27752a9912f03ed64e2b29e1ea06ebb4b09e83"><code>2f27752</code></a> chore: remove <code>env_logger</code> from <code>hyper</code> example</li>
<li><a href="https://github.com/tokio-rs/tracing/commit/f96846d78a3f240a2c81636a7d6921e5d74ac79f"><code>f96846d</code></a> attributes: fix typo &quot;overriden&quot; =&gt; &quot;overridden&quot; (<a href="https://redirect.github.com/tokio-rs/tracing/issues/2719">#2719</a>)</li>
<li>Additional commits viewable in <a href="https://github.com/tokio-rs/tracing/compare/tracing-0.1.37...tracing-0.1.39">compare view</a></li>
</ul>
</details>
<br />


[![Dependabot compatibility score](https://dependabot-badges.githubapp.com/badges/compatibility_score?dependency-name=tracing&package-manager=cargo&previous-version=0.1.37&new-version=0.1.39)](https://docs.github.com/en/github/managing-security-vulnerabilities/about-dependabot-security-updates#about-compatibility-scores)

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

_Label `internal` added by @dependabot[bot] on 2023-10-16 08:34_

---

_Merged by @MichaReiser on 2023-10-17 07:46_

---

_Closed by @MichaReiser on 2023-10-17 07:46_

---

_Branch deleted on 2023-10-17 07:46_

---

_Comment by @github-actions[bot] on 2023-10-17 07:53_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.



---
