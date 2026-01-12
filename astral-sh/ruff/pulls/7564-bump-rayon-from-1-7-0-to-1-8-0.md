```yaml
number: 7564
title: Bump rayon from 1.7.0 to 1.8.0
type: pull_request
state: merged
author: dependabot
labels:
  - internal
assignees: []
merged: true
base: main
head: dependabot/cargo/rayon-1.8.0
created_at: 2023-09-21T08:57:22Z
updated_at: 2023-09-21T09:52:42Z
url: https://github.com/astral-sh/ruff/pull/7564
synced_at: 2026-01-12T15:55:24Z
```

# Bump rayon from 1.7.0 to 1.8.0

---

_@dependabot_

Bumps [rayon](https://github.com/rayon-rs/rayon) from 1.7.0 to 1.8.0.
<details>
<summary>Changelog</summary>
<p><em>Sourced from <a href="https://github.com/rayon-rs/rayon/blob/master/RELEASES.md">rayon's changelog</a>.</em></p>
<blockquote>
<h1>Release rayon 1.8.0 / rayon-core 1.12.0 (2023-09-20)</h1>
<ul>
<li>The minimum supported <code>rustc</code> is now 1.63.</li>
<li>Added <code>ThreadPoolBuilder::use_current_thread</code> to use the builder thread as
part of the new thread pool. That thread does not run the pool's main loop,
but it may participate in work-stealing if it yields to rayon in some way.</li>
<li>Implemented <code>FromParallelIterator&lt;T&gt;</code> for <code>Box&lt;[T]&gt;</code>, <code>Rc&lt;[T]&gt;</code>, and
<code>Arc&lt;[T]&gt;</code>, as well as <code>FromParallelIterator&lt;Box&lt;str&gt;&gt;</code> and
<code>ParallelExtend&lt;Box&lt;str&gt;&gt;</code> for <code>String</code>.</li>
<li><code>ThreadPoolBuilder::build_scoped</code> now uses <code>std::thread::scope</code>.</li>
<li>The default number of threads is now determined using
<code>std::thread::available_parallelism</code> instead of the <code>num_cpus</code> crate.</li>
<li>The internal logging facility has been removed, reducing bloat for all users.</li>
<li>Many smaller performance tweaks and documentation updates.</li>
</ul>
</blockquote>
</details>
<details>
<summary>Commits</summary>
<ul>
<li><a href="https://github.com/rayon-rs/rayon/commit/21e1ae1e1251eece2a731d905415047207b80849"><code>21e1ae1</code></a> Release rayon 1.4.0 / rayon-core 1.8.0</li>
<li><a href="https://github.com/rayon-rs/rayon/commit/a0e5833b76b36de1072d0700153b5fd10e1bc0d8"><code>a0e5833</code></a> Merge <a href="https://redirect.github.com/rayon-rs/rayon/issues/785">#785</a> <a href="https://redirect.github.com/rayon-rs/rayon/issues/790">#790</a> <a href="https://redirect.github.com/rayon-rs/rayon/issues/791">#791</a></li>
<li><a href="https://github.com/rayon-rs/rayon/commit/9f7357befbe886d861b6d741a7a7be5348bca59b"><code>9f7357b</code></a> Merge <a href="https://redirect.github.com/rayon-rs/rayon/issues/792">#792</a></li>
<li><a href="https://github.com/rayon-rs/rayon/commit/998f134242d62eea2126a8de22ca7c86ca063898"><code>998f134</code></a> Removed outdated documentation</li>
<li><a href="https://github.com/rayon-rs/rayon/commit/c7d963a9c242c915c4ed320fc79a0634d506ccd0"><code>c7d963a</code></a> Use crossbeam_deque::Injector instead of crossbeam_queue::SegQueue</li>
<li><a href="https://github.com/rayon-rs/rayon/commit/2e889293a88a5d1e6e65529ede89a2c723186a07"><code>2e88929</code></a> Micro-optimize the WorkerThread::steal loop</li>
<li><a href="https://github.com/rayon-rs/rayon/commit/66559fe9ce1e4f05ac011b65172c6a3ca9bf2ea9"><code>66559fe</code></a> Remove the lifetime constraint from the scope OP</li>
<li><a href="https://github.com/rayon-rs/rayon/commit/09428ec11d26d82d06ad6e848df504565e3b47ae"><code>09428ec</code></a> Merge <a href="https://redirect.github.com/rayon-rs/rayon/issues/746">#746</a></li>
<li><a href="https://github.com/rayon-rs/rayon/commit/ed6a5f75c4dc1cb0ac4f59f6f01b38f8a695a777"><code>ed6a5f7</code></a> Update ci/compat-Cargo.lock</li>
<li><a href="https://github.com/rayon-rs/rayon/commit/96ba9ef188b4bd5f20b162fb9b861df134e27e9a"><code>96ba9ef</code></a> inline more Counter methods</li>
<li>Additional commits viewable in <a href="https://github.com/rayon-rs/rayon/compare/rayon-core-v1.7.0...rayon-core-v1.8.0">compare view</a></li>
</ul>
</details>
<br />


[![Dependabot compatibility score](https://dependabot-badges.githubapp.com/badges/compatibility_score?dependency-name=rayon&package-manager=cargo&previous-version=1.7.0&new-version=1.8.0)](https://docs.github.com/en/github/managing-security-vulnerabilities/about-dependabot-security-updates#about-compatibility-scores)

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

_Label `internal` added by @dependabot[bot] on 2023-09-21 08:57_

---

_Comment by @codspeed-hq[bot] on 2023-09-21 09:08_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/dependabot/cargo/rayon-1.8.0)

### Merging #7564 will **degrade performances by 2.27%**

<sub>Comparing <code>dependabot/cargo/rayon-1.8.0</code> (757cd2f) with <code>main</code> (f8f1cd5)</sub>



### Summary

`❌ 1` regressions
`✅ 24` untouched benchmarks



> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/ruff/branches/dependabot/cargo/rayon-1.8.0)._

### Benchmarks breakdown

|     | Benchmark | `main` | `dependabot/cargo/rayon-1.8.0` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ❌ | `linter/default-rules[large/dataset.py]` | 92 ms | 94.1 ms | -2.27% |


---

_Comment by @github-actions[bot] on 2023-09-21 09:13_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.



---

_Merged by @MichaReiser on 2023-09-21 09:52_

---

_Closed by @MichaReiser on 2023-09-21 09:52_

---

_Branch deleted on 2023-09-21 09:52_

---
