```yaml
number: 9738
title: Bump proc-macro2 from 1.0.76 to 1.0.78
type: pull_request
state: merged
author: dependabot
labels:
  - internal
assignees: []
merged: true
base: main
head: dependabot/cargo/proc-macro2-1.0.78
created_at: 2024-01-31T15:29:35Z
updated_at: 2024-01-31T15:48:38Z
url: https://github.com/astral-sh/ruff/pull/9738
synced_at: 2026-01-10T22:57:09Z
```

# Bump proc-macro2 from 1.0.76 to 1.0.78

---

_Pull request opened by @dependabot on 2024-01-31 15:29_

Bumps [proc-macro2](https://github.com/dtolnay/proc-macro2) from 1.0.76 to 1.0.78.
<details>
<summary>Release notes</summary>
<p><em>Sourced from <a href="https://github.com/dtolnay/proc-macro2/releases">proc-macro2's releases</a>.</em></p>
<blockquote>
<h2>1.0.78</h2>
<ul>
<li>Expose <a href="https://doc.rust-lang.org/1.71.0/proc_macro/struct.Span.html#method.byte_range">Span::byte_range</a> (<a href="https://redirect.github.com/dtolnay/proc-macro2/issues/442">#442</a>)</li>
</ul>
<h2>1.0.77</h2>
<ul>
<li>Add a function to reset thread-local span data (<a href="https://redirect.github.com/dtolnay/proc-macro2/issues/438">#438</a>, thanks <a href="https://github.com/buchgr"><code>@​buchgr</code></a>)</li>
</ul>
</blockquote>
</details>
<details>
<summary>Commits</summary>
<ul>
<li><a href="https://github.com/dtolnay/proc-macro2/commit/d850a1db5e3fe7732d62cacbfc4145e496c2a80e"><code>d850a1d</code></a> Release 1.0.78</li>
<li><a href="https://github.com/dtolnay/proc-macro2/commit/6cefaec285bf51ad9476a52e1676ef698fe2ba9c"><code>6cefaec</code></a> Merge pull request <a href="https://redirect.github.com/dtolnay/proc-macro2/issues/442">#442</a> from dtolnay/byterange</li>
<li><a href="https://github.com/dtolnay/proc-macro2/commit/10827679cdb6c7cbcade2d90cbebbb77484a0570"><code>1082767</code></a> Expose Span::byte_range</li>
<li><a href="https://github.com/dtolnay/proc-macro2/commit/da1be4d1aea56dcfd5e515b7bded9283323320f5"><code>da1be4d</code></a> Release 1.0.77</li>
<li><a href="https://github.com/dtolnay/proc-macro2/commit/69fee37f91d24bc662ff46dad87dbc4c35991698"><code>69fee37</code></a> Merge pull request <a href="https://redirect.github.com/dtolnay/proc-macro2/issues/440">#440</a> from dtolnay/example</li>
<li><a href="https://github.com/dtolnay/proc-macro2/commit/66a3ef00b8cd3ba934f3e43720413aa927c018e7"><code>66a3ef0</code></a> Raise minimum tested compiler to 1.63</li>
<li><a href="https://github.com/dtolnay/proc-macro2/commit/d2441c39a8af23f605cb4fe917e2c2884262503f"><code>d2441c3</code></a> Add example code for invalidate_current_thread_spans</li>
<li><a href="https://github.com/dtolnay/proc-macro2/commit/43011bf8edf29c4668fbc3619243485f2e1d0f52"><code>43011bf</code></a> Ensure invalidate_current_thread_spans has docs even if proc-macro disabled</li>
<li><a href="https://github.com/dtolnay/proc-macro2/commit/7e7bb0fbc94b94a14292e56d0b9cfbe2558a479c"><code>7e7bb0f</code></a> Improve documentation of invalidate_current_thread_spans</li>
<li><a href="https://github.com/dtolnay/proc-macro2/commit/64778fc68ff4dfa0e17e2de566206c7d441116b8"><code>64778fc</code></a> Move invalidate_current_thread_spans to proc_macro2::extra</li>
<li>Additional commits viewable in <a href="https://github.com/dtolnay/proc-macro2/compare/1.0.76...1.0.78">compare view</a></li>
</ul>
</details>
<br />


[![Dependabot compatibility score](https://dependabot-badges.githubapp.com/badges/compatibility_score?dependency-name=proc-macro2&package-manager=cargo&previous-version=1.0.76&new-version=1.0.78)](https://docs.github.com/en/github/managing-security-vulnerabilities/about-dependabot-security-updates#about-compatibility-scores)

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

_Label `internal` added by @dependabot[bot] on 2024-01-31 15:29_

---

_Merged by @charliermarsh on 2024-01-31 15:48_

---

_Closed by @charliermarsh on 2024-01-31 15:48_

---

_Branch deleted on 2024-01-31 15:48_

---
