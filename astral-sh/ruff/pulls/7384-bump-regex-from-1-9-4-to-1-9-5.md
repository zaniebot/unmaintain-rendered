```yaml
number: 7384
title: Bump regex from 1.9.4 to 1.9.5
type: pull_request
state: merged
author: dependabot
labels:
  - internal
assignees: []
merged: true
base: main
head: dependabot/cargo/regex-1.9.5
created_at: 2023-09-14T09:03:34Z
updated_at: 2023-09-14T15:31:52Z
url: https://github.com/astral-sh/ruff/pull/7384
synced_at: 2026-01-12T02:39:10Z
```

# Bump regex from 1.9.4 to 1.9.5

---

_Pull request opened by @dependabot on 2023-09-14 09:03_

Bumps [regex](https://github.com/rust-lang/regex) from 1.9.4 to 1.9.5.
<details>
<summary>Changelog</summary>
<p><em>Sourced from <a href="https://github.com/rust-lang/regex/blob/master/CHANGELOG.md">regex's changelog</a>.</em></p>
<blockquote>
<h1>1.9.5 (2023-09-02)</h1>
<p>This is a patch release that hopefully mostly fixes a performance bug that
occurs when sharing a regex across multiple threads.</p>
<p>Issue <a href="https://redirect.github.com/rust-lang/regex/issues/934">#934</a>
explains this in more detail. It is <a href="https://docs.rs/regex/latest/regex/#sharing-a-regex-across-threads-can-result-in-contention">also noted in the crate
documentation</a>.
The bug can appear when sharing a regex across multiple threads simultaneously,
as might be the case when using a regex from a <code>OnceLock</code>, <code>lazy_static</code> or
similar primitive. Usually high contention only results when using many threads
to execute searches on small haystacks.</p>
<p>One can avoid the contention problem entirely through one of two methods.
The first is to use lower level APIs from <code>regex-automata</code> that require passing
state explicitly, such as <a href="https://docs.rs/regex-automata/latest/regex_automata/meta/struct.Regex.html#method.search_with"><code>meta::Regex::search_with</code></a>.
The second is to clone a regex and send it to other threads explicitly. This
will not use any additional memory usage compared to sharing the regex. The
only downside of this approach is that it may be less convenient, for example,
it won't work with things like <code>OnceLock</code> or <code>lazy_static</code> or <code>once_cell</code>.</p>
<p>With that said, as of this release, the contention performance problems have
been greatly reduced. This was achieved by changing the free-list so that it
was sharded across threads, and that ensuring each sharded mutex occupies a
single cache line to mitigate false sharing. So while contention may still
impact performance in some cases, it should be a lot better now.</p>
<p>Because of the changes to how the free-list works, please report any issues you
find with this release. That not only includes search time regressions but also
significant regressions in memory usage. Reporting improvements is also welcome
as well! If possible, provide a reproduction.</p>
<p>Bug fixes:</p>
<ul>
<li>[BUG <a href="https://redirect.github.com/rust-lang/regex/issues/934">#934</a>](<a href="https://redirect.github.com/rust-lang/regex/issues/934">rust-lang/regex#934</a>):
Fix a performance bug where high contention on a single regex led to massive
slow downs.</li>
</ul>
</blockquote>
</details>
<details>
<summary>Commits</summary>
<ul>
<li><a href="https://github.com/rust-lang/regex/commit/554469b8c1116322f3c0a054ceeb610224f8ac65"><code>554469b</code></a> 1.9.5</li>
<li><a href="https://github.com/rust-lang/regex/commit/48e09a85e46d2b8cc379cd0b69cd98467639f7ff"><code>48e09a8</code></a> deps: bump regex-automata to 0.3.8</li>
<li><a href="https://github.com/rust-lang/regex/commit/894dcbe11e45d08b23db24f877574e06f3a69a35"><code>894dcbe</code></a> regex-automata-0.3.8</li>
<li><a href="https://github.com/rust-lang/regex/commit/135e11ba9c54b383072ae98043c31dfe1066886a"><code>135e11b</code></a> changelog: 1.9.5</li>
<li><a href="https://github.com/rust-lang/regex/commit/f578d74ff42f3df408378ff52d3bdf4433423532"><code>f578d74</code></a> automata: reduce regex contention somewhat</li>
<li><a href="https://github.com/rust-lang/regex/commit/9a505a1804f8f89e3448a2a2c5c70573dc6362e5"><code>9a505a1</code></a> deps: bump to memchr 2.6</li>
<li><a href="https://github.com/rust-lang/regex/commit/15cdc64869ea5508d96a0e7667c44c7c459986a1"><code>15cdc64</code></a> cli: remove use of deprecated API</li>
<li><a href="https://github.com/rust-lang/regex/commit/329c6a32451434fc2f229ad8d3c934c70148ae45"><code>329c6a3</code></a> ci: use dtolnay@master instead of <a href="https://github.com/v1"><code>@​v1</code></a></li>
<li><a href="https://github.com/rust-lang/regex/commit/2637e11b9fb9f3dc8bbfc3cbc625fd454c091d04"><code>2637e11</code></a> ci: remove stale comment</li>
<li>See full diff in <a href="https://github.com/rust-lang/regex/compare/1.9.4...1.9.5">compare view</a></li>
</ul>
</details>
<br />


[![Dependabot compatibility score](https://dependabot-badges.githubapp.com/badges/compatibility_score?dependency-name=regex&package-manager=cargo&previous-version=1.9.4&new-version=1.9.5)](https://docs.github.com/en/github/managing-security-vulnerabilities/about-dependabot-security-updates#about-compatibility-scores)

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

_Label `internal` added by @dependabot[bot] on 2023-09-14 09:03_

---

_Comment by @codspeed-hq[bot] on 2023-09-14 09:21_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/dependabot/cargo/regex-1.9.5)

### Merging #7384 will **not alter performance**

<sub>Comparing <code>dependabot/cargo/regex-1.9.5</code> (3ef4af2) with <code>main</code> (45eabdd)</sub>



### Summary

`✅ 25` untouched benchmarks






---

_Comment by @MichaReiser on 2023-09-14 09:22_

@charliermarsh do you know if there are more IO based rules in the linter other than 

```
    rules.disable(Rule::ShebangMissingExecutableFile);
    rules.disable(Rule::ShebangNotExecutable);
```

Or does the resolver run as part of the linter?

---

_Comment by @github-actions[bot] on 2023-09-14 09:42_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.



---

_@zanieb approved on 2023-09-14 14:14_

---

_Merged by @MichaReiser on 2023-09-14 14:58_

---

_Closed by @MichaReiser on 2023-09-14 14:58_

---

_Branch deleted on 2023-09-14 14:58_

---

_Comment by @charliermarsh on 2023-09-14 15:31_

@MichaReiser - I can think of a few things... For example, we always run the `detect_package_roots` stuff to find `__init__.py` file, but that's not specific to the `all-rules` benchmark. I'll look into it a bit more...

---
