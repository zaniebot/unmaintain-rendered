```yaml
number: 2521
title: Bump http from 0.2.12 to 1.1.0
type: pull_request
state: closed
author: dependabot
labels:
  - internal
assignees: []
base: main
head: dependabot/cargo/http-1.1.0
created_at: 2024-03-18T21:41:58Z
updated_at: 2024-03-18T21:52:19Z
url: https://github.com/astral-sh/uv/pull/2521
synced_at: 2026-01-10T14:49:08Z
```

# Bump http from 0.2.12 to 1.1.0

---

_Pull request opened by @dependabot on 2024-03-18 21:41_

Bumps [http](https://github.com/hyperium/http) from 0.2.12 to 1.1.0.
<details>
<summary>Release notes</summary>
<p><em>Sourced from <a href="https://github.com/hyperium/http/releases">http's releases</a>.</em></p>
<blockquote>
<h2>v1.1.0</h2>
<h2>What's Changed</h2>
<ul>
<li>Add methods to allow trying to allocate in the <code>HeaderMap</code>, returning an error if oversize instead of panicking.</li>
<li>Add <code>Extensions::get_or_insert()</code> method.</li>
<li>Implement <code>From&lt;Uri&gt;</code> for <code>uri::Builder</code>.</li>
<li>Fix <code>HeaderName::from_lowercase</code> that could allow NUL bytes in some cases.</li>
</ul>
<h2>New Contributors</h2>
<ul>
<li><a href="https://github.com/tottoto"><code>@​tottoto</code></a> made their first contribution in <a href="https://redirect.github.com/hyperium/http/pull/645">hyperium/http#645</a></li>
<li><a href="https://github.com/julianbraha"><code>@​julianbraha</code></a> made their first contribution in <a href="https://redirect.github.com/hyperium/http/pull/652">hyperium/http#652</a></li>
<li><a href="https://github.com/LukeMathWalker"><code>@​LukeMathWalker</code></a> made their first contribution in <a href="https://redirect.github.com/hyperium/http/pull/654">hyperium/http#654</a></li>
<li><a href="https://github.com/mattgathu"><code>@​mattgathu</code></a> made their first contribution in <a href="https://redirect.github.com/hyperium/http/pull/660">hyperium/http#660</a></li>
<li><a href="https://github.com/LukasKalbertodt"><code>@​LukasKalbertodt</code></a> made their first contribution in <a href="https://redirect.github.com/hyperium/http/pull/667">hyperium/http#667</a></li>
<li><a href="https://github.com/dswij"><code>@​dswij</code></a> made their first contribution in <a href="https://redirect.github.com/hyperium/http/pull/673">hyperium/http#673</a></li>
</ul>
<p><strong>Full Changelog</strong>: <a href="https://github.com/hyperium/http/compare/v1.0.0...v1.1.0">https://github.com/hyperium/http/compare/v1.0.0...v1.1.0</a></p>
<h2>v1.0.0</h2>
<h2>What's Changed</h2>
<ul>
<li>Implement <code>Clone</code> for <code>Request</code>, <code>Response</code>, and <code>Extensions</code>. This breaking change requires
that all extensions now implement <code>Clone</code>.</li>
<li>Add a default-on <code>std</code> feature. Disabling it currently is not supported.</li>
<li>Fix MIRI warnings in <code>HeaderMap::iter()</code>.</li>
</ul>
<h2>New Contributors</h2>
<ul>
<li><a href="https://github.com/hjr3"><code>@​hjr3</code></a> made their first contribution in <a href="https://redirect.github.com/hyperium/http/pull/644">hyperium/http#644</a></li>
</ul>
</blockquote>
</details>
<details>
<summary>Changelog</summary>
<p><em>Sourced from <a href="https://github.com/hyperium/http/blob/master/CHANGELOG.md">http's changelog</a>.</em></p>
<blockquote>
<h1>1.1.0 (March 4, 2024)</h1>
<ul>
<li>Add methods to allow trying to allocate in the <code>HeaderMap</code>, returning an error if oversize instead of panicking.</li>
<li>Add <code>Extensions::get_or_insert()</code> method.</li>
<li>Implement <code>From&lt;Uri&gt;</code> for <code>uri::Builder</code>.</li>
<li>Fix <code>HeaderName::from_lowercase</code> that could allow NUL bytes in some cases.</li>
</ul>
<h1>1.0.0 (November 15, 2023)</h1>
<ul>
<li>Implement <code>Clone</code> for <code>Request</code>, <code>Response</code>, and <code>Extensions</code>. This breaking change requires
that all extensions now implement <code>Clone</code>.</li>
<li>Add a default-on <code>std</code> feature. Disabling it currently is not supported.</li>
<li>Fix MIRI warnings in <code>HeaderMap::iter()</code>.</li>
</ul>
<h1>0.2.10 (November 10, 2023)</h1>
<ul>
<li>Fix parsing of <code>Authority</code> to handle square brackets in incorrect order.</li>
<li>Fix <code>HeaderMap::with_capacity()</code> to handle arithmetic overflow.</li>
</ul>
<h1>0.2.9 (February 17, 2023)</h1>
<ul>
<li>Add <code>HeaderName</code> constants for <code>cache-status</code> and <code>cdn-cache-control</code>.</li>
<li>Implement <code>Hash</code> for <code>PathAndQuery</code>.</li>
<li>Re-export <code>HeaderName</code> at crate root.</li>
</ul>
<h1>0.2.8 (June 6, 2022)</h1>
<ul>
<li>Fix internal usage of uninitialized memory to use <code>MaybeUninit</code> inside <code>HeaderName</code>.</li>
</ul>
<h1>0.2.7 (April 28, 2022)</h1>
<ul>
<li>MSRV bumped to <code>1.49</code>.</li>
<li>Add <code>extend()</code> method to <code>Extensions</code>.</li>
<li>Add <code>From&lt;Authority&gt;</code> and <code>From&lt;PathAndQuery&gt;</code> impls for <code>Uri</code>.</li>
<li>Make <code>HeaderName::from_static</code> a <code>const fn</code>.</li>
</ul>
<h1>0.2.6 (December 30, 2021)</h1>
<ul>
<li>Upgrade internal <code>itoa</code> dependency to 1.0.</li>
</ul>
<h1>0.2.5 (September 21, 2021)</h1>
<ul>
<li>Add <code>is_empty()</code> and <code>len()</code> methods to <code>Extensions</code>.</li>
<li>Add <code>version_ref()</code> method to <code>request::Builder</code>.</li>
<li>Implement <code>TryFrom&lt;Vec&lt;u8&gt;&gt;</code> and <code>TryFrom&lt;String&gt;</code> for <code>Authority</code>, <code>Uri</code>, <code>PathAndQuery</code>, and <code>HeaderName</code>.</li>
<li>Make <code>HeaderValue::from_static</code> a <code>const fn</code>.</li>
</ul>
<h1>0.2.4 (April 4, 2021)</h1>
<ul>
<li>Fix <code>Uri</code> parsing to allow <code>{</code>, <code>&quot;</code>, and <code>}</code> in paths.</li>
</ul>
<!-- raw HTML omitted -->
</blockquote>
<p>... (truncated)</p>
</details>
<details>
<summary>Commits</summary>
<ul>
<li><a href="https://github.com/hyperium/http/commit/3fe7267b8c301dbe034749f0f31f48957ba61b3c"><code>3fe7267</code></a> v1.1.0</li>
<li><a href="https://github.com/hyperium/http/commit/96dc52f513336285c572fe1509ab637261cfec9b"><code>96dc52f</code></a> fix: HeaderName::from_lowercase allowing NUL bytes in some cases</li>
<li><a href="https://github.com/hyperium/http/commit/caa8b4f0a4d158a2e1933f3fc591d1a6cdd2814c"><code>caa8b4f</code></a> feat: add <code>HeaderMap::try_</code> methods to handle capacity overflow</li>
<li><a href="https://github.com/hyperium/http/commit/63102bcd29fcd4a094cac6a4afb0af370ef1fcbe"><code>63102bc</code></a> chore(lib): remove importing prelude AsRef trait</li>
<li><a href="https://github.com/hyperium/http/commit/c03cc8b5c57251ffc8eac4807846a5b3f375e811"><code>c03cc8b</code></a> chore(header): allow clippy::should_implement_trait rule for HeaderValue::fro...</li>
<li><a href="https://github.com/hyperium/http/commit/4785cdd6b7ab50c1343ac54d867bbadf8aa37be1"><code>4785cdd</code></a> refactor(header): rename method to follow naming convention</li>
<li><a href="https://github.com/hyperium/http/commit/63e7d630f0be16664eb7faaf3688bd8a7de52251"><code>63e7d63</code></a> doc(header): add panics and safety section to document</li>
<li><a href="https://github.com/hyperium/http/commit/b8ddea7d21a415918ba3d1d40984ef5f1f3658e6"><code>b8ddea7</code></a> refactor(header): add comment and lint allowing to panic in const context wor...</li>
<li><a href="https://github.com/hyperium/http/commit/fe1932d45c91101b5dd8c9546b4b0bb72116511e"><code>fe1932d</code></a> refactor(status): remove redundant static lifetime</li>
<li><a href="https://github.com/hyperium/http/commit/79f8da59f607685fa1bb0998ceb64b7be225dd95"><code>79f8da5</code></a> refactor(header): ownership is not needed to iterate</li>
<li>Additional commits viewable in <a href="https://github.com/hyperium/http/compare/v0.2.12...v1.1.0">compare view</a></li>
</ul>
</details>
<br />


[![Dependabot compatibility score](https://dependabot-badges.githubapp.com/badges/compatibility_score?dependency-name=http&package-manager=cargo&previous-version=0.2.12&new-version=1.1.0)](https://docs.github.com/en/github/managing-security-vulnerabilities/about-dependabot-security-updates#about-compatibility-scores)

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

_Label `internal` added by @dependabot[bot] on 2024-03-18 21:41_

---

_Comment by @charliermarsh on 2024-03-18 21:52_

We need to stay in-sync with reqwest.

@dependabot ignore this minor version



---

_Closed by @charliermarsh on 2024-03-18 21:52_

---

_Comment by @dependabot[bot] on 2024-03-18 21:52_

OK, I won't notify you about version 1.1.x again, unless you re-open this PR.

---

_Branch deleted on 2024-03-18 21:52_

---
