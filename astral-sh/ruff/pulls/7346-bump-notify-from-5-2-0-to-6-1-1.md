```yaml
number: 7346
title: Bump notify from 5.2.0 to 6.1.1
type: pull_request
state: closed
author: dependabot
labels:
  - internal
assignees: []
base: main
head: dependabot/cargo/notify-6.1.1
created_at: 2023-09-13T15:57:17Z
updated_at: 2023-09-13T16:54:06Z
url: https://github.com/astral-sh/ruff/pull/7346
synced_at: 2026-01-12T15:55:23Z
```

# Bump notify from 5.2.0 to 6.1.1

---

_@dependabot_

Bumps [notify](https://github.com/notify-rs/notify) from 5.2.0 to 6.1.1.
<details>
<summary>Release notes</summary>
<p><em>Sourced from <a href="https://github.com/notify-rs/notify/releases">notify's releases</a>.</em></p>
<blockquote>
<h2>notify 6.1.1 (2023-08-21)</h2>
<ul>
<li>CHANGE: remove serde binary experiment opt-out after it got removed <a href="https://redirect.github.com/notify-rs/notify/issues/530">#530</a></li>
</ul>
<p><a href="https://redirect.github.com/notify-rs/notify/issues/530">#530</a>: <a href="https://redirect.github.com/notify-rs/notify/pull/530">notify-rs/notify#530</a></p>
<h2>notify 6.1.0 (2023-08-18)</h2>
<ul>
<li>CHANGE: opt-out of the serde binary experiment by restricting it to &lt; 1.0.172 <a href="https://redirect.github.com/notify-rs/notify/issues/528">#528</a></li>
<li>CHANGE: license changed to only CC0-1.0 <a href="https://redirect.github.com/notify-rs/notify/issues/520">#520</a></li>
<li>CHANGE: use logging <a href="https://redirect.github.com/notify-rs/notify/issues/499">#499</a></li>
<li>CHANGE: upgrade windows-sys to 0.48 <a href="https://redirect.github.com/notify-rs/notify/issues/479">#479</a></li>
<li>CHANGE: bump filetime to 0.2.22 <a href="https://redirect.github.com/notify-rs/notify/issues/521">#521</a></li>
<li>FEATURE: support manual polling of PollWatcher and disabling automatic polling <a href="https://redirect.github.com/notify-rs/notify/issues/524">#524</a></li>
<li>FEATURE: support listening to the initial pollwatcher file scan <a href="https://redirect.github.com/notify-rs/notify/issues/507">#507</a></li>
<li>FIX: fix moved folders not being watched on linux <a href="https://redirect.github.com/notify-rs/notify/issues/498">#498</a></li>
<li>FIX: fixup potential future double free on windows <a href="https://redirect.github.com/notify-rs/notify/issues/517">#517</a></li>
<li>FIX: require bitflags only on macos and upgrade the crate <a href="https://redirect.github.com/notify-rs/notify/issues/505">#505</a></li>
<li>DOCS: add more known issues, typos and cleanup examples <a href="https://redirect.github.com/notify-rs/notify/issues/523">#523</a> <a href="https://redirect.github.com/notify-rs/notify/issues/502">#502</a> <a href="https://redirect.github.com/notify-rs/notify/issues/522">#522</a></li>
</ul>
<p><a href="https://redirect.github.com/notify-rs/notify/issues/524">#524</a>: <a href="https://redirect.github.com/notify-rs/notify/pull/524">notify-rs/notify#524</a>
<a href="https://redirect.github.com/notify-rs/notify/issues/523">#523</a>: <a href="https://redirect.github.com/notify-rs/notify/pull/523">notify-rs/notify#523</a>
<a href="https://redirect.github.com/notify-rs/notify/issues/502">#502</a>: <a href="https://redirect.github.com/notify-rs/notify/pull/502">notify-rs/notify#502</a>
<a href="https://redirect.github.com/notify-rs/notify/issues/522">#522</a>: <a href="https://redirect.github.com/notify-rs/notify/pull/522">notify-rs/notify#522</a>
<a href="https://redirect.github.com/notify-rs/notify/issues/479">#479</a>: <a href="https://redirect.github.com/notify-rs/notify/pull/479">notify-rs/notify#479</a>
<a href="https://redirect.github.com/notify-rs/notify/issues/521">#521</a>: <a href="https://redirect.github.com/notify-rs/notify/pull/521">notify-rs/notify#521</a>
<a href="https://redirect.github.com/notify-rs/notify/issues/517">#517</a>: <a href="https://redirect.github.com/notify-rs/notify/pull/517">notify-rs/notify#517</a>
<a href="https://redirect.github.com/notify-rs/notify/issues/507">#507</a>: <a href="https://redirect.github.com/notify-rs/notify/pull/507">notify-rs/notify#507</a>
<a href="https://redirect.github.com/notify-rs/notify/issues/499">#499</a>: <a href="https://redirect.github.com/notify-rs/notify/pull/499">notify-rs/notify#499</a>
<a href="https://redirect.github.com/notify-rs/notify/issues/505">#505</a>: <a href="https://redirect.github.com/notify-rs/notify/pull/505">notify-rs/notify#505</a>
<a href="https://redirect.github.com/notify-rs/notify/issues/498">#498</a>: <a href="https://redirect.github.com/notify-rs/notify/pull/498">notify-rs/notify#498</a>
<a href="https://redirect.github.com/notify-rs/notify/issues/520">#520</a>: <a href="https://redirect.github.com/notify-rs/notify/pull/520">notify-rs/notify#520</a>
<a href="https://redirect.github.com/notify-rs/notify/issues/528">#528</a>: <a href="https://redirect.github.com/notify-rs/notify/pull/528">notify-rs/notify#528</a></p>
<h2>notify 6.0.1 (2023-06-16)</h2>
<ul>
<li>DOCS: fix swapped debouncer-full / -mini links in the readme/crates.io <a href="https://github.com/notify-rs/notify/commit/4be6bdef4fa7b260a3a56a11212ac074f8e39b39">4be6bde</a></li>
</ul>
<h2>notify 6.0.0 (2023-05-17)</h2>
<ul>
<li>CHANGE: files and directories moved into a watch folder on Linux will now be reported as <code>rename to</code> events instead of <code>create</code> events <a href="https://redirect.github.com/notify-rs/notify/issues/480">#480</a></li>
<li>CHANGE: on Linux <code>rename from</code> events will be emitted immediately without starting a new thread <a href="https://redirect.github.com/notify-rs/notify/issues/480">#480</a></li>
<li>CHANGE: raise MSRV to 1.60 <a href="https://redirect.github.com/notify-rs/notify/issues/480">#480</a></li>
</ul>
<p><a href="https://redirect.github.com/notify-rs/notify/issues/480">#480</a>: <a href="https://redirect.github.com/notify-rs/notify/pull/480">notify-rs/notify#480</a></p>
</blockquote>
</details>
<details>
<summary>Changelog</summary>
<p><em>Sourced from <a href="https://github.com/notify-rs/notify/blob/main/CHANGELOG.md">notify's changelog</a>.</em></p>
<blockquote>
<h2>notify 6.1.1 (2023-08-21)</h2>
<ul>
<li>CHANGE: remove serde binary experiment opt-out after it got removed <a href="https://redirect.github.com/notify-rs/notify/issues/530">#530</a></li>
</ul>
<h2>file-id 0.2.1 (2023-08-21)</h2>
<ul>
<li>CHANGE: remove serde binary experiment opt-out after it got removed <a href="https://redirect.github.com/notify-rs/notify/issues/530">#530</a></li>
</ul>
<p><a href="https://redirect.github.com/notify-rs/notify/issues/530">#530</a>: <a href="https://redirect.github.com/notify-rs/notify/pull/530">notify-rs/notify#530</a></p>
<h2>debouncer-full 0.3.0 (2023-08-18)</h2>
<ul>
<li>CHANGE: opt-out of the serde binary experiment by restricting it to &lt; 1.0.172 <a href="https://redirect.github.com/notify-rs/notify/issues/528">#528</a></li>
<li>CHANGE: license changed to dual-license of MIT OR Apache-2.0 <a href="https://redirect.github.com/notify-rs/notify/issues/520">#520</a></li>
<li>CHANGE: upgrade to file-id 0.2.0 for high resolution file IDs <a href="https://redirect.github.com/notify-rs/notify/issues/494">#494</a></li>
<li>FEATURE: derive debug for the debouncer struct <a href="https://redirect.github.com/notify-rs/notify/issues/510">#510</a></li>
</ul>
<h2>debouncer-mini 0.4.0 (2023-08-18)</h2>
<ul>
<li>CHANGE: opt-out of the serde binary experiment by restricting it to &lt; 1.0.172 <a href="https://redirect.github.com/notify-rs/notify/issues/528">#528</a></li>
<li>CHANGE: license changed to dual-license of MIT OR Apache-2.0 <a href="https://redirect.github.com/notify-rs/notify/issues/520">#520</a></li>
<li>CHANGE: replace active polling with passive loop, removing empty ticks <a href="https://redirect.github.com/notify-rs/notify/issues/467">#467</a></li>
<li>FEATURE: derive debug for the debouncer struct <a href="https://redirect.github.com/notify-rs/notify/issues/510">#510</a></li>
</ul>
<p><a href="https://redirect.github.com/notify-rs/notify/issues/467">#467</a>: <a href="https://redirect.github.com/notify-rs/notify/pull/467">notify-rs/notify#467</a>
<a href="https://redirect.github.com/notify-rs/notify/issues/510">#510</a>: <a href="https://redirect.github.com/notify-rs/notify/pull/510">notify-rs/notify#510</a></p>
<h2>notify 6.1.0 (2023-08-18)</h2>
<ul>
<li>CHANGE: opt-out of the serde binary experiment by restricting it to &lt; 1.0.172 <a href="https://redirect.github.com/notify-rs/notify/issues/528">#528</a></li>
<li>CHANGE: license changed to only CC0-1.0 <a href="https://redirect.github.com/notify-rs/notify/issues/520">#520</a></li>
<li>CHANGE: use logging <a href="https://redirect.github.com/notify-rs/notify/issues/499">#499</a></li>
<li>CHANGE: upgrade windows-sys to 0.48 <a href="https://redirect.github.com/notify-rs/notify/issues/479">#479</a></li>
<li>CHANGE: bump filetime to 0.2.22 <a href="https://redirect.github.com/notify-rs/notify/issues/521">#521</a></li>
<li>FEATURE: support manual polling of PollWatcher and disabling automatic polling <a href="https://redirect.github.com/notify-rs/notify/issues/524">#524</a></li>
<li>FEATURE: support listening to the initial pollwatcher file scan <a href="https://redirect.github.com/notify-rs/notify/issues/507">#507</a></li>
<li>FIX: fix moved folders not being watched on linux <a href="https://redirect.github.com/notify-rs/notify/issues/498">#498</a></li>
<li>FIX: fixup potential future double free on windows <a href="https://redirect.github.com/notify-rs/notify/issues/517">#517</a></li>
<li>FIX: require bitflags only on macos and upgrade the crate <a href="https://redirect.github.com/notify-rs/notify/issues/505">#505</a></li>
<li>DOCS: add more known issues, typos and cleanup examples <a href="https://redirect.github.com/notify-rs/notify/issues/523">#523</a> <a href="https://redirect.github.com/notify-rs/notify/issues/502">#502</a> <a href="https://redirect.github.com/notify-rs/notify/issues/522">#522</a></li>
</ul>
<p><a href="https://redirect.github.com/notify-rs/notify/issues/524">#524</a>: <a href="https://redirect.github.com/notify-rs/notify/pull/524">notify-rs/notify#524</a>
<a href="https://redirect.github.com/notify-rs/notify/issues/523">#523</a>: <a href="https://redirect.github.com/notify-rs/notify/pull/523">notify-rs/notify#523</a>
<a href="https://redirect.github.com/notify-rs/notify/issues/502">#502</a>: <a href="https://redirect.github.com/notify-rs/notify/pull/502">notify-rs/notify#502</a>
<a href="https://redirect.github.com/notify-rs/notify/issues/522">#522</a>: <a href="https://redirect.github.com/notify-rs/notify/pull/522">notify-rs/notify#522</a>
<a href="https://redirect.github.com/notify-rs/notify/issues/479">#479</a>: <a href="https://redirect.github.com/notify-rs/notify/pull/479">notify-rs/notify#479</a>
<a href="https://redirect.github.com/notify-rs/notify/issues/521">#521</a>: <a href="https://redirect.github.com/notify-rs/notify/pull/521">notify-rs/notify#521</a>
<a href="https://redirect.github.com/notify-rs/notify/issues/517">#517</a>: <a href="https://redirect.github.com/notify-rs/notify/pull/517">notify-rs/notify#517</a>
<a href="https://redirect.github.com/notify-rs/notify/issues/507">#507</a>: <a href="https://redirect.github.com/notify-rs/notify/pull/507">notify-rs/notify#507</a>
<a href="https://redirect.github.com/notify-rs/notify/issues/499">#499</a>: <a href="https://redirect.github.com/notify-rs/notify/pull/499">notify-rs/notify#499</a></p>
<!-- raw HTML omitted -->
</blockquote>
<p>... (truncated)</p>
</details>
<details>
<summary>Commits</summary>
<ul>
<li>See full diff in <a href="https://github.com/notify-rs/notify/commits/notify-6.1.1">compare view</a></li>
</ul>
</details>
<br />


[![Dependabot compatibility score](https://dependabot-badges.githubapp.com/badges/compatibility_score?dependency-name=notify&package-manager=cargo&previous-version=5.2.0&new-version=6.1.1)](https://docs.github.com/en/github/managing-security-vulnerabilities/about-dependabot-security-updates#about-compatibility-scores)

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

_Label `internal` added by @dependabot[bot] on 2023-09-13 15:57_

---

_Comment by @MichaReiser on 2023-09-13 16:02_

There's a license change and I want to test these changes manually.

---

_Closed by @MichaReiser on 2023-09-13 16:02_

---

_Comment by @dependabot[bot] on 2023-09-13 16:02_

OK, I won't notify you again about this release, but will get in touch when a new version is available. If you'd rather skip all updates until the next major or minor version, let me know by commenting `@dependabot ignore this major version` or `@dependabot ignore this minor version`. You can also ignore all major, minor, or patch releases for a dependency by adding an [`ignore` condition](https://docs.github.com/en/code-security/supply-chain-security/configuration-options-for-dependency-updates#ignore) with the desired `update_types` to your config file.

If you change your mind, just re-open this PR and I'll resolve any conflicts on it.

---

_Branch deleted on 2023-09-13 16:02_

---

_Comment by @github-actions[bot] on 2023-09-13 16:54_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.



---
