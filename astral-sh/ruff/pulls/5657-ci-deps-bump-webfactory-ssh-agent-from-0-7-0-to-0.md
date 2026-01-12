```yaml
number: 5657
title: "ci(deps): bump webfactory/ssh-agent from 0.7.0 to 0.8.0"
type: pull_request
state: merged
author: dependabot
labels:
  - dependencies
assignees: []
merged: true
base: main
head: dependabot/github_actions/webfactory/ssh-agent-0.8.0
created_at: 2023-07-10T16:42:17Z
updated_at: 2023-07-10T17:10:27Z
url: https://github.com/astral-sh/ruff/pull/5657
synced_at: 2026-01-12T03:36:55Z
```

# ci(deps): bump webfactory/ssh-agent from 0.7.0 to 0.8.0

---

_Pull request opened by @dependabot on 2023-07-10 16:42_

Bumps [webfactory/ssh-agent](https://github.com/webfactory/ssh-agent) from 0.7.0 to 0.8.0.
<details>
<summary>Release notes</summary>
<p><em>Sourced from <a href="https://github.com/webfactory/ssh-agent/releases">webfactory/ssh-agent's releases</a>.</em></p>
<blockquote>
<h2>SSH host keys no longer managed – read below 👇</h2>
<p>Starting with this release, this action no longer writes GitHub's SSH host keys into the <code>known_hosts</code> SSH config file upon start.</p>
<p>GitHub changed their host keys on short notice this morning, see <a href="https://github.blog/2023-03-23-we-updated-our-rsa-ssh-host-key/">https://github.blog/2023-03-23-we-updated-our-rsa-ssh-host-key/</a>. We took this as an opportunity to stop maintaining GH SSH keys in the code shipped with this action (<a href="https://redirect.github.com/webfactory/ssh-agent/issues/171">#171</a>).</p>
<p>What you need to do:</p>
<ul>
<li>On GitHub hosted runners, nothing. ✔︎ These runners ship with SSH host keys (for <code>github.com</code>) maintained by directly by GitHub.</li>
<li>On self-hosted runners, review and fix your SSH <code>known_hosts</code> file:
<ul>
<li>First, you'll find it bloated with redundant entries for <code>github.com</code>, as described in <a href="https://redirect.github.com/webfactory/ssh-agent/issues/106">#106</a>. Remove these entries.</li>
<li>Review <a href="https://github.blog/2023-03-23-we-updated-our-rsa-ssh-host-key/">https://github.blog/2023-03-23-we-updated-our-rsa-ssh-host-key/</a>. You probably removed the old (invalid) SSH key in the previous step.</li>
<li>Configure GitHub's current SSH keys as documented on <a href="https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/githubs-ssh-key-fingerprints">https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/githubs-ssh-key-fingerprints</a></li>
<li>As long as versions before <code>v0.8.0</code> of this action here are run on the self-hosted runner, the old entries will come back. Keep an eye on it, possibly you'll have to rinse &amp; repeat.</li>
</ul>
</li>
</ul>
<h4>Other code changes in this release</h4>
<ul>
<li>Update to <code>actions/checkout@v3</code> by <a href="https://github.com/mpdude"><code>@​mpdude</code></a> in <a href="https://redirect.github.com/webfactory/ssh-agent/pull/143">webfactory/ssh-agent#143</a></li>
<li>Allow the user to override the commands for <code>git</code>, <code>ssh-agent</code>, and <code>ssh-add</code> by <a href="https://github.com/DilumAluthge"><code>@​DilumAluthge</code></a> in <a href="https://redirect.github.com/webfactory/ssh-agent/pull/154">webfactory/ssh-agent#154</a></li>
</ul>
<h2>New Contributors</h2>
<ul>
<li><a href="https://github.com/prhiggins"><code>@​prhiggins</code></a> made their first contribution in <a href="https://redirect.github.com/webfactory/ssh-agent/pull/153">webfactory/ssh-agent#153</a></li>
<li><a href="https://github.com/kjarkur"><code>@​kjarkur</code></a> made their first contribution in <a href="https://redirect.github.com/webfactory/ssh-agent/pull/147">webfactory/ssh-agent#147</a></li>
<li><a href="https://github.com/DilumAluthge"><code>@​DilumAluthge</code></a> made their first contribution in <a href="https://redirect.github.com/webfactory/ssh-agent/pull/154">webfactory/ssh-agent#154</a></li>
</ul>
<p><strong>Full Changelog</strong>: <a href="https://github.com/webfactory/ssh-agent/compare/v0.7.0...v0.8.0">https://github.com/webfactory/ssh-agent/compare/v0.7.0...v0.8.0</a></p>
</blockquote>
</details>
<details>
<summary>Changelog</summary>
<p><em>Sourced from <a href="https://github.com/webfactory/ssh-agent/blob/master/CHANGELOG.md">webfactory/ssh-agent's changelog</a>.</em></p>
<blockquote>
<h1>Changelog</h1>
<p>All notable changes to this project will be documented in this file.</p>
<p>The format is based on <a href="https://keepachangelog.com/en/1.0.0/">Keep a Changelog</a>,
and this project adheres to <a href="https://semver.org/spec/v2.0.0.html">Semantic Versioning</a>.</p>
<h2>[Unreleased]</h2>
</blockquote>
</details>
<details>
<summary>Commits</summary>
<ul>
<li><a href="https://github.com/webfactory/ssh-agent/commit/d4b9b8ff72958532804b70bbe600ad43b36d5f2e"><code>d4b9b8f</code></a> Stop adding GitHub SSH keys (<a href="https://redirect.github.com/webfactory/ssh-agent/issues/171">#171</a>)</li>
<li><a href="https://github.com/webfactory/ssh-agent/commit/ea17a056b908d85ca1f46c275ab91389eb65c9fa"><code>ea17a05</code></a> Add missing semicolons (<a href="https://redirect.github.com/webfactory/ssh-agent/issues/159">#159</a>)</li>
<li><a href="https://github.com/webfactory/ssh-agent/commit/9fbc246995ce195568ea31d1110c161283c89684"><code>9fbc246</code></a> Clarify usage for Docker build processes, especially with deployment keys (<a href="https://redirect.github.com/webfactory/ssh-agent/issues/145">#145</a>)</li>
<li><a href="https://github.com/webfactory/ssh-agent/commit/6f828ccb51b0d042f4783ff45710decae9836d37"><code>6f828cc</code></a> Allow the user to override the commands for <code>git</code>, <code>ssh-agent</code>, and <code>ssh-add</code>...</li>
<li><a href="https://github.com/webfactory/ssh-agent/commit/209e2d72ff4a448964d26610aceaaf1b3f8764c6"><code>209e2d7</code></a> Fix a typo in the README.md (<a href="https://redirect.github.com/webfactory/ssh-agent/issues/146">#146</a>)</li>
<li><a href="https://github.com/webfactory/ssh-agent/commit/18ff7066d37af765c4268af534ab1500bc3ea7f9"><code>18ff706</code></a> Update README.md (<a href="https://redirect.github.com/webfactory/ssh-agent/issues/147">#147</a>)</li>
<li><a href="https://github.com/webfactory/ssh-agent/commit/2996779c087b05cb911c8fefc8403a8af8d8f2d4"><code>2996779</code></a> Replace 0.6.0 references with 0.7.0 in README.md (<a href="https://redirect.github.com/webfactory/ssh-agent/issues/153">#153</a>)</li>
<li><a href="https://github.com/webfactory/ssh-agent/commit/4512be8010f262a32b74d4b4a59300d76bdd43c6"><code>4512be8</code></a> Update to <code>actions/checkout@v3</code> (<a href="https://redirect.github.com/webfactory/ssh-agent/issues/143">#143</a>)</li>
<li>See full diff in <a href="https://github.com/webfactory/ssh-agent/compare/v0.7.0...v0.8.0">compare view</a></li>
</ul>
</details>
<br />


[![Dependabot compatibility score](https://dependabot-badges.githubapp.com/badges/compatibility_score?dependency-name=webfactory/ssh-agent&package-manager=github_actions&previous-version=0.7.0&new-version=0.8.0)](https://docs.github.com/en/github/managing-security-vulnerabilities/about-dependabot-security-updates#about-compatibility-scores)

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
- `@dependabot ignore this major version` will close this PR and stop Dependabot creating any more for this major version (unless you reopen the PR or upgrade to it yourself)
- `@dependabot ignore this minor version` will close this PR and stop Dependabot creating any more for this minor version (unless you reopen the PR or upgrade to it yourself)
- `@dependabot ignore this dependency` will close this PR and stop Dependabot creating any more for this dependency (unless you reopen the PR or upgrade to it yourself)


</details>

---

_Label `dependencies` added by @dependabot[bot] on 2023-07-10 16:42_

---

_Comment by @github-actions[bot] on 2023-07-10 16:53_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.06      9.4±0.45ms     4.3 MB/sec    1.00      8.8±0.41ms     4.6 MB/sec
formatter/numpy/ctypeslib.py               1.02      2.0±0.11ms     8.3 MB/sec    1.00  1962.9±93.50µs     8.5 MB/sec
formatter/numpy/globals.py                 1.01   228.1±11.71µs    12.9 MB/sec    1.00   226.6±11.46µs    13.0 MB/sec
formatter/pydantic/types.py                1.00      4.3±0.19ms     5.9 MB/sec    1.02      4.4±0.29ms     5.8 MB/sec
linter/all-rules/large/dataset.py          1.00     15.8±0.66ms     2.6 MB/sec    1.11     17.6±0.55ms     2.3 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.1±0.18ms     4.1 MB/sec    1.07      4.3±0.10ms     3.8 MB/sec
linter/all-rules/numpy/globals.py          1.00   511.9±23.11µs     5.8 MB/sec    1.11   568.3±24.06µs     5.2 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.0±0.41ms     3.6 MB/sec    1.12      7.8±0.26ms     3.3 MB/sec
linter/default-rules/large/dataset.py      1.00      7.7±0.32ms     5.2 MB/sec    1.19      9.2±0.36ms     4.4 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1698.4±71.70µs     9.8 MB/sec    1.10  1863.9±183.92µs     8.9 MB/sec
linter/default-rules/numpy/globals.py      1.00   204.9±14.51µs    14.4 MB/sec    1.03    211.2±8.37µs    14.0 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.5±0.15ms     7.2 MB/sec    1.09      3.9±0.19ms     6.6 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      9.5±0.11ms     4.3 MB/sec    1.01      9.6±0.20ms     4.2 MB/sec
formatter/numpy/ctypeslib.py               1.00      2.1±0.03ms     8.1 MB/sec    1.02      2.1±0.07ms     7.9 MB/sec
formatter/numpy/globals.py                 1.00    231.1±4.13µs    12.8 MB/sec    1.02   236.6±10.08µs    12.5 MB/sec
formatter/pydantic/types.py                1.00      4.6±0.07ms     5.6 MB/sec    1.02      4.7±0.11ms     5.5 MB/sec
linter/all-rules/large/dataset.py          1.00     16.2±0.15ms     2.5 MB/sec    1.00     16.2±0.16ms     2.5 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.2±0.07ms     4.0 MB/sec    1.00      4.2±0.06ms     4.0 MB/sec
linter/all-rules/numpy/globals.py          1.00    502.0±8.85µs     5.9 MB/sec    1.01    506.4±8.20µs     5.8 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.1±0.14ms     3.6 MB/sec    1.00      7.1±0.07ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.00      8.1±0.09ms     5.0 MB/sec    1.00      8.1±0.06ms     5.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1709.1±18.25µs     9.7 MB/sec    1.00  1713.7±16.77µs     9.7 MB/sec
linter/default-rules/numpy/globals.py      1.00    199.4±3.98µs    14.8 MB/sec    1.00    200.2±6.05µs    14.7 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.6±0.03ms     7.1 MB/sec    1.01      3.6±0.04ms     7.0 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_@zanieb approved on 2023-07-10 16:58_

The ssh key changes look fine for us

---

_Merged by @charliermarsh on 2023-07-10 17:10_

---

_Closed by @charliermarsh on 2023-07-10 17:10_

---

_Branch deleted on 2023-07-10 17:10_

---
