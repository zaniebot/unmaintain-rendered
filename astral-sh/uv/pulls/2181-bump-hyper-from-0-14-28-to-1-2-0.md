```yaml
number: 2181
title: Bump hyper from 0.14.28 to 1.2.0
type: pull_request
state: closed
author: dependabot
labels:
  - internal
assignees: []
base: main
head: dependabot/cargo/hyper-1.2.0
created_at: 2024-03-04T22:05:25Z
updated_at: 2024-03-04T22:07:01Z
url: https://github.com/astral-sh/uv/pull/2181
synced_at: 2026-01-12T16:04:54Z
```

# Bump hyper from 0.14.28 to 1.2.0

---

_@dependabot_

Bumps [hyper](https://github.com/hyperium/hyper) from 0.14.28 to 1.2.0.
<details>
<summary>Release notes</summary>
<p><em>Sourced from <a href="https://github.com/hyperium/hyper/releases">hyper's releases</a>.</em></p>
<blockquote>
<h2>v1.2.0</h2>
<h2>Features</h2>
<ul>
<li><strong>http1:</strong> support configurable <code>max_headers(num)</code> to client and server (<a href="https://redirect.github.com/hyperium/hyper/issues/3523">#3523</a>) (<a href="https://github.com/hyperium/hyper/commit/b114244898828e9fb254bea1f0bbdd24850b2f3f">b1142448</a>)</li>
<li><strong>http2:</strong>
<ul>
<li>add config for <code>max_local_error_reset_streams</code> in server (<a href="https://redirect.github.com/hyperium/hyper/issues/3530">#3530</a>) (<a href="https://github.com/hyperium/hyper/commit/d7680e30e48926a5a3f94a0986d39181d5ab2218">d7680e30</a>)</li>
<li>add <code>initial_max_send_streams</code> method to HTTP/2 client builder (<a href="https://redirect.github.com/hyperium/hyper/issues/3524">#3524</a>) (<a href="https://github.com/hyperium/hyper/commit/fdfa60d9fafb8a6bfb40acc4042ee54a2b9aad32">fdfa60d9</a>)
<ul>
<li><strong>NOTE</strong>: The default for this will change in v1.3 to something conservative. If you have an environment where the server can always accept a large amount of concurrent streams, and depend on that for performance, you should set this option manually.</li>
</ul>
</li>
<li>add <code>max_pending_accept_reset_streams(num)</code> back to HTTP/2 server builder (<a href="https://redirect.github.com/hyperium/hyper/issues/3507">#3507</a> (<a href="https://github.com/hyperium/hyper/commit/a9fa893f18c6409abae2e1dcbba0f4487df54d4f">a9fa893f</a>)</li>
</ul>
</li>
</ul>
<h2>Bug Fixes</h2>
<ul>
<li><strong>http2:</strong> typo in trace logging (<a href="https://redirect.github.com/hyperium/hyper/issues/3536">#3536</a>) (<a href="https://github.com/hyperium/hyper/commit/79862ec2e84c32122c820958ceec06d8b7701ff7">79862ec2</a>)</li>
<li><strong>rt:</strong> <code>Sleep::downcast_mut_pin()</code> no longer extend lifetime (<a href="https://github.com/hyperium/hyper/commit/7206fe30302937075c51c16a69d1eb3bbce6a671">7206fe30</a>, closes <a href="https://redirect.github.com/hyperium/hyper/issues/3556">#3556</a>)</li>
</ul>
<h2>Breaking Changes</h2>
<ul>
<li>The returned lifetime from <code>Sleep::downcast_mut_pin()</code>
is no longer <code>'static</code>. This shouldn't affect most usage. This sort of
breaking change is needed because it is <em>wrong</em>.  (<a href="https://github.com/hyperium/hyper/commit/7206fe30302937075c51c16a69d1eb3bbce6a671">7206fe30</a>)</li>
</ul>
<h2>New Contributors</h2>
<ul>
<li><a href="https://github.com/getong"><code>@​getong</code></a> made their first contribution in <a href="https://redirect.github.com/hyperium/hyper/pull/3503">hyperium/hyper#3503</a></li>
<li><a href="https://github.com/dsgallups"><code>@​dsgallups</code></a> made their first contribution in <a href="https://redirect.github.com/hyperium/hyper/pull/3507">hyperium/hyper#3507</a></li>
<li><a href="https://github.com/magurotuna"><code>@​magurotuna</code></a> made their first contribution in <a href="https://redirect.github.com/hyperium/hyper/pull/3524">hyperium/hyper#3524</a></li>
<li><a href="https://github.com/erebe"><code>@​erebe</code></a> made their first contribution in <a href="https://redirect.github.com/hyperium/hyper/pull/3536">hyperium/hyper#3536</a></li>
<li><a href="https://github.com/wfly1998"><code>@​wfly1998</code></a> made their first contribution in <a href="https://redirect.github.com/hyperium/hyper/pull/3523">hyperium/hyper#3523</a></li>
</ul>
<h2>v1.1.0</h2>
<h2>Features</h2>
<ul>
<li><strong>client:</strong> add <code>http1::Connection</code> <code>without_shutdown()</code> method (<a href="https://redirect.github.com/hyperium/hyper/issues/3430">#3430</a>) (<a href="https://github.com/hyperium/hyper/commit/210bfaa711b5da1f6756582a2e4bc3e229924800">210bfaa7</a>)</li>
<li><strong>http1:</strong> Add support for sending HTTP/1.1 Chunked Trailer Fields (<a href="https://redirect.github.com/hyperium/hyper/issues/3375">#3375</a>) (<a href="https://github.com/hyperium/hyper/commit/31b41807523370f3efbf47ba16c9e1c193b6335a">31b41807</a>, closes <a href="https://redirect.github.com/hyperium/hyper/issues/2719">#2719</a>)</li>
<li><strong>server:</strong> expose <code>server::conn::http1::UpgradeableConnection</code> (<a href="https://redirect.github.com/hyperium/hyper/issues/3457">#3457</a>) (<a href="https://github.com/hyperium/hyper/commit/6e3042a86f10359624857d31bc9e876f521aee42">6e3042a8</a>)</li>
</ul>
<h2>Bug Fixes</h2>
<ul>
<li><strong>http1:</strong>
<ul>
<li>add internal limit for chunked extensions (<a href="https://redirect.github.com/hyperium/hyper/issues/3495">#3495</a>) (<a href="https://github.com/hyperium/hyper/commit/d71ff962b08aca2f1c9c1724dfdab5bc1ec6ecd2">d71ff962</a>)</li>
<li>reject chunked headers missing a digit (<a href="https://redirect.github.com/hyperium/hyper/issues/3494">#3494</a>) (<a href="https://github.com/hyperium/hyper/commit/829153865a4d2bbb52227183c8857e57dc3e231b">82915386</a>)</li>
</ul>
</li>
</ul>
<h2>New Contributors</h2>
<ul>
<li><a href="https://github.com/acedogblast"><code>@​acedogblast</code></a> made their first contribution in <a href="https://redirect.github.com/hyperium/hyper/pull/3433">hyperium/hyper#3433</a></li>
<li><a href="https://github.com/hatoo"><code>@​hatoo</code></a> made their first contribution in <a href="https://redirect.github.com/hyperium/hyper/pull/3434">hyperium/hyper#3434</a></li>
<li><a href="https://github.com/allan2"><code>@​allan2</code></a> made their first contribution in <a href="https://redirect.github.com/hyperium/hyper/pull/3449">hyperium/hyper#3449</a></li>
<li><a href="https://github.com/daxhuiberts"><code>@​daxhuiberts</code></a> made their first contribution in <a href="https://redirect.github.com/hyperium/hyper/pull/3464">hyperium/hyper#3464</a></li>
<li><a href="https://github.com/ncihnegn"><code>@​ncihnegn</code></a> made their first contribution in <a href="https://redirect.github.com/hyperium/hyper/pull/3479">hyperium/hyper#3479</a></li>
</ul>
<!-- raw HTML omitted -->
</blockquote>
<p>... (truncated)</p>
</details>
<details>
<summary>Changelog</summary>
<p><em>Sourced from <a href="https://github.com/hyperium/hyper/blob/master/CHANGELOG.md">hyper's changelog</a>.</em></p>
<blockquote>
<h2>v1.2.0 (2024-02-21)</h2>
<h4>Bug Fixes</h4>
<ul>
<li><strong>http2:</strong> typo in trace logging (<a href="https://redirect.github.com/hyperium/hyper/issues/3536">#3536</a>) (<a href="https://github.com/hyperium/hyper/commit/79862ec2e84c32122c820958ceec06d8b7701ff7">79862ec2</a>)</li>
<li><strong>rt:</strong> <code>Sleep::downcast_mut_pin()</code> no longer extend lifetime (<a href="https://github.com/hyperium/hyper/commit/7206fe30302937075c51c16a69d1eb3bbce6a671">7206fe30</a>, closes <a href="https://redirect.github.com/hyperium/hyper/issues/3556">#3556</a>)</li>
</ul>
<h4>Features</h4>
<ul>
<li><strong>http1:</strong> support configurable <code>max_headers(num)</code> to client and server (<a href="https://redirect.github.com/hyperium/hyper/issues/3523">#3523</a>) (<a href="https://github.com/hyperium/hyper/commit/b114244898828e9fb254bea1f0bbdd24850b2f3f">b1142448</a>)</li>
<li><strong>http2:</strong>
<ul>
<li>add config for <code>max_local_error_reset_streams</code> in server (<a href="https://redirect.github.com/hyperium/hyper/issues/3530">#3530</a>) (<a href="https://github.com/hyperium/hyper/commit/d7680e30e48926a5a3f94a0986d39181d5ab2218">d7680e30</a>)</li>
<li>add <code>initial_max_send_streams</code> method to HTTP/2 client builder (<a href="https://redirect.github.com/hyperium/hyper/issues/3524">#3524</a>) (<a href="https://github.com/hyperium/hyper/commit/fdfa60d9fafb8a6bfb40acc4042ee54a2b9aad32">fdfa60d9</a>)</li>
<li>add <code>max_pending_accept_reset_streams(num)</code> back to HTTP/2 server builder (<a href="https://redirect.github.com/hyperium/hyper/issues/3507">#3507</a> (<a href="https://github.com/hyperium/hyper/commit/a9fa893f18c6409abae2e1dcbba0f4487df54d4f">a9fa893f</a>)</li>
</ul>
</li>
</ul>
<h4>Breaking Changes</h4>
<ul>
<li>The returned lifetime from <code>Sleep::downcast_mut_pin()</code>
is no longer <code>'static</code>. This shouldn't affect most usage. This sort of
breaking change is needed because it is <em>wrong</em>.</li>
</ul>
<p>(<a href="https://github.com/hyperium/hyper/commit/7206fe30302937075c51c16a69d1eb3bbce6a671">7206fe30</a>)</p>
<h2>v1.1.0 (2023-12-18)</h2>
<h4>Bug Fixes</h4>
<ul>
<li><strong>http1:</strong>
<ul>
<li>add internal limit for chunked extensions (<a href="https://redirect.github.com/hyperium/hyper/issues/3495">#3495</a>) (<a href="https://github.com/hyperium/hyper/commit/d71ff962b08aca2f1c9c1724dfdab5bc1ec6ecd2">d71ff962</a>)</li>
<li>reject chunked headers missing a digit (<a href="https://redirect.github.com/hyperium/hyper/issues/3494">#3494</a>) (<a href="https://github.com/hyperium/hyper/commit/829153865a4d2bbb52227183c8857e57dc3e231b">82915386</a>)</li>
</ul>
</li>
</ul>
<h4>Features</h4>
<ul>
<li><strong>client:</strong> add <code>http1::Connection</code> <code>without_shutdown()</code> method (<a href="https://redirect.github.com/hyperium/hyper/issues/3430">#3430</a>) (<a href="https://github.com/hyperium/hyper/commit/210bfaa711b5da1f6756582a2e4bc3e229924800">210bfaa7</a>)</li>
<li><strong>http1:</strong> Add support for sending HTTP/1.1 Chunked Trailer Fields (<a href="https://redirect.github.com/hyperium/hyper/issues/3375">#3375</a>) (<a href="https://github.com/hyperium/hyper/commit/31b41807523370f3efbf47ba16c9e1c193b6335a">31b41807</a>, closes <a href="https://redirect.github.com/hyperium/hyper/issues/2719">#2719</a>)</li>
<li><strong>server:</strong> expose <code>server::conn::http1::UpgradeableConnection</code> (<a href="https://redirect.github.com/hyperium/hyper/issues/3457">#3457</a>) (<a href="https://github.com/hyperium/hyper/commit/6e3042a86f10359624857d31bc9e876f521aee42">6e3042a8</a>)</li>
</ul>
<h3>v1.0.1 (2023-11-16)</h3>
<p>This release &quot;fixes&quot; or adds a few things that should have been in 1.0.0, but were forgotten. Thus, it includes additions that would normally be a semver-minor release, but because it is so close to 1.0.0, it is released as a patch version.</p>
<h4>Bug Fixes</h4>
<!-- raw HTML omitted -->
</blockquote>
<p>... (truncated)</p>
</details>
<details>
<summary>Commits</summary>
<ul>
<li><a href="https://github.com/hyperium/hyper/commit/198c1b9622f06a2afd7d748a7263963a08c2c6b1"><code>198c1b9</code></a> v1.2.0</li>
<li><a href="https://github.com/hyperium/hyper/commit/a7bdc60bb5c3fd78b12c6d5668ff0cf889e0a752"><code>a7bdc60</code></a> refactor(lib): importing Unpin is not needed in 2021 edition</li>
<li><a href="https://github.com/hyperium/hyper/commit/00a703a9ef268266f8a8f78540253cbb2dcc6a55"><code>00a703a</code></a> chore(ci): update to cargo-check-external-types 0.1.11</li>
<li><a href="https://github.com/hyperium/hyper/commit/b0c1395aec0f3cdabf9d7af82dbd06418b72a24c"><code>b0c1395</code></a> refactor(error): resolve unused trait error</li>
<li><a href="https://github.com/hyperium/hyper/commit/7206fe30302937075c51c16a69d1eb3bbce6a671"><code>7206fe3</code></a> fix(rt): <code>Sleep::downcast_mut_pin()</code> no longer extend lifetime</li>
<li><a href="https://github.com/hyperium/hyper/commit/90eb95f62a32981cb662b0f750027231d8a2586b"><code>90eb95f</code></a> chore(lib): remove importing prelude trait in 2021 edition (<a href="https://redirect.github.com/hyperium/hyper/issues/3546">#3546</a>)</li>
<li><a href="https://github.com/hyperium/hyper/commit/b114244898828e9fb254bea1f0bbdd24850b2f3f"><code>b114244</code></a> feat(http1): support configurable <code>max_headers(num)</code> to client and server (<a href="https://redirect.github.com/hyperium/hyper/issues/3">#3</a>...</li>
<li><a href="https://github.com/hyperium/hyper/commit/7177770a66f61d40fb7aaa10bd04e3cd6dd3a6b9"><code>7177770</code></a> chore(lib): update to 2021 edition</li>
<li><a href="https://github.com/hyperium/hyper/commit/7a0a640dc7bec93c282f4ce8f46245a93ccd06fb"><code>7a0a640</code></a> docs(maintainers): add dswij (<a href="https://github.com/dswij"><code>@​dswij</code></a>) to triagers (<a href="https://redirect.github.com/hyperium/hyper/issues/3540">#3540</a>)</li>
<li><a href="https://github.com/hyperium/hyper/commit/79862ec2e84c32122c820958ceec06d8b7701ff7"><code>79862ec</code></a> fix(http2): typo in trace logging (<a href="https://redirect.github.com/hyperium/hyper/issues/3536">#3536</a>)</li>
<li>Additional commits viewable in <a href="https://github.com/hyperium/hyper/compare/v0.14.28...v1.2.0">compare view</a></li>
</ul>
</details>
<br />


[![Dependabot compatibility score](https://dependabot-badges.githubapp.com/badges/compatibility_score?dependency-name=hyper&package-manager=cargo&previous-version=0.14.28&new-version=1.2.0)](https://docs.github.com/en/github/managing-security-vulnerabilities/about-dependabot-security-updates#about-compatibility-scores)

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

_Label `internal` added by @dependabot[bot] on 2024-03-04 22:05_

---

_Comment by @charliermarsh on 2024-03-04 22:06_

@dependabot ignore this dependency

---

_Closed by @dependabot[bot] on 2024-03-04 22:06_

---

_Comment by @dependabot[bot] on 2024-03-04 22:06_

OK, I won't notify you about hyper again, unless you re-open this PR.

---

_Branch deleted on 2024-03-04 22:06_

---

_Comment by @charliermarsh on 2024-03-04 22:07_

reqwest does not support this yet.

---
