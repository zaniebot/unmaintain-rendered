```yaml
number: 9940
title: Bump chrono from 0.4.33 to 0.4.34
type: pull_request
state: merged
author: dependabot
labels:
  - internal
assignees: []
merged: true
base: main
head: dependabot/cargo/chrono-0.4.34
created_at: 2024-02-12T08:45:37Z
updated_at: 2024-02-12T09:24:17Z
url: https://github.com/astral-sh/ruff/pull/9940
synced_at: 2026-01-12T15:55:30Z
```

# Bump chrono from 0.4.33 to 0.4.34

---

_@dependabot_

Bumps [chrono](https://github.com/chronotope/chrono) from 0.4.33 to 0.4.34.
<details>
<summary>Release notes</summary>
<p><em>Sourced from <a href="https://github.com/chronotope/chrono/releases">chrono's releases</a>.</em></p>
<blockquote>
<h2>0.4.34</h2>
<h1>Notable changes</h1>
<ul>
<li>In chrono 0.4.34 we finished the work to make all methods const where doing so is supported by rust 1.61.</li>
<li>We renamed the <code>Duration</code> type to <code>TimeDelta</code>. This removes the confusion between chrono's type and the later <code>Duration</code> type in the standard library. It will remain available under the old name as a type alias for compatibility.</li>
<li>The Windows implementation of <code>Local</code> is rewritten. The new version avoids panics when the date is outside of the range supported by windows (the years 1601 to 30828), and gives more accurate results during DST transitions.</li>
<li>The <code>Display</code> format of <code>TimeDelta</code> is modified to conform better to ISO 8601. Previously it converted all values greater than 24 hours to a value with days. This is not correct, as doing so changes the duration from an 'accurate' to a 'nominal' representation to use ISO 8601 terms.</li>
</ul>
<h1>Fixes</h1>
<ul>
<li>Add missing range check in <code>TimeDelta::milliseconds</code> (<a href="https://redirect.github.com/chronotope/chrono/issues/1385">#1385</a>, thanks <a href="https://github.com/danwilliams"><code>@​danwilliams</code></a>)</li>
<li>Remove check for <code>DurationExceedsTimestamp</code> in <code>DurationRound</code> (<a href="https://redirect.github.com/chronotope/chrono/issues/1403">#1403</a>, thanks <a href="https://github.com/joroKr21"><code>@​joroKr21</code></a>)</li>
<li>Fix localized formatting with <code>%X</code> ((<a href="https://redirect.github.com/chronotope/pure-rust-locales/pull/12">chronotope/pure-rust-locales#12</a>, <a href="https://redirect.github.com/chronotope/chrono/issues/1420">#1420</a>)</li>
<li>Windows: base implementation on <code>GetTimeZoneInformationForYear</code> (<a href="https://redirect.github.com/chronotope/chrono/issues/1017">#1017</a>)</li>
</ul>
<h1>Additions</h1>
<ul>
<li>Add <code>TimeDelta::try_milliseconds</code> (<a href="https://redirect.github.com/chronotope/chrono/issues/1385">#1385</a>, thanks <a href="https://github.com/danwilliams"><code>@​danwilliams</code></a>)</li>
<li>Add <code>TimeDelta::new</code> (<a href="https://redirect.github.com/chronotope/chrono/issues/1337">#1337</a>)</li>
<li>Add <code>StrftimeItems::{parse, parse_to_owned}</code> and more documentation (<a href="https://redirect.github.com/chronotope/chrono/issues/1184">#1184</a>)</li>
<li>More standard traits and documentation for <code>format::Locale</code> (via <a href="https://redirect.github.com/chronotope/pure-rust-locales/pull/8">chronotope/pure-rust-locales#8</a>)</li>
</ul>
<h1>Changes</h1>
<ul>
<li>Rename <code>Duration</code> to <code>TimeDelta</code>, add type alias (<a href="https://redirect.github.com/chronotope/chrono/issues/1406">#1406</a>)</li>
<li>Make <code>TimeDelta</code> methods const (<a href="https://redirect.github.com/chronotope/chrono/issues/1337">#1337</a>)</li>
<li>Make remaining methods of <code>NaiveDate</code>, <code>NaiveWeek</code>, <code>NaiveTime</code> and <code>NaiveDateTime</code> const where possible (<a href="https://redirect.github.com/chronotope/chrono/issues/1337">#1337</a>)</li>
<li>Make methods on <code>DateTime</code> const where possible (<a href="https://redirect.github.com/chronotope/chrono/issues/1400">#1400</a>)</li>
<li>Make <code>Display</code> format of <code>TimeDelta</code> conform better to ISO 8601 (<a href="https://redirect.github.com/chronotope/chrono/issues/1328">#1328</a>)</li>
</ul>
<h1>Documentation</h1>
<ul>
<li>Fix the formatting of <code>timestamp_micros</code>'s Example doc (<a href="https://redirect.github.com/chronotope/chrono/issues/1338">#1338</a> via <a href="https://redirect.github.com/chronotope/chrono/issues/1386">#1386</a>, thanks <a href="https://github.com/emikitas"><code>@​emikitas</code></a>)</li>
<li>Specify branch for GitHub Actions badge and fix link (<a href="https://redirect.github.com/chronotope/chrono/issues/1388">#1388</a>)</li>
<li>Don't mention some deprecated methods in docs (<a href="https://redirect.github.com/chronotope/chrono/issues/1395">#1395</a>)</li>
<li>Remove stray documentation from main (<a href="https://redirect.github.com/chronotope/chrono/issues/1397">#1397</a>)</li>
<li>Improved documentation of <code>TimeDelta</code> constructors (<a href="https://redirect.github.com/chronotope/chrono/issues/1385">#1385</a>, thanks <a href="https://github.com/danwilliams"><code>@​danwilliams</code></a>)</li>
</ul>
<h1>Internal</h1>
<ul>
<li>Switch branch names: 0.4.x releases are the <code>main</code> branch, work on 0.5 happens in the <code>0.5.x</code> branch (<a href="https://redirect.github.com/chronotope/chrono/issues/1390">#1390</a>, <a href="https://redirect.github.com/chronotope/chrono/issues/1402">#1402</a>).</li>
<li>Don't use deprecated method in <code>impl Arbitrary for DateTime</code> and set up CI test (<a href="https://redirect.github.com/chronotope/chrono/issues/1336">#1336</a>)</li>
<li>Remove workaround for Rust &lt; 1.61 (<a href="https://redirect.github.com/chronotope/chrono/issues/1393">#1393</a>)</li>
<li>Bump <code>codecov/codecov-action</code> from 3 to 4 (<a href="https://redirect.github.com/chronotope/chrono/issues/1404">#1404</a>)</li>
<li>Remove partial support for handling <code>-0000</code> offset (<a href="https://redirect.github.com/chronotope/chrono/issues/1411">#1411</a>)</li>
<li>Move <code>TOO_LONG</code> error out of <code>parse_internal</code> (<a href="https://redirect.github.com/chronotope/chrono/issues/1419">#1419</a>)</li>
</ul>
<p>Thanks to all contributors on behalf of the chrono team, <a href="https://github.com/djc"><code>@​djc</code></a> and <a href="https://github.com/pitdicker"><code>@​pitdicker</code></a>!</p>
</blockquote>
</details>
<details>
<summary>Commits</summary>
<ul>
<li><a href="https://github.com/chronotope/chrono/commit/dc196062650c05528cbe259e340210f0340a05d1"><code>dc19606</code></a> Prepare 0.4.34</li>
<li><a href="https://github.com/chronotope/chrono/commit/58a2149a8ef5eaab7236bc447a4a37a3965b579b"><code>58a2149</code></a> Add <code>StrftimeItems::parse_to_owned</code></li>
<li><a href="https://github.com/chronotope/chrono/commit/59eeb8c5374039cf79dcdf7379cd9e79a7d2d2ca"><code>59eeb8c</code></a> Add <code>StrftimeItems::parse</code></li>
<li><a href="https://github.com/chronotope/chrono/commit/79de122615b7d7497c236e37c7f88e9ca14993f6"><code>79de122</code></a> Add more documentation to <code>StrftimeItems::new_with_locale</code></li>
<li><a href="https://github.com/chronotope/chrono/commit/5b7cf85c5305939503bf2114b91864f7e192bfb2"><code>5b7cf85</code></a> Add more documentation to <code>StrftimeItems::new</code></li>
<li><a href="https://github.com/chronotope/chrono/commit/be6af79ae5258cc0720a270816474cdbf761cacf"><code>be6af79</code></a> Make <code>Display</code> format of <code>TimeDelta</code> conform better to ISO 8601</li>
<li><a href="https://github.com/chronotope/chrono/commit/d1cf0e93935f5c41af1ae91678c7ea7d8478bb02"><code>d1cf0e9</code></a> Add test for issue 651</li>
<li><a href="https://github.com/chronotope/chrono/commit/0ef34e4da331f8f3a580b24c429c27170e5a3d05"><code>0ef34e4</code></a> Extend test to more distant dates</li>
<li><a href="https://github.com/chronotope/chrono/commit/fc67f3e6e0b41635bdf93b45ca09b8ac54672bb6"><code>fc67f3e</code></a> Remove obsolete test</li>
<li><a href="https://github.com/chronotope/chrono/commit/acb693ac6428ee31daef8206e5aae6d15d88fa09"><code>acb693a</code></a> Windows: rewrite using <code>GetTimeZoneInformationForYear</code></li>
<li>Additional commits viewable in <a href="https://github.com/chronotope/chrono/compare/v0.4.33...v0.4.34">compare view</a></li>
</ul>
</details>
<br />


[![Dependabot compatibility score](https://dependabot-badges.githubapp.com/badges/compatibility_score?dependency-name=chrono&package-manager=cargo&previous-version=0.4.33&new-version=0.4.34)](https://docs.github.com/en/github/managing-security-vulnerabilities/about-dependabot-security-updates#about-compatibility-scores)

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

_Label `internal` added by @dependabot[bot] on 2024-02-12 08:45_

---

_Comment by @github-actions[bot] on 2024-02-12 09:03_

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

_Merged by @MichaReiser on 2024-02-12 09:24_

---

_Closed by @MichaReiser on 2024-02-12 09:24_

---

_Branch deleted on 2024-02-12 09:24_

---
