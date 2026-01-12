```yaml
number: 1939
title: Bump itertools from 0.10.5 to 0.12.1
type: pull_request
state: merged
author: dependabot
labels:
  - internal
assignees: []
merged: true
base: main
head: dependabot/cargo/itertools-0.12.1
created_at: 2024-02-23T19:47:06Z
updated_at: 2024-02-23T20:01:12Z
url: https://github.com/astral-sh/uv/pull/1939
synced_at: 2026-01-12T16:04:48Z
```

# Bump itertools from 0.10.5 to 0.12.1

---

_@dependabot_

Bumps [itertools](https://github.com/rust-itertools/itertools) from 0.10.5 to 0.12.1.
<details>
<summary>Changelog</summary>
<p><em>Sourced from <a href="https://github.com/rust-itertools/itertools/blob/master/CHANGELOG.md">itertools's changelog</a>.</em></p>
<blockquote>
<h2>0.12.1</h2>
<h3>Added</h3>
<ul>
<li>Documented iteration order guarantee for <code>Itertools::[tuple_]combinations</code> (<a href="https://redirect.github.com/rust-itertools/itertools/issues/822">#822</a>)</li>
<li>Documented possible panic in <code>iterate</code> (<a href="https://redirect.github.com/rust-itertools/itertools/issues/842">#842</a>)</li>
<li>Implemented <code>Clone</code> and <code>Debug</code> for <code>Diff</code> (<a href="https://redirect.github.com/rust-itertools/itertools/issues/845">#845</a>)</li>
<li>Implemented <code>Debug</code> for <code>WithPosition</code> (<a href="https://redirect.github.com/rust-itertools/itertools/issues/859">#859</a>)</li>
<li>Implemented <code>Eq</code> for <code>MinMaxResult</code> (<a href="https://redirect.github.com/rust-itertools/itertools/issues/838">#838</a>)</li>
<li>Implemented <code>From&lt;EitherOrBoth&lt;A, B&gt;&gt;</code> for <code>Option&lt;Either&lt;A, B&gt;&gt;</code> (<a href="https://redirect.github.com/rust-itertools/itertools/issues/843">#843</a>)</li>
<li>Implemented <code>PeekingNext</code> for <code>RepeatN</code> (<a href="https://redirect.github.com/rust-itertools/itertools/issues/855">#855</a>)</li>
</ul>
<h3>Changed</h3>
<ul>
<li>Made <code>CoalesceBy</code> lazy (<a href="https://redirect.github.com/rust-itertools/itertools/issues/801">#801</a>)</li>
<li>Optimized <code>Filter[Map]Ok::next</code>, <code>Itertools::partition</code>, <code>Unique[By]::next[_back]</code> (<a href="https://redirect.github.com/rust-itertools/itertools/issues/818">#818</a>)</li>
<li>Optimized <code>Itertools::find_position</code> (<a href="https://redirect.github.com/rust-itertools/itertools/issues/837">#837</a>)</li>
<li>Optimized <code>Positions::next[_back]</code> (<a href="https://redirect.github.com/rust-itertools/itertools/issues/816">#816</a>)</li>
<li>Optimized <code>ZipLongest::fold</code> (<a href="https://redirect.github.com/rust-itertools/itertools/issues/854">#854</a>)</li>
<li>Relaxed <code>Debug</code> bounds for <code>GroupingMapBy</code> (<a href="https://redirect.github.com/rust-itertools/itertools/issues/860">#860</a>)</li>
<li>Specialized <code>ExactlyOneError::fold</code> (<a href="https://redirect.github.com/rust-itertools/itertools/issues/826">#826</a>)</li>
<li>Specialized <code>Interleave[Shortest]::fold</code> (<a href="https://redirect.github.com/rust-itertools/itertools/issues/849">#849</a>)</li>
<li>Specialized <code>MultiPeek::fold</code> (<a href="https://redirect.github.com/rust-itertools/itertools/issues/820">#820</a>)</li>
<li>Specialized <code>PadUsing::[r]fold</code> (<a href="https://redirect.github.com/rust-itertools/itertools/issues/825">#825</a>)</li>
<li>Specialized <code>PeekNth::fold</code> (<a href="https://redirect.github.com/rust-itertools/itertools/issues/824">#824</a>)</li>
<li>Specialized <code>Positions::[r]fold</code> (<a href="https://redirect.github.com/rust-itertools/itertools/issues/813">#813</a>)</li>
<li>Specialized <code>PutBackN::fold</code> (<a href="https://redirect.github.com/rust-itertools/itertools/issues/823">#823</a>)</li>
<li>Specialized <code>RepeatN::[r]fold</code> (<a href="https://redirect.github.com/rust-itertools/itertools/issues/821">#821</a>)</li>
<li>Specialized <code>TakeWhileInclusive::fold</code> (<a href="https://redirect.github.com/rust-itertools/itertools/issues/851">#851</a>)</li>
<li>Specialized <code>ZipLongest::rfold</code> (<a href="https://redirect.github.com/rust-itertools/itertools/issues/848">#848</a>)</li>
</ul>
<h3>Notable Internal Changes</h3>
<ul>
<li>Added test coverage in CI (<a href="https://redirect.github.com/rust-itertools/itertools/issues/847">#847</a>, <a href="https://redirect.github.com/rust-itertools/itertools/issues/856">#856</a>)</li>
<li>Added semver check in CI (<a href="https://redirect.github.com/rust-itertools/itertools/issues/784">#784</a>)</li>
<li>Enforced <code>clippy</code> in CI (<a href="https://redirect.github.com/rust-itertools/itertools/issues/740">#740</a>)</li>
<li>Enforced <code>rustdoc</code> in CI (<a href="https://redirect.github.com/rust-itertools/itertools/issues/840">#840</a>)</li>
<li>Improved specialization tests (<a href="https://redirect.github.com/rust-itertools/itertools/issues/807">#807</a>)</li>
<li>More specialization benchmarks (<a href="https://redirect.github.com/rust-itertools/itertools/issues/806">#806</a>)</li>
</ul>
<h2>0.12.0</h2>
<h3>Breaking</h3>
<ul>
<li>Made <code>take_while_inclusive</code> consume iterator by value (<a href="https://redirect.github.com/rust-itertools/itertools/issues/709">#709</a>)</li>
<li>Added <code>Clone</code> bound to <code>Unique</code> (<a href="https://redirect.github.com/rust-itertools/itertools/issues/777">#777</a>)</li>
</ul>
<h3>Added</h3>
<ul>
<li>Added <code>Itertools::try_len</code> (<a href="https://redirect.github.com/rust-itertools/itertools/issues/723">#723</a>)</li>
<li>Added free function <code>sort_unstable</code> (<a href="https://redirect.github.com/rust-itertools/itertools/issues/796">#796</a>)</li>
<li>Added <code>GroupMap::fold_with</code> (<a href="https://redirect.github.com/rust-itertools/itertools/issues/778">#778</a>, <a href="https://redirect.github.com/rust-itertools/itertools/issues/785">#785</a>)</li>
<li>Added <code>PeekNth::{peek_mut, peek_nth_mut}</code> (<a href="https://redirect.github.com/rust-itertools/itertools/issues/716">#716</a>)</li>
<li>Added <code>PeekNth::{next_if, next_if_eq}</code> (<a href="https://redirect.github.com/rust-itertools/itertools/issues/734">#734</a>)</li>
<li>Added conversion into <code>(Option&lt;A&gt;,Option&lt;B&gt;)</code> to <code>EitherOrBoth</code> (<a href="https://redirect.github.com/rust-itertools/itertools/issues/713">#713</a>)</li>
</ul>
<!-- raw HTML omitted -->
</blockquote>
<p>... (truncated)</p>
</details>
<details>
<summary>Commits</summary>
<ul>
<li><a href="https://github.com/rust-itertools/itertools/commit/98d3978c87d6aa8d35e3f459dc0089130e04b646"><code>98d3978</code></a> Prepare v0.12.1 release</li>
<li><a href="https://github.com/rust-itertools/itertools/commit/dffac1fde4f0c1454e3542d1530cce96e78063f3"><code>dffac1f</code></a> Bump obi1kenobi/cargo-semver-checks-action from 2.2 to 2.3</li>
<li><a href="https://github.com/rust-itertools/itertools/commit/00998a4bbc0fee91ceef380e7c46d8865fa22b95"><code>00998a4</code></a> <code>CoalesceBy</code>: missing field in <code>Debug</code></li>
<li><a href="https://github.com/rust-itertools/itertools/commit/a0411d6c6f03057d13e7282cc64071a1b5976beb"><code>a0411d6</code></a> <code>CombinationsWithReplacement</code>: use a boxed slice internally</li>
<li><a href="https://github.com/rust-itertools/itertools/commit/8dd75f155cd88813e438fc23c373dc6716c1f8d7"><code>8dd75f1</code></a> <code>Permutations</code>: use boxed slices internally</li>
<li><a href="https://github.com/rust-itertools/itertools/commit/b785403f5f96404e1580ddcf3ce22e02d178248b"><code>b785403</code></a> <code>ExactlyOneError</code>: implement Debug differently</li>
<li><a href="https://github.com/rust-itertools/itertools/commit/7a1c22be5efc6be366496d1cfce7ebac2386f02f"><code>7a1c22b</code></a> <code>FlattenOk</code>: Debug with macro</li>
<li><a href="https://github.com/rust-itertools/itertools/commit/94452e3eafb7c95fcbbb85c3dd78c274c6e43b62"><code>94452e3</code></a> <code>GroupingMapBy</code>: fix Debug implementation</li>
<li><a href="https://github.com/rust-itertools/itertools/commit/2e325a0bd466330c127907b1e0b6f5a7ebff40bf"><code>2e325a0</code></a> <code>TakeWhileInclusive</code>: missing field in <code>Debug</code></li>
<li><a href="https://github.com/rust-itertools/itertools/commit/a48c5b474bd005d184f23146a125ad1c94c54ab6"><code>a48c5b4</code></a> <code>WithPosition</code>: implement Debug</li>
<li>Additional commits viewable in <a href="https://github.com/rust-itertools/itertools/compare/v0.10.5...v0.12.1">compare view</a></li>
</ul>
</details>
<br />


[![Dependabot compatibility score](https://dependabot-badges.githubapp.com/badges/compatibility_score?dependency-name=itertools&package-manager=cargo&previous-version=0.10.5&new-version=0.12.1)](https://docs.github.com/en/github/managing-security-vulnerabilities/about-dependabot-security-updates#about-compatibility-scores)

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

_Label `internal` added by @dependabot[bot] on 2024-02-23 19:47_

---

_Merged by @zanieb on 2024-02-23 20:01_

---

_Closed by @zanieb on 2024-02-23 20:01_

---

_Branch deleted on 2024-02-23 20:01_

---
