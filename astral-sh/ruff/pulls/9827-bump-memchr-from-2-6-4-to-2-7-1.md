```yaml
number: 9827
title: Bump memchr from 2.6.4 to 2.7.1
type: pull_request
state: merged
author: dependabot
labels:
  - internal
assignees: []
merged: true
base: main
head: dependabot/cargo/memchr-2.7.1
created_at: 2024-02-05T08:30:46Z
updated_at: 2024-02-05T13:24:24Z
url: https://github.com/astral-sh/ruff/pull/9827
synced_at: 2026-01-12T15:55:30Z
```

# Bump memchr from 2.6.4 to 2.7.1

---

_@dependabot_

Bumps [memchr](https://github.com/BurntSushi/memchr) from 2.6.4 to 2.7.1.
<details>
<summary>Commits</summary>
<ul>
<li><a href="https://github.com/BurntSushi/memchr/commit/31c1e7911e45f17db761aa1f152896d92d782587"><code>31c1e79</code></a> 2.7.1</li>
<li><a href="https://github.com/BurntSushi/memchr/commit/d9ac66d726e1529217a9586685c609a080f38388"><code>d9ac66d</code></a> api: impl Clone for FindRevIter</li>
<li><a href="https://github.com/BurntSushi/memchr/commit/8957028d162d04bc29b5ccb22b79bbb86a239619"><code>8957028</code></a> benchmarks/engines/rust-memchr: complete bump to 2.7.0</li>
<li><a href="https://github.com/BurntSushi/memchr/commit/5caaf3e736f9b6b7bfadcfee064d2914331a33fd"><code>5caaf3e</code></a> benchmarks/engines/rust-memchr: bump to 2.7.0</li>
<li><a href="https://github.com/BurntSushi/memchr/commit/b93d817ea62ee42da6d47280be2310a6154a0518"><code>b93d817</code></a> 2.7.0</li>
<li><a href="https://github.com/BurntSushi/memchr/commit/8b62928c7bd4e215c2c4827f7eb9dfc264eb465a"><code>8b62928</code></a> cargo: remove unused exclusions</li>
<li><a href="https://github.com/BurntSushi/memchr/commit/a22b2df27d7c445ef1e6f6e0d4c67d215700fb42"><code>a22b2df</code></a> ci: update to wasmtime 15</li>
<li><a href="https://github.com/BurntSushi/memchr/commit/bce19408dd5c145fded8bbbb4ec8d76a937bf885"><code>bce1940</code></a> benchmarks/engines/bytecount: revert to 0.6.4</li>
<li><a href="https://github.com/BurntSushi/memchr/commit/2f5d8c4842c2cb3b404d9afe311340653b3f1bf3"><code>2f5d8c4</code></a> benchmarks: fix wasmtime command</li>
<li><a href="https://github.com/BurntSushi/memchr/commit/e77f0bf07ae3445d77776aabe233a9352dd753c5"><code>e77f0bf</code></a> arch: simplify and improve is_equal_raw</li>
<li>Additional commits viewable in <a href="https://github.com/BurntSushi/memchr/compare/2.6.4...2.7.1">compare view</a></li>
</ul>
</details>
<br />


[![Dependabot compatibility score](https://dependabot-badges.githubapp.com/badges/compatibility_score?dependency-name=memchr&package-manager=cargo&previous-version=2.6.4&new-version=2.7.1)](https://docs.github.com/en/github/managing-security-vulnerabilities/about-dependabot-security-updates#about-compatibility-scores)

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

_Label `internal` added by @dependabot[bot] on 2024-02-05 08:30_

---

_Comment by @github-actions[bot] on 2024-02-05 08:50_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **encountered linter errors**. (no lint changes; 1 project error)

<details><summary><a href="https://github.com/sphinx-doc/sphinx">sphinx-doc/sphinx</a> (error)</summary>
<p>

```
ruff failed
  Cause: Selection of unstable rules without the `--preview` flag is not allowed. Enable preview or remove selection of:
	- FURB113
	- FURB131
	- FURB132
```

</p>
</details>

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
ℹ️ ecosystem check **encountered format errors**. (no format changes; 2 project errors)

<details><summary><a href="https://github.com/sphinx-doc/sphinx">sphinx-doc/sphinx</a> (error)</summary>
<p>
<pre>ruff format --no-preview --exclude tests/roots/test-pycode/cp_1251_coded.py</pre>
</p>
<p>

```
ruff failed
  Cause: Selection of unstable rules without the `--preview` flag is not allowed. Enable preview or remove selection of:
	- FURB113
	- FURB131
	- FURB132
```

</p>
</details>
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

_Merged by @MichaReiser on 2024-02-05 13:24_

---

_Closed by @MichaReiser on 2024-02-05 13:24_

---

_Branch deleted on 2024-02-05 13:24_

---
