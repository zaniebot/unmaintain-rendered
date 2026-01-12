```yaml
number: 7864
title: Bump proc-macro2 from 1.0.67 to 1.0.69
type: pull_request
state: merged
author: dependabot
labels:
  - internal
assignees: []
merged: true
base: main
head: dependabot/cargo/proc-macro2-1.0.69
created_at: 2023-10-09T08:30:56Z
updated_at: 2023-10-09T11:44:22Z
url: https://github.com/astral-sh/ruff/pull/7864
synced_at: 2026-01-12T02:32:41Z
```

# Bump proc-macro2 from 1.0.67 to 1.0.69

---

_Pull request opened by @dependabot on 2023-10-09 08:30_

Bumps [proc-macro2](https://github.com/dtolnay/proc-macro2) from 1.0.67 to 1.0.69.
<details>
<summary>Release notes</summary>
<p><em>Sourced from <a href="https://github.com/dtolnay/proc-macro2/releases">proc-macro2's releases</a>.</em></p>
<blockquote>
<h2>1.0.69</h2>
<ul>
<li>Fix Span::source_text() bug causing panics or incorrect source text (<a href="https://redirect.github.com/dtolnay/proc-macro2/issues/410">#410</a>)</li>
</ul>
<h2>1.0.68</h2>
<ul>
<li>Fix panic in Span::source_text() when source contains multibyte characters (<a href="https://redirect.github.com/dtolnay/proc-macro2/issues/408">#408</a>)</li>
</ul>
</blockquote>
</details>
<details>
<summary>Commits</summary>
<ul>
<li><a href="https://github.com/dtolnay/proc-macro2/commit/937bbcdcc15ef7bc7ee90813dc72093a2beb30c5"><code>937bbcd</code></a> Release 1.0.69</li>
<li><a href="https://github.com/dtolnay/proc-macro2/commit/42dc36efcefbccb95b757f1e95bace944df0c9ed"><code>42dc36e</code></a> Merge pull request <a href="https://redirect.github.com/dtolnay/proc-macro2/issues/412">#412</a> from dtolnay/sourcetext</li>
<li><a href="https://github.com/dtolnay/proc-macro2/commit/6461c2dd607aabec87d88144a7ffe7e00a2b2991"><code>6461c2d</code></a> Add out-of-order call to source_text test</li>
<li><a href="https://github.com/dtolnay/proc-macro2/commit/c4c3251c57747263f48a38ac8fb5a76a17f8fa8c"><code>c4c3251</code></a> Explain source_text implementation approach</li>
<li><a href="https://github.com/dtolnay/proc-macro2/commit/31b14c30f290d0e46942b25a24fe2a1ce59f9a82"><code>31b14c3</code></a> Cache byte offsets computed from a char index</li>
<li><a href="https://github.com/dtolnay/proc-macro2/commit/0e154618a1e89331044803b0730543e23ff80c37"><code>0e15461</code></a> Make FileInfo mut in source_text to allow amortization of char indices</li>
<li><a href="https://github.com/dtolnay/proc-macro2/commit/90b8e1eb016b72d197d58cbdf6f981f97e211587"><code>90b8e1e</code></a> Merge pull request <a href="https://redirect.github.com/dtolnay/proc-macro2/issues/411">#411</a> from dtolnay/sourcetext</li>
<li><a href="https://github.com/dtolnay/proc-macro2/commit/137ae0a341be5cb04be12f251f48911437b566cd"><code>137ae0a</code></a> Fix source_text treating span.lo as byte offset not char index</li>
<li><a href="https://github.com/dtolnay/proc-macro2/commit/4c0bd28a61d911a179be86e8e82a1501853406a4"><code>4c0bd28</code></a> Add regression test for issue 410</li>
<li><a href="https://github.com/dtolnay/proc-macro2/commit/12eddc03a40e8393e82f7ef1dadbaaabdcb00a08"><code>12eddc0</code></a> Reword explanation of SourceMap initial value</li>
<li>Additional commits viewable in <a href="https://github.com/dtolnay/proc-macro2/compare/1.0.67...1.0.69">compare view</a></li>
</ul>
</details>
<br />


[![Dependabot compatibility score](https://dependabot-badges.githubapp.com/badges/compatibility_score?dependency-name=proc-macro2&package-manager=cargo&previous-version=1.0.67&new-version=1.0.69)](https://docs.github.com/en/github/managing-security-vulnerabilities/about-dependabot-security-updates#about-compatibility-scores)

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

_Label `internal` added by @dependabot[bot] on 2023-10-09 08:30_

---

_Comment by @github-actions[bot] on 2023-10-09 08:52_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.



---

_Merged by @charliermarsh on 2023-10-09 11:44_

---

_Closed by @charliermarsh on 2023-10-09 11:44_

---

_Branch deleted on 2023-10-09 11:44_

---
