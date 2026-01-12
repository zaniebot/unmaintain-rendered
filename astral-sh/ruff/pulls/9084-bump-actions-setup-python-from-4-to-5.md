```yaml
number: 9084
title: Bump actions/setup-python from 4 to 5
type: pull_request
state: merged
author: dependabot
labels:
  - internal
assignees: []
merged: true
base: main
head: dependabot/github_actions/actions/setup-python-5
created_at: 2023-12-11T08:06:35Z
updated_at: 2023-12-11T20:49:34Z
url: https://github.com/astral-sh/ruff/pull/9084
synced_at: 2026-01-10T23:40:55Z
```

# Bump actions/setup-python from 4 to 5

---

_Pull request opened by @dependabot on 2023-12-11 08:06_

Bumps [actions/setup-python](https://github.com/actions/setup-python) from 4 to 5.
<details>
<summary>Release notes</summary>
<p><em>Sourced from <a href="https://github.com/actions/setup-python/releases">actions/setup-python's releases</a>.</em></p>
<blockquote>
<h2>v5.0.0</h2>
<h2>What's Changed</h2>
<p>In scope of this release, we update node version runtime from node16 to node20 (<a href="https://redirect.github.com/actions/setup-python/pull/772">actions/setup-python#772</a>). Besides, we update dependencies to the latest versions.</p>
<p><strong>Full Changelog</strong>: <a href="https://github.com/actions/setup-python/compare/v4.8.0...v5.0.0">https://github.com/actions/setup-python/compare/v4.8.0...v5.0.0</a></p>
<h2>v4.8.0</h2>
<h2>What's Changed</h2>
<p>In scope of this release we added support for GraalPy (<a href="https://redirect.github.com/actions/setup-python/pull/694">actions/setup-python#694</a>). You can use this snippet to set up GraalPy:</p>
<pre lang="yaml"><code>steps:
- uses: actions/checkout@v4
- uses: actions/setup-python@v4 
  with:
    python-version: 'graalpy-22.3' 
- run: python my_script.py
</code></pre>
<p>Besides, the release contains such changes as:</p>
<ul>
<li>Trim python version when reading from file by <a href="https://github.com/FerranPares"><code>@​FerranPares</code></a> in <a href="https://redirect.github.com/actions/setup-python/pull/628">actions/setup-python#628</a></li>
<li>Use non-deprecated versions in examples by <a href="https://github.com/jeffwidman"><code>@​jeffwidman</code></a> in <a href="https://redirect.github.com/actions/setup-python/pull/724">actions/setup-python#724</a></li>
<li>Change deprecation comment to past tense by <a href="https://github.com/jeffwidman"><code>@​jeffwidman</code></a> in <a href="https://redirect.github.com/actions/setup-python/pull/723">actions/setup-python#723</a></li>
<li>Bump <code>@​babel/traverse</code> from 7.9.0 to 7.23.2 by <a href="https://github.com/dependabot"><code>@​dependabot</code></a> in <a href="https://redirect.github.com/actions/setup-python/pull/743">actions/setup-python#743</a></li>
<li>advanced-usage.md: Encourage the use actions/checkout@v4 by <a href="https://github.com/cclauss"><code>@​cclauss</code></a> in <a href="https://redirect.github.com/actions/setup-python/pull/729">actions/setup-python#729</a></li>
<li>Examples now use checkout@v4 by <a href="https://github.com/simonw"><code>@​simonw</code></a> in <a href="https://redirect.github.com/actions/setup-python/pull/738">actions/setup-python#738</a></li>
<li>Update actions/checkout to v4 by <a href="https://github.com/dmitry-shibanov"><code>@​dmitry-shibanov</code></a> in <a href="https://redirect.github.com/actions/setup-python/pull/761">actions/setup-python#761</a></li>
</ul>
<h2>New Contributors</h2>
<ul>
<li><a href="https://github.com/FerranPares"><code>@​FerranPares</code></a> made their first contribution in <a href="https://redirect.github.com/actions/setup-python/pull/628">actions/setup-python#628</a></li>
<li><a href="https://github.com/timfel"><code>@​timfel</code></a> made their first contribution in <a href="https://redirect.github.com/actions/setup-python/pull/694">actions/setup-python#694</a></li>
<li><a href="https://github.com/jeffwidman"><code>@​jeffwidman</code></a> made their first contribution in <a href="https://redirect.github.com/actions/setup-python/pull/724">actions/setup-python#724</a></li>
</ul>
<p><strong>Full Changelog</strong>: <a href="https://github.com/actions/setup-python/compare/v4...v4.8.0">https://github.com/actions/setup-python/compare/v4...v4.8.0</a></p>
<h2>v4.7.1</h2>
<h2>What's Changed</h2>
<ul>
<li>Bump word-wrap from 1.2.3 to 1.2.4 by <a href="https://github.com/dependabot"><code>@​dependabot</code></a> in <a href="https://redirect.github.com/actions/setup-python/pull/702">actions/setup-python#702</a></li>
<li>Add range validation for toml files by <a href="https://github.com/dmitry-shibanov"><code>@​dmitry-shibanov</code></a> in <a href="https://redirect.github.com/actions/setup-python/pull/726">actions/setup-python#726</a></li>
</ul>
<p><strong>Full Changelog</strong>: <a href="https://github.com/actions/setup-python/compare/v4...v4.7.1">https://github.com/actions/setup-python/compare/v4...v4.7.1</a></p>
<h2>v4.7.0</h2>
<p>In scope of this release, the support for reading python version from pyproject.toml was added (<a href="https://redirect.github.com/actions/setup-python/pull/669">actions/setup-python#669</a>).</p>
<pre lang="yaml"><code>      - name: Setup Python
        uses: actions/setup-python@v4
&lt;/tr&gt;&lt;/table&gt; 
</code></pre>
</blockquote>
<p>... (truncated)</p>
</details>
<details>
<summary>Commits</summary>
<ul>
<li><a href="https://github.com/actions/setup-python/commit/0a5c61591373683505ea898e09a3ea4f39ef2b9c"><code>0a5c615</code></a> Update action to node20 (<a href="https://redirect.github.com/actions/setup-python/issues/772">#772</a>)</li>
<li><a href="https://github.com/actions/setup-python/commit/0ae58361cdfd39e2950bed97a1e26aa20c3d8955"><code>0ae5836</code></a> Add example of GraalPy to docs (<a href="https://redirect.github.com/actions/setup-python/issues/773">#773</a>)</li>
<li><a href="https://github.com/actions/setup-python/commit/b64ffcaf5b410884ad320a9cfac8866006a109aa"><code>b64ffca</code></a> update actions/checkout to v4 (<a href="https://redirect.github.com/actions/setup-python/issues/761">#761</a>)</li>
<li><a href="https://github.com/actions/setup-python/commit/8d2896179abf658742de432b3f203d2c2d86a587"><code>8d28961</code></a> Examples now use checkout@v4 (<a href="https://redirect.github.com/actions/setup-python/issues/738">#738</a>)</li>
<li><a href="https://github.com/actions/setup-python/commit/7bc6abb01e0555719edc2dbca70a2fde309e5e56"><code>7bc6abb</code></a> advanced-usage.md: Encourage the use actions/checkout@v4 (<a href="https://redirect.github.com/actions/setup-python/issues/729">#729</a>)</li>
<li><a href="https://github.com/actions/setup-python/commit/e8111cec9d3dc15220d8a3b638f08419f57b906a"><code>e8111ce</code></a> Bump <code>@​babel/traverse</code> from 7.9.0 to 7.23.2 (<a href="https://redirect.github.com/actions/setup-python/issues/743">#743</a>)</li>
<li><a href="https://github.com/actions/setup-python/commit/a00ea43da65e7c04d2bdae58b3afecd77057eb9e"><code>a00ea43</code></a> add fix for graalpy ci (<a href="https://redirect.github.com/actions/setup-python/issues/741">#741</a>)</li>
<li><a href="https://github.com/actions/setup-python/commit/8635b1ccc5934e73ed3510980fd2e7790b85839b"><code>8635b1c</code></a> Change deprecation comment to past tense (<a href="https://redirect.github.com/actions/setup-python/issues/723">#723</a>)</li>
<li><a href="https://github.com/actions/setup-python/commit/f6cc428f535856f9c23558d01765a42a4d6cf758"><code>f6cc428</code></a> Use non-deprecated versions in examples (<a href="https://redirect.github.com/actions/setup-python/issues/724">#724</a>)</li>
<li><a href="https://github.com/actions/setup-python/commit/5f2af211d616f86005883b44826180b21abb4060"><code>5f2af21</code></a> Add GraalPy support (<a href="https://redirect.github.com/actions/setup-python/issues/694">#694</a>)</li>
<li>Additional commits viewable in <a href="https://github.com/actions/setup-python/compare/v4...v5">compare view</a></li>
</ul>
</details>
<br />


[![Dependabot compatibility score](https://dependabot-badges.githubapp.com/badges/compatibility_score?dependency-name=actions/setup-python&package-manager=github_actions&previous-version=4&new-version=5)](https://docs.github.com/en/github/managing-security-vulnerabilities/about-dependabot-security-updates#about-compatibility-scores)

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

_Label `internal` added by @dependabot[bot] on 2023-12-11 08:06_

---

_Comment by @github-actions[bot] on 2023-12-11 08:23_

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

_Comment by @konstin on 2023-12-11 08:47_

@dependabot rebase

---

_Merged by @charliermarsh on 2023-12-11 20:49_

---

_Closed by @charliermarsh on 2023-12-11 20:49_

---

_Branch deleted on 2023-12-11 20:49_

---
