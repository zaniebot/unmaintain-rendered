```yaml
number: 8776
title: Bump wasm-bindgen from 0.2.87 to 0.2.88
type: pull_request
state: closed
author: dependabot
labels:
  - internal
assignees: []
base: main
head: dependabot/cargo/wasm-bindgen-0.2.88
created_at: 2023-11-20T08:17:13Z
updated_at: 2023-11-20T08:44:13Z
url: https://github.com/astral-sh/ruff/pull/8776
synced_at: 2026-01-10T23:40:55Z
```

# Bump wasm-bindgen from 0.2.87 to 0.2.88

---

_Pull request opened by @dependabot on 2023-11-20 08:17_

Bumps [wasm-bindgen](https://github.com/rustwasm/wasm-bindgen) from 0.2.87 to 0.2.88.
<details>
<summary>Changelog</summary>
<p><em>Sourced from <a href="https://github.com/rustwasm/wasm-bindgen/blob/main/CHANGELOG.md">wasm-bindgen's changelog</a>.</em></p>
<blockquote>
<h2><a href="https://github.com/rustwasm/wasm-bindgen/compare/0.2.87...0.2.88">0.2.88</a></h2>
<p>Released 2023-11-01</p>
<h3>Added</h3>
<ul>
<li>
<p>Add bindings for <code>RTCRtpTransceiverInit.sendEncodings</code>.
<a href="https://redirect.github.com/rustwasm/wasm-bindgen/pull/3642">#3642</a></p>
</li>
<li>
<p>Add bindings for the Web Locks API to <code>web-sys</code>.
<a href="https://redirect.github.com/rustwasm/wasm-bindgen/pull/3604">#3604</a></p>
</li>
<li>
<p>Add bindings for <code>ViewTransition</code> to <code>web-sys</code>.
<a href="https://redirect.github.com/rustwasm/wasm-bindgen/pull/3598">#3598</a></p>
</li>
<li>
<p>Extend <code>AudioContext</code> with unstable features supporting audio sink configuration.
<a href="https://redirect.github.com/rustwasm/wasm-bindgen/pull/3433">#3433</a></p>
</li>
<li>
<p>Add bindings for <code>WebAssembly.Tag</code> and <code>WebAssembly.Exception</code>.
<a href="https://redirect.github.com/rustwasm/wasm-bindgen/pull/3484">#3484</a></p>
</li>
<li>
<p>Re-export <code>wasm-bindgen</code> from <code>js-sys</code>, <code>web-sys</code> and <code>wasm-bindgen-futures</code>.
<a href="https://redirect.github.com/rustwasm/wasm-bindgen/pull/3466">#3466</a>
<a href="https://redirect.github.com/rustwasm/wasm-bindgen/pull/3601">#3601</a></p>
</li>
<li>
<p>Re-export <code>js-sys</code> from <code>web-sys</code> and <code>wasm-bindgen-futures</code>.
<a href="https://redirect.github.com/rustwasm/wasm-bindgen/pull/3466">#3466</a>
<a href="https://redirect.github.com/rustwasm/wasm-bindgen/pull/3601">#3601</a></p>
</li>
<li>
<p>Add bindings for async variants of <code>Atomics.wait</code>.
<a href="https://redirect.github.com/rustwasm/wasm-bindgen/pull/3504">#3504</a></p>
</li>
<li>
<p>Add bindings for <code>WorkerGlobalScope.performance</code>.
<a href="https://redirect.github.com/rustwasm/wasm-bindgen/pull/3506">#3506</a></p>
</li>
<li>
<p>Add support for installing pre-built artifacts of <code>wasm-bindgen-cli</code>
via <code>cargo binstall wasm-bindgen-cli</code>.
<a href="https://redirect.github.com/rustwasm/wasm-bindgen/pull/3544">#3544</a></p>
</li>
<li>
<p>Add bindings for <code>RTCDataChannel.id</code>.
<a href="https://redirect.github.com/rustwasm/wasm-bindgen/pull/3547">#3547</a></p>
</li>
<li>
<p>Add bindings for <code>HTMLElement.inert</code>.
<a href="https://redirect.github.com/rustwasm/wasm-bindgen/pull/3557">#3557</a></p>
</li>
<li>
<p>Add unstable bindings for the Prioritized Task Scheduling API.
<a href="https://redirect.github.com/rustwasm/wasm-bindgen/pull/3566">#3566</a></p>
</li>
<li>
<p>Add bindings for <code>CssStyleSheet</code> constructor and <code>replace(_sync)</code> methods.
<a href="https://redirect.github.com/rustwasm/wasm-bindgen/pull/3573">#3573</a></p>
</li>
</ul>
<!-- raw HTML omitted -->
</blockquote>
<p>... (truncated)</p>
</details>
<details>
<summary>Commits</summary>
<ul>
<li><a href="https://github.com/rustwasm/wasm-bindgen/commit/0b5f0eec2f3d5e75a923fd67ef14b3b5cc855c80"><code>0b5f0ee</code></a> Bump versions for v0.2.88 (<a href="https://redirect.github.com/rustwasm/wasm-bindgen/issues/3676">#3676</a>)</li>
<li><a href="https://github.com/rustwasm/wasm-bindgen/commit/bd596dcef1a9253f1566d4eb3ab67c8b3225ca53"><code>bd596dc</code></a> Configure git in bump workflow (<a href="https://redirect.github.com/rustwasm/wasm-bindgen/issues/3675">#3675</a>)</li>
<li><a href="https://github.com/rustwasm/wasm-bindgen/commit/ef7ad00cbe6a793e9528d52818fa803c0eef6e74"><code>ef7ad00</code></a> Add workflow to bump versions and create releases (<a href="https://redirect.github.com/rustwasm/wasm-bindgen/issues/3674">#3674</a>)</li>
<li><a href="https://github.com/rustwasm/wasm-bindgen/commit/a03d23bd16cb9b1706a71f20ba062e5532b369ec"><code>a03d23b</code></a> Updated Notification APIs (<a href="https://redirect.github.com/rustwasm/wasm-bindgen/issues/3667">#3667</a>)</li>
<li><a href="https://github.com/rustwasm/wasm-bindgen/commit/54f22ee0a593187ee102e085089319b51a58ea09"><code>54f22ee</code></a> Return an error if there are two enums with the same name (<a href="https://redirect.github.com/rustwasm/wasm-bindgen/issues/3669">#3669</a>)</li>
<li><a href="https://github.com/rustwasm/wasm-bindgen/commit/561d3c05843334283da756b4e76625ac7b31c3ae"><code>561d3c0</code></a> Update node from 16 to 20 (<a href="https://redirect.github.com/rustwasm/wasm-bindgen/issues/3666">#3666</a>)</li>
<li><a href="https://github.com/rustwasm/wasm-bindgen/commit/2e9ff5dfa3f11415f0efe9b946ee2734500e9ee3"><code>2e9ff5d</code></a> Add <code>rust-toolchain.toml</code> to raytracing example (<a href="https://redirect.github.com/rustwasm/wasm-bindgen/issues/3607">#3607</a>)</li>
<li><a href="https://github.com/rustwasm/wasm-bindgen/commit/3d2f392ccccb5eba2edd4281c74f26e4e333be5d"><code>3d2f392</code></a> Bump MSRV to v1.57 (<a href="https://redirect.github.com/rustwasm/wasm-bindgen/issues/3657">#3657</a>)</li>
<li><a href="https://github.com/rustwasm/wasm-bindgen/commit/cb64552c1f9e8a3d39f06143fcd2481c06c8ca13"><code>cb64552</code></a> use Vec's try_reserve_exact method (<a href="https://redirect.github.com/rustwasm/wasm-bindgen/issues/3656">#3656</a>)</li>
<li><a href="https://github.com/rustwasm/wasm-bindgen/commit/84b735c117737e084089d3e2d17af2b774ab26a5"><code>84b735c</code></a> Add missing changelog for <a href="https://redirect.github.com/rustwasm/wasm-bindgen/issues/3655">#3655</a></li>
<li>Additional commits viewable in <a href="https://github.com/rustwasm/wasm-bindgen/compare/0.2.87...0.2.88">compare view</a></li>
</ul>
</details>
<br />


[![Dependabot compatibility score](https://dependabot-badges.githubapp.com/badges/compatibility_score?dependency-name=wasm-bindgen&package-manager=cargo&previous-version=0.2.87&new-version=0.2.88)](https://docs.github.com/en/github/managing-security-vulnerabilities/about-dependabot-security-updates#about-compatibility-scores)

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

_Label `internal` added by @dependabot[bot] on 2023-11-20 08:17_

---

_Comment by @github-actions[bot] on 2023-11-20 08:39_

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

_Comment by @dependabot[bot] on 2023-11-20 08:44_

Looks like wasm-bindgen is up-to-date now, so this is no longer needed.

---

_Closed by @dependabot[bot] on 2023-11-20 08:44_

---

_Branch deleted on 2023-11-20 08:44_

---
