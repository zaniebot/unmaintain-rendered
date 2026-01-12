```yaml
number: 7861
title: Bump regex from 1.9.5 to 1.9.6
type: pull_request
state: merged
author: dependabot
labels:
  - internal
assignees: []
merged: true
base: main
head: dependabot/cargo/regex-1.9.6
created_at: 2023-10-09T08:29:10Z
updated_at: 2023-10-09T11:44:36Z
url: https://github.com/astral-sh/ruff/pull/7861
synced_at: 2026-01-12T02:32:41Z
```

# Bump regex from 1.9.5 to 1.9.6

---

_Pull request opened by @dependabot on 2023-10-09 08:29_

Bumps [regex](https://github.com/rust-lang/regex) from 1.9.5 to 1.9.6.
<details>
<summary>Changelog</summary>
<p><em>Sourced from <a href="https://github.com/rust-lang/regex/blob/master/CHANGELOG.md">regex's changelog</a>.</em></p>
<blockquote>
<h1>1.9.6 (2023-09-30)</h1>
<p>This is a patch release that fixes a panic that can occur when the default
regex size limit is increased to a large number.</p>
<ul>
<li><a href="https://github.com/rust-lang/regex/commit/aa4e4c7120b0090ce0624e3c42a2ed06dd8b918a">BUG aa4e4c71</a>:
Fix a bug where computing the maximum haystack length for the bounded
backtracker could result underflow and thus provoke a panic later in a search
due to a broken invariant.</li>
</ul>
</blockquote>
</details>
<details>
<summary>Commits</summary>
<ul>
<li><a href="https://github.com/rust-lang/regex/commit/11b44439786499014f61afe6e294650fb01550be"><code>11b4443</code></a> 1.9.6</li>
<li><a href="https://github.com/rust-lang/regex/commit/3dda4255e11ddb9257f6b75135bb2f3f8a554acb"><code>3dda425</code></a> deps: bump regex-automata to 0.3.9</li>
<li><a href="https://github.com/rust-lang/regex/commit/03f00bd756d85ee21714136e46836c4a5ad1b99c"><code>03f00bd</code></a> regex-automata-0.3.9</li>
<li><a href="https://github.com/rust-lang/regex/commit/e4674083346283cdf24fdc211dc44a4a6f6846b1"><code>e467408</code></a> changelog: 1.9.6</li>
<li><a href="https://github.com/rust-lang/regex/commit/aa4e4c7120b0090ce0624e3c42a2ed06dd8b918a"><code>aa4e4c7</code></a> automata: fix unintended panic in max_haystack_len</li>
<li><a href="https://github.com/rust-lang/regex/commit/27a25385c0bd1228716271668febc88bd8c74932"><code>27a2538</code></a> automata: add some #[inline] annotations</li>
<li><a href="https://github.com/rust-lang/regex/commit/061ee815ef2c44101dba7b0b124600fcb03c1912"><code>061ee81</code></a> readme: visually emphasize performance criteria difference</li>
<li><a href="https://github.com/rust-lang/regex/commit/8275c1b3bef014a4393d9975285757f89d7e4592"><code>8275c1b</code></a> doc: fix a few typos</li>
<li><a href="https://github.com/rust-lang/regex/commit/cdc0dbd3547462aedb6235197c2b743ec4ea75e5"><code>cdc0dbd</code></a> readme: add section about performance and benchmarks</li>
<li><a href="https://github.com/rust-lang/regex/commit/4aaf3896ef1147000a5e63f174fa49bfa5d18d65"><code>4aaf389</code></a> ci: pin to memchr 2.6.2 for MSRV CI job</li>
<li>See full diff in <a href="https://github.com/rust-lang/regex/compare/1.9.5...1.9.6">compare view</a></li>
</ul>
</details>
<br />


[![Dependabot compatibility score](https://dependabot-badges.githubapp.com/badges/compatibility_score?dependency-name=regex&package-manager=cargo&previous-version=1.9.5&new-version=1.9.6)](https://docs.github.com/en/github/managing-security-vulnerabilities/about-dependabot-security-updates#about-compatibility-scores)

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

_Label `internal` added by @dependabot[bot] on 2023-10-09 08:29_

---

_Comment by @github-actions[bot] on 2023-10-09 08:47_

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
