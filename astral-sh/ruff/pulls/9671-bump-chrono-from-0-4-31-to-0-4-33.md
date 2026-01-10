```yaml
number: 9671
title: Bump chrono from 0.4.31 to 0.4.33
type: pull_request
state: merged
author: dependabot
labels:
  - internal
assignees: []
merged: true
base: main
head: dependabot/cargo/chrono-0.4.33
created_at: 2024-01-29T09:07:41Z
updated_at: 2024-01-29T10:01:47Z
url: https://github.com/astral-sh/ruff/pull/9671
synced_at: 2026-01-10T22:57:09Z
```

# Bump chrono from 0.4.31 to 0.4.33

---

_Pull request opened by @dependabot on 2024-01-29 09:07_

Bumps [chrono](https://github.com/chronotope/chrono) from 0.4.31 to 0.4.33.
<details>
<summary>Release notes</summary>
<p><em>Sourced from <a href="https://github.com/chronotope/chrono/releases">chrono's releases</a>.</em></p>
<blockquote>
<h2>0.4.33</h2>
<p>This release fixes the broken docrs.rs build of <a href="https://github.com/chronotope/chrono/releases/tag/v0.4.32">chrono 0.4.32</a>.</p>
<h2>What's Changed</h2>
<ul>
<li>Make <code>rkyv</code> feature imply <code>size_32</code> (<a href="https://redirect.github.com/chronotope/chrono/issues/1383">#1383</a>)</li>
<li>Fixed typo in <code>Duration::hours()</code> exception (<a href="https://redirect.github.com/chronotope/chrono/issues/1384">#1384</a>, thanks <a href="https://github.com/danwilliams"><code>@​danwilliams</code></a>)</li>
</ul>
<h2>v0.4.32</h2>
<p>In this release we shipped part of the effort to reduce the number of methods that could unexpectedly panic, notably for the <code>DateTime</code> and <code>Duration</code> types.</p>
<p>Chrono internally stores the value of a <code>DateTime</code> in UTC, and transparently converts it to the local value as required. For example adding a second to a <code>DateTime</code> needs to be done in UTC to get the correct result, but adding a day needs to be done in local time to be correct. What happens when the value is near the edge of the representable range, and the implicit conversions pushes it beyond the representable range? <em>Many</em> methods could panic on such inputs, including formatting the value for <code>Debug</code> output.</p>
<p>In chrono 0.4.32 the range of <code>NaiveDate</code>, <code>NaiveDateTime</code> and <code>DateTime</code> is made slightly smaller. This allows us to always do the implicit conversion, and in many cases return the expected result. Specifically the range is now from January 1, -262144 until December 31, 262143, one year less on both sides than before. We expect this may trip up tests if you hardcoded the <code>MIN</code> and <code>MAX</code> dates.</p>
<p><code>Duration</code> had a similar issue. The range of this type was pretty arbitrary picked to match the range of an <code>i64</code> in milliseconds. Negating an <code>i64::MIN</code> pushes a value out of range, and in the same way negating <code>Duration::MIN</code> could push it out of our defined range and cause a panic. This turns out to be somewhat common and hidden behind many layers of abstraction. We adjusted the type to have a minimum value of <code>-Duration::MAX</code> instead and prevent the panic case.</p>
<p>Other highlights:</p>
<ul>
<li><code>Duration</code> gained new fallible initialization methods.</li>
<li>Better support for <code>rkyv</code>.</li>
<li>Most methods on <code>NaiveDateTime</code> are now const.</li>
<li>We had to bump our MSRV to 1.61 to keep building with our dependencies. This will also allow us to make more methods on <code>DateTime</code> const in a future release.</li>
</ul>
<p>Complete list of changes:</p>
<h2>Fixes</h2>
<ul>
<li>Fix panic in <code>TimeZone::from_local_datetime</code> (<a href="https://redirect.github.com/chronotope/chrono/issues/1071">#1071</a>)</li>
<li>Fix out of range panics in <code>DateTime</code> getters and setters (<a href="https://redirect.github.com/chronotope/chrono/issues/1317">#1317</a>, <a href="https://redirect.github.com/chronotope/chrono/issues/1329">#1329</a>)</li>
</ul>
<h2>Additions</h2>
<ul>
<li>Add <code>NaiveDateTime::checked_(add|sub)_offset</code> (<a href="https://redirect.github.com/chronotope/chrono/issues/1313">#1313</a>)</li>
<li>Add <code>DateTime::to_utc</code> (<a href="https://redirect.github.com/chronotope/chrono/issues/1325">#1325</a>)</li>
<li>Duration features part 1 (<a href="https://redirect.github.com/chronotope/chrono/issues/1327">#1327</a>)</li>
<li>Make methods on <code>NaiveDateTime</code> const where possible (<a href="https://redirect.github.com/chronotope/chrono/issues/1286">#1286</a>)</li>
<li>Split <code>clock</code> feature into <code>clock</code> and <code>now</code> (<a href="https://redirect.github.com/chronotope/chrono/issues/1343">#1343</a>, thanks <a href="https://github.com/mmastrac"><code>@​mmastrac</code></a>)</li>
<li>Add <code>From&lt;NaiveDate&gt;</code> for <code>NaiveDateTime</code> (<a href="https://redirect.github.com/chronotope/chrono/issues/1355">#1355</a>, thanks <a href="https://github.com/dcechano"><code>@​dcechano</code></a>)</li>
<li>Add <code>NaiveDateTime::from_timestamp_nanos</code> (<a href="https://redirect.github.com/chronotope/chrono/issues/1357">#1357</a>, thanks <a href="https://github.com/Ali-Mirghasemi"><code>@​Ali-Mirghasemi</code></a>)</li>
<li>Add <code>Months::num_months()</code> and <code>num_years()</code> (<a href="https://redirect.github.com/chronotope/chrono/issues/1373">#1373</a>, thanks <a href="https://github.com/danwilliams"><code>@​danwilliams</code></a>)</li>
<li>Add <code>DateTime&lt;Utc&gt;::from_timestamp_millis</code> (<a href="https://redirect.github.com/chronotope/chrono/issues/1374">#1374</a>, thanks <a href="https://github.com/xmakro"><code>@​xmakro</code></a>)</li>
</ul>
<h2>Changes</h2>
<ul>
<li>Fix panic in <code>Duration::MIN.abs()</code> (adjust <code>Duration::MIN</code> by 1 millisecond) (<a href="https://redirect.github.com/chronotope/chrono/issues/1334">#1334</a>)</li>
<li>Bump MSRV to 1.61 (<a href="https://redirect.github.com/chronotope/chrono/issues/1347">#1347</a>)</li>
<li>Update windows-targets requirement from 0.48 to 0.52 (<a href="https://redirect.github.com/chronotope/chrono/issues/1360">#1360</a>)</li>
<li>Update windows-bindgen to 0.52 (<a href="https://redirect.github.com/chronotope/chrono/issues/1379">#1379</a>)</li>
</ul>
<h2>Deprecations</h2>
<ul>
<li>Deprecate standalone <code>format</code> functions (<a href="https://redirect.github.com/chronotope/chrono/issues/1306">#1306</a>)</li>
</ul>
<h2>Documentation</h2>
<ul>
<li>Improve doc comment and tests for timestamp_nanos_opt (<a href="https://redirect.github.com/chronotope/chrono/issues/1299">#1299</a>, thanks <a href="https://github.com/mlegner"><code>@​mlegner</code></a>)</li>
</ul>
<!-- raw HTML omitted -->
</blockquote>
<p>... (truncated)</p>
</details>
<details>
<summary>Commits</summary>
<ul>
<li><a href="https://github.com/chronotope/chrono/commit/7c419a358e8978fa774e649c300196095365eb58"><code>7c419a3</code></a> Prepare 0.4.33 release</li>
<li><a href="https://github.com/chronotope/chrono/commit/a9b37c4c81de09b5b420b617ee6394f30ce491b1"><code>a9b37c4</code></a> Make <code>rkyv</code> feature default to <code>size_32</code></li>
<li><a href="https://github.com/chronotope/chrono/commit/a73b54320ac24a770b2e451e60e84eb52a6acbf9"><code>a73b543</code></a> Don't assume <code>rkyv-(16|32|64)</code> implies the <code>rkyv</code> feature</li>
<li><a href="https://github.com/chronotope/chrono/commit/b5381f8fb5c2ba32260288dc2fc43a711ed2549d"><code>b5381f8</code></a> Fixed typo in Duration::hours() exception</li>
<li><a href="https://github.com/chronotope/chrono/commit/bf704191f2914a2db43abf6827a73cbe05864d57"><code>bf70419</code></a> 52</li>
<li><a href="https://github.com/chronotope/chrono/commit/7757386368913e8df9a8505f9ef37c5868045a88"><code>7757386</code></a> Prepare 0.4.32 release</li>
<li><a href="https://github.com/chronotope/chrono/commit/cee242a6565bffcc2a90c8566136c3b96491a821"><code>cee242a</code></a> Fix typos in Datelike impl for DateTime</li>
<li><a href="https://github.com/chronotope/chrono/commit/6ec8f97d16ce320a6f948b1cd1494ed3a21b251f"><code>6ec8f97</code></a> Add from_timestamp_millis to DateTime&lt;Utc&gt; (<a href="https://redirect.github.com/chronotope/chrono/issues/1374">#1374</a>)</li>
<li><a href="https://github.com/chronotope/chrono/commit/65f0cc2aa41800a93878dfbeb816b182cc027b1d"><code>65f0cc2</code></a> CI Linting: Fix missing sources checkout in <code>toml</code> job.</li>
<li><a href="https://github.com/chronotope/chrono/commit/5536687c0d4dfae66130cc44c04e08d9f2797708"><code>5536687</code></a> Add Months::as_u32() (<a href="https://redirect.github.com/chronotope/chrono/issues/1373">#1373</a>)</li>
<li>Additional commits viewable in <a href="https://github.com/chronotope/chrono/compare/v0.4.31...v0.4.33">compare view</a></li>
</ul>
</details>
<br />


[![Dependabot compatibility score](https://dependabot-badges.githubapp.com/badges/compatibility_score?dependency-name=chrono&package-manager=cargo&previous-version=0.4.31&new-version=0.4.33)](https://docs.github.com/en/github/managing-security-vulnerabilities/about-dependabot-security-updates#about-compatibility-scores)

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

_Label `internal` added by @dependabot[bot] on 2024-01-29 09:07_

---

_Comment by @github-actions[bot] on 2024-01-29 09:26_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
ℹ️ ecosystem check **encountered format errors**. (no format changes; 1 project error)

<details><summary><a href="https://github.com/openai/openai-cookbook">openai/openai-cookbook</a> (error)</summary>
<p>

```
warning: Detected debug build without --no-cache.
error: Failed to parse examples/dalle/Image_generations_edits_and_variations_with_DALL-E.ipynb:3:7:8: Unexpected token 'prompt'
```

</p>
</details>

### Formatter (preview)
ℹ️ ecosystem check **encountered format errors**. (no format changes; 1 project error)

<details><summary><a href="https://github.com/openai/openai-cookbook">openai/openai-cookbook</a> (error)</summary>
<p>
<pre>ruff format --preview</pre>
</p>
<p>

```
warning: Detected debug build without --no-cache.
error: Failed to parse examples/dalle/Image_generations_edits_and_variations_with_DALL-E.ipynb:3:7:8: Unexpected token 'prompt'
```

</p>
</details>




---

_Merged by @MichaReiser on 2024-01-29 10:01_

---

_Closed by @MichaReiser on 2024-01-29 10:01_

---

_Branch deleted on 2024-01-29 10:01_

---
