```yaml
number: 1942
title: Bump insta from 1.34.0 to 1.35.1
type: pull_request
state: merged
author: dependabot
labels:
  - internal
assignees: []
merged: true
base: main
head: dependabot/cargo/insta-1.35.1
created_at: 2024-02-23T19:48:38Z
updated_at: 2024-02-23T21:00:36Z
url: https://github.com/astral-sh/uv/pull/1942
synced_at: 2026-01-10T14:54:43Z
```

# Bump insta from 1.34.0 to 1.35.1

---

_Pull request opened by @dependabot on 2024-02-23 19:48_

Bumps [insta](https://github.com/mitsuhiko/insta) from 1.34.0 to 1.35.1.
<details>
<summary>Changelog</summary>
<p><em>Sourced from <a href="https://github.com/mitsuhiko/insta/blob/master/CHANGELOG.md">insta's changelog</a>.</em></p>
<blockquote>
<h2>1.35.1</h2>
<ul>
<li>Fixed a bug with diffs showing bogus newlines.</li>
</ul>
<h2>1.35.0</h2>
<ul>
<li>Fixed a crash when a file named <code>.config</code> was in the root.</li>
<li>Added new alternative <code>match .. { ... }</code> syntax to redactions for better
<code>rustfmt</code> support.  (<a href="https://redirect.github.com/mitsuhiko/insta/issues/428">#428</a>)</li>
<li>The <code>--package</code> parameter can be supplied multiple times now.  (<a href="https://redirect.github.com/mitsuhiko/insta/issues/427">#427</a>)</li>
<li>Leading newlines in snapshots are now ignored to resolve issues with
inline snapshots that were never able to match.  (<a href="https://redirect.github.com/mitsuhiko/insta/issues/444">#444</a>)</li>
<li><code>cargo insta test</code> now accepts the <code>--test</code> parameter multiple times.  (<a href="https://redirect.github.com/mitsuhiko/insta/issues/437">#437</a>)</li>
</ul>
</blockquote>
</details>
<details>
<summary>Commits</summary>
<ul>
<li><a href="https://github.com/mitsuhiko/insta/commit/a75308a722a43749f812367dc4827143b4275233"><code>a75308a</code></a> 1.35.1</li>
<li><a href="https://github.com/mitsuhiko/insta/commit/ffc3a9bf83502ccea512cc23ea984258f1347fe2"><code>ffc3a9b</code></a> Trim on diff (<a href="https://redirect.github.com/mitsuhiko/insta/issues/445">#445</a>)</li>
<li><a href="https://github.com/mitsuhiko/insta/commit/0d9c6c227fe7e4fcd000a829e417efb8f637c729"><code>0d9c6c2</code></a> 1.35.0</li>
<li><a href="https://github.com/mitsuhiko/insta/commit/fd86b00544389026b57d5aaa758805391df275ac"><code>fd86b00</code></a> Accept --test multiple times</li>
<li><a href="https://github.com/mitsuhiko/insta/commit/9f9c890b54c1c17725b640b4d30d98538652407b"><code>9f9c890</code></a> Clean up error location reporting for yaml parsing errors</li>
<li><a href="https://github.com/mitsuhiko/insta/commit/64d66474474ddf8699b9b8bda2728ea9c0596208"><code>64d6647</code></a> Add the file reference on a YAML error message (<a href="https://redirect.github.com/mitsuhiko/insta/issues/443">#443</a>)</li>
<li><a href="https://github.com/mitsuhiko/insta/commit/c23b05746384d3dee41d08bf1f408f730d511cca"><code>c23b057</code></a> Add deprecation warning for --delete-unreferenced-snapshots (<a href="https://redirect.github.com/mitsuhiko/insta/issues/441">#441</a>)</li>
<li><a href="https://github.com/mitsuhiko/insta/commit/1f9ee0f8fbfe60e04d79face19a4e7df116faf91"><code>1f9ee0f</code></a> Only delete unreferenced snapshots after a successful test run (<a href="https://redirect.github.com/mitsuhiko/insta/issues/440">#440</a>)</li>
<li><a href="https://github.com/mitsuhiko/insta/commit/0974fe8dd579cc43074482982ac6be7573bf21db"><code>0974fe8</code></a> Ignore leading newlines on snapshot comparison (<a href="https://redirect.github.com/mitsuhiko/insta/issues/444">#444</a>)</li>
<li><a href="https://github.com/mitsuhiko/insta/commit/2a60c07b0fe3e2d0cda68e7a6810407bbce3764b"><code>2a60c07</code></a> Clippy be happy</li>
<li>Additional commits viewable in <a href="https://github.com/mitsuhiko/insta/compare/1.34.0...1.35.1">compare view</a></li>
</ul>
</details>
<br />


[![Dependabot compatibility score](https://dependabot-badges.githubapp.com/badges/compatibility_score?dependency-name=insta&package-manager=cargo&previous-version=1.34.0&new-version=1.35.1)](https://docs.github.com/en/github/managing-security-vulnerabilities/about-dependabot-security-updates#about-compatibility-scores)

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

_Label `internal` added by @dependabot[bot] on 2024-02-23 19:48_

---

_Merged by @zanieb on 2024-02-23 21:00_

---

_Closed by @zanieb on 2024-02-23 21:00_

---

_Branch deleted on 2024-02-23 21:00_

---
