```yaml
number: 7512
title: Bump syn from 2.0.33 to 2.0.37
type: pull_request
state: merged
author: dependabot
labels:
  - internal
assignees: []
merged: true
base: main
head: dependabot/cargo/syn-2.0.37
created_at: 2023-09-19T08:12:40Z
updated_at: 2023-09-19T09:08:57Z
url: https://github.com/astral-sh/ruff/pull/7512
synced_at: 2026-01-12T02:39:10Z
```

# Bump syn from 2.0.33 to 2.0.37

---

_Pull request opened by @dependabot on 2023-09-19 08:12_

Bumps [syn](https://github.com/dtolnay/syn) from 2.0.33 to 2.0.37.
<details>
<summary>Release notes</summary>
<p><em>Sourced from <a href="https://github.com/dtolnay/syn/releases">syn's releases</a>.</em></p>
<blockquote>
<h2>2.0.37</h2>
<ul>
<li>Work around incorrect future compatibility warning in rustc 1.74.0-nightly</li>
</ul>
<h2>2.0.36</h2>
<ul>
<li>Restore compatibility with <code>--generate-link-to-definition</code> documentation builds (<a href="https://redirect.github.com/dtolnay/syn/issues/1514">#1514</a>)</li>
</ul>
<h2>2.0.35</h2>
<ul>
<li>Make rust-analyzer produce preferred brackets for invocations of <code>Token!</code> macro (<a href="https://redirect.github.com/dtolnay/syn/issues/1510">#1510</a>, <a href="https://redirect.github.com/dtolnay/syn/issues/1512">#1512</a>)</li>
</ul>
<h2>2.0.34</h2>
<ul>
<li>Documentation improvements</li>
</ul>
</blockquote>
</details>
<details>
<summary>Commits</summary>
<ul>
<li><a href="https://github.com/dtolnay/syn/commit/96810880f3acbb63415cf4684ab42c82edc31e2a"><code>9681088</code></a> Release 2.0.37</li>
<li><a href="https://github.com/dtolnay/syn/commit/fbe3bc2ddcee4192f95697a953330d031030f5a1"><code>fbe3bc2</code></a> Work around unknown_lints warning on rustc older than 1.64</li>
<li><a href="https://github.com/dtolnay/syn/commit/75cf912e061ef5a1d97c003f4988f43d7639f5a8"><code>75cf912</code></a> Ignore more repr_transparent_external_private_fields</li>
<li><a href="https://github.com/dtolnay/syn/commit/299c782439823b868de4aea3682b41d03351d978"><code>299c782</code></a> Ignore false repr_transparent_external_private_fields FCW in generated code</li>
<li><a href="https://github.com/dtolnay/syn/commit/ef6476c76431da488720c8ee3fdfff57a199a848"><code>ef6476c</code></a> Release 2.0.36</li>
<li><a href="https://github.com/dtolnay/syn/commit/6ae1a9756a128f18341a4cba56772ee1715a06e2"><code>6ae1a97</code></a> Merge pull request <a href="https://redirect.github.com/dtolnay/syn/issues/1514">#1514</a> from dtolnay/pubnotdoc</li>
<li><a href="https://github.com/dtolnay/syn/commit/7bfef4b2befee954bc002724c8331612fed3d47f"><code>7bfef4b</code></a> Work around doc breakage in generate-link-to-definition mode</li>
<li><a href="https://github.com/dtolnay/syn/commit/ce360ff4b1695311dcc569ade1fc9ff1ff6ac4d9"><code>ce360ff</code></a> Release 2.0.35</li>
<li><a href="https://github.com/dtolnay/syn/commit/23736bdc48a964286ea032ea5c172574239119ef"><code>23736bd</code></a> Merge pull request <a href="https://redirect.github.com/dtolnay/syn/issues/1512">#1512</a> from dtolnay/tokendoc</li>
<li><a href="https://github.com/dtolnay/syn/commit/6f6b0040d420abfda0b5504dc13b7c76319ebccd"><code>6f6b004</code></a> Improve docs of Token! macro</li>
<li>Additional commits viewable in <a href="https://github.com/dtolnay/syn/compare/2.0.33...2.0.37">compare view</a></li>
</ul>
</details>
<br />


[![Dependabot compatibility score](https://dependabot-badges.githubapp.com/badges/compatibility_score?dependency-name=syn&package-manager=cargo&previous-version=2.0.33&new-version=2.0.37)](https://docs.github.com/en/github/managing-security-vulnerabilities/about-dependabot-security-updates#about-compatibility-scores)

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

_Label `internal` added by @dependabot[bot] on 2023-09-19 08:12_

---

_Comment by @github-actions[bot] on 2023-09-19 08:35_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.



---

_Merged by @MichaReiser on 2023-09-19 09:08_

---

_Closed by @MichaReiser on 2023-09-19 09:08_

---

_Branch deleted on 2023-09-19 09:08_

---
