```yaml
number: 7481
title: Bump chrono from 0.4.30 to 0.4.31
type: pull_request
state: merged
author: dependabot
labels:
  - internal
assignees: []
merged: true
base: main
head: dependabot/cargo/chrono-0.4.31
created_at: 2023-09-18T08:43:39Z
updated_at: 2023-09-18T17:11:52Z
url: https://github.com/astral-sh/ruff/pull/7481
synced_at: 2026-01-12T15:55:24Z
```

# Bump chrono from 0.4.30 to 0.4.31

---

_@dependabot_

Bumps [chrono](https://github.com/chronotope/chrono) from 0.4.30 to 0.4.31.
<details>
<summary>Release notes</summary>
<p><em>Sourced from <a href="https://github.com/chronotope/chrono/releases">chrono's releases</a>.</em></p>
<blockquote>
<h2>0.4.31</h2>
<p>Another maintenance release.
It was not a planned effort to improve our support for UNIX timestamps, yet most PRs seem related to this.</p>
<h3>Deprecations</h3>
<ul>
<li>Deprecate <code>timestamp_nanos</code> in favor of the non-panicking <code>timestamp_nanos_opt</code> (<a href="https://redirect.github.com/chronotope/chrono/issues/1275">#1275</a>)</li>
</ul>
<h3>Additions</h3>
<ul>
<li>Add <code>DateTime::&lt;Utc&gt;::from_timestamp</code> (<a href="https://redirect.github.com/chronotope/chrono/issues/1279">#1279</a>, thanks <a href="https://github.com/demurgos"><code>@​demurgos</code></a>)</li>
<li>Add <code>TimeZone::timestamp_micros</code> (<a href="https://redirect.github.com/chronotope/chrono/issues/1285">#1285</a>, thanks <a href="https://github.com/emikitas"><code>@​emikitas</code></a>)</li>
<li>Add <code>DateTime&lt;Tz&gt;::timestamp_nanos_opt</code> and <code>NaiveDateTime::timestamp_nanos_opt</code> (<a href="https://redirect.github.com/chronotope/chrono/issues/1275">#1275</a>)</li>
<li>Add <code>UNIX_EPOCH</code> constants (<a href="https://redirect.github.com/chronotope/chrono/issues/1291">#1291</a>)</li>
</ul>
<h3>Fixes</h3>
<ul>
<li>Format day of month in RFC 2822 without padding (<a href="https://redirect.github.com/chronotope/chrono/issues/1272">#1272</a>)</li>
<li>Don't allow strange leap seconds which are not on a minute boundary initialization methods (<a href="https://redirect.github.com/chronotope/chrono/issues/1283">#1283</a>)
This makes many methods a little more strict:
<ul>
<li><code>NaiveTime::from_hms_milli</code></li>
<li><code>NaiveTime::from_hms_milli_opt</code></li>
<li><code>NaiveTime::from_hms_micro</code></li>
<li><code>NaiveTime::from_hms_micro_opt</code></li>
<li><code>NaiveTime::from_hms_nano</code></li>
<li><code>NaiveTime::from_hms_nano_opt</code></li>
<li><code>NaiveTime::from_num_seconds_from_midnight</code></li>
<li><code>NaiveTime::from_num_seconds_from_midnight_opt</code></li>
<li><code>NaiveDate::and_hms_milli</code></li>
<li><code>NaiveDate::and_hms_milli_opt</code></li>
<li><code>NaiveDate::and_hms_micro</code></li>
<li><code>NaiveDate::and_hms_micro_opt</code></li>
<li><code>NaiveDate::and_hms_nano</code></li>
<li><code>NaiveDate::and_hms_nano_opt</code></li>
<li><code>NaiveDateTime::from_timestamp</code></li>
<li><code>NaiveDateTime::from_timestamp_opt</code></li>
<li><code>TimeZone::timestamp</code></li>
<li><code>TimeZone::timestamp_opt</code></li>
</ul>
</li>
<li>Fix underflow in <code>NaiveDateTime::timestamp_nanos_opt</code> (<a href="https://redirect.github.com/chronotope/chrono/issues/1294">#1294</a>, thanks <a href="https://github.com/crepererum"><code>@​crepererum</code></a>)</li>
</ul>
<h3>Documentation</h3>
<ul>
<li>Add more documentation about the RFC 2822 obsolete date format (<a href="https://redirect.github.com/chronotope/chrono/issues/1267">#1267</a>)</li>
</ul>
<h3>Internal</h3>
<ul>
<li>Remove internal <code>__doctest</code> feature and <code>doc_comment</code> dependency (<a href="https://redirect.github.com/chronotope/chrono/issues/1276">#1276</a>)</li>
<li>CI: Bump <code>actions/checkout</code> from 3 to 4 (<a href="https://redirect.github.com/chronotope/chrono/issues/1280">#1280</a>)</li>
<li>Optimize <code>NaiveDate::add_days</code> for small values (<a href="https://redirect.github.com/chronotope/chrono/issues/1214">#1214</a>)</li>
<li>Upgrade <code>pure-rust-locales</code> to 0.7.0 (<a href="https://redirect.github.com/chronotope/chrono/issues/1288">#1288</a>, thanks <a href="https://github.com/jeremija"><code>@​jeremija</code></a> wo did good improvements on <code>pure-rust-locales</code>)</li>
</ul>
<p>Thanks to all contributors on behalf of the chrono team, <a href="https://github.com/djc"><code>@​djc</code></a> and <a href="https://github.com/pitdicker"><code>@​pitdicker</code></a>!</p>
</blockquote>
</details>
<details>
<summary>Commits</summary>
<ul>
<li><a href="https://github.com/chronotope/chrono/commit/e730c6ac45649a6a636abf30b796304bc46ecd15"><code>e730c6a</code></a> Bump version to 0.4.31</li>
<li><a href="https://github.com/chronotope/chrono/commit/2afdde8f7f23f087b5027662e2882dba0663fef7"><code>2afdde8</code></a> fix: underflow during datetime-&gt;nanos conversion</li>
<li><a href="https://github.com/chronotope/chrono/commit/46ad2c2b2c901eb20e43a7fca025aac02605bda4"><code>46ad2c2</code></a> Add <code>UNIX_EPOCH</code> constants</li>
<li><a href="https://github.com/chronotope/chrono/commit/1df8db3a547bf50929d3d1774306f996b63e9e7c"><code>1df8db3</code></a> Add TimeZone::timestamp_micros</li>
<li><a href="https://github.com/chronotope/chrono/commit/861d4e12487181e71744478a320db20f56bd61af"><code>861d4e1</code></a> Make TimeZone::timestamp_millis_opt use</li>
<li><a href="https://github.com/chronotope/chrono/commit/3c4846a88235a38105a047b0acd34ce239a1cfb5"><code>3c4846a</code></a> Upgrade pure-rust-locales to 0.7.0</li>
<li><a href="https://github.com/chronotope/chrono/commit/6665804676e55e9e2375eed7c10cc9e0910abf11"><code>6665804</code></a> Deny leap second if secs != 59 in <code>from_num_seconds_from_midnight_opt</code></li>
<li><a href="https://github.com/chronotope/chrono/commit/61b7ffbb7a95df577c308eb0f2ab5c68e1566cf1"><code>61b7ffb</code></a> Deny leap second if secs != 59 in <code>from_hms_nano_opt</code></li>
<li><a href="https://github.com/chronotope/chrono/commit/202af6cfda9bfdb195dc96377fcdeee7ed024b65"><code>202af6c</code></a> Don't generate leap seconds that are not 60 in NaiveTime's Arbitrary impl</li>
<li><a href="https://github.com/chronotope/chrono/commit/60283ab55cbc9ea6caa86e19bf5da2c086cdf4bc"><code>60283ab</code></a> Don't create strange leap seconds in tests</li>
<li>Additional commits viewable in <a href="https://github.com/chronotope/chrono/compare/v0.4.30...v0.4.31">compare view</a></li>
</ul>
</details>
<br />


[![Dependabot compatibility score](https://dependabot-badges.githubapp.com/badges/compatibility_score?dependency-name=chrono&package-manager=cargo&previous-version=0.4.30&new-version=0.4.31)](https://docs.github.com/en/github/managing-security-vulnerabilities/about-dependabot-security-updates#about-compatibility-scores)

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

_Label `internal` added by @dependabot[bot] on 2023-09-18 08:43_

---

_Comment by @github-actions[bot] on 2023-09-18 09:05_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.



---

_Merged by @charliermarsh on 2023-09-18 17:11_

---

_Closed by @charliermarsh on 2023-09-18 17:11_

---

_Branch deleted on 2023-09-18 17:11_

---
