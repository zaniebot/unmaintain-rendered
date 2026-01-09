---
number: 6065
title: "chore(deps): bump openssl from 0.10.64 to 0.10.73"
type: pull_request
state: open
author: dependabot
labels:
  - dependencies
  - rust
assignees: []
base: master
head: dependabot/cargo/openssl-0.10.73
created_at: 2025-07-08T21:11:24Z
updated_at: 2025-08-07T21:28:54Z
url: https://github.com/clap-rs/clap/pull/6065
synced_at: 2026-01-07T13:12:20-06:00
---

# chore(deps): bump openssl from 0.10.64 to 0.10.73

---

_Pull request opened by @dependabot on 2025-07-08 21:11_

Bumps [openssl](https://github.com/sfackler/rust-openssl) from 0.10.64 to 0.10.73.
<details>
<summary>Release notes</summary>
<p><em>Sourced from <a href="https://github.com/sfackler/rust-openssl/releases">openssl's releases</a>.</em></p>
<blockquote>
<h2>openssl-v0.10.73</h2>
<h2>What's Changed</h2>
<ul>
<li>test against openssl 3.5.0 by <a href="https://github.com/alex"><code>@​alex</code></a> in <a href="https://redirect.github.com/sfackler/rust-openssl/pull/2392">sfackler/rust-openssl#2392</a></li>
<li>Support Libressl 4.1 by <a href="https://github.com/botovq"><code>@​botovq</code></a> in <a href="https://redirect.github.com/sfackler/rust-openssl/pull/2398">sfackler/rust-openssl#2398</a></li>
<li>Release openssl-sys v0.9.108 by <a href="https://github.com/alex"><code>@​alex</code></a> in <a href="https://redirect.github.com/sfackler/rust-openssl/pull/2399">sfackler/rust-openssl#2399</a></li>
<li>Replace ctest2 with ctest by <a href="https://github.com/botovq"><code>@​botovq</code></a> in <a href="https://redirect.github.com/sfackler/rust-openssl/pull/2403">sfackler/rust-openssl#2403</a></li>
<li>fixed building on the latest boringssl by <a href="https://github.com/alex"><code>@​alex</code></a> in <a href="https://redirect.github.com/sfackler/rust-openssl/pull/2414">sfackler/rust-openssl#2414</a></li>
<li>Release openssl v0.10.73 and openssl-sys v0.9.109 by <a href="https://github.com/alex"><code>@​alex</code></a> in <a href="https://redirect.github.com/sfackler/rust-openssl/pull/2415">sfackler/rust-openssl#2415</a></li>
</ul>
<p><strong>Full Changelog</strong>: <a href="https://github.com/sfackler/rust-openssl/compare/openssl-v0.10.72...openssl-v0.10.73">https://github.com/sfackler/rust-openssl/compare/openssl-v0.10.72...openssl-v0.10.73</a></p>
<h2>openssl-v0.10.72</h2>
<h2>What's Changed</h2>
<ul>
<li>make set_rsa_oaep_md visible to boringssl config by <a href="https://github.com/frncs-rss"><code>@​frncs-rss</code></a> in <a href="https://redirect.github.com/sfackler/rust-openssl/pull/2372">sfackler/rust-openssl#2372</a></li>
<li>Fix typo in openssl-sys build script by <a href="https://github.com/rushilmehra"><code>@​rushilmehra</code></a> in <a href="https://redirect.github.com/sfackler/rust-openssl/pull/2375">sfackler/rust-openssl#2375</a></li>
<li>Unify the two BoringSSL codepaths a bit and simplify init by <a href="https://github.com/davidben"><code>@​davidben</code></a> in <a href="https://redirect.github.com/sfackler/rust-openssl/pull/2377">sfackler/rust-openssl#2377</a></li>
<li>pkey_ctx: Fix link to the corresponding OpenSSL function by <a href="https://github.com/Jakuje"><code>@​Jakuje</code></a> in <a href="https://redirect.github.com/sfackler/rust-openssl/pull/2378">sfackler/rust-openssl#2378</a></li>
<li>fix test on MSRV by <a href="https://github.com/alex"><code>@​alex</code></a> in <a href="https://redirect.github.com/sfackler/rust-openssl/pull/2383">sfackler/rust-openssl#2383</a></li>
<li>Add support for AWS-LC to openssl and openssl-sys crates by <a href="https://github.com/skmcgrail"><code>@​skmcgrail</code></a> in <a href="https://redirect.github.com/sfackler/rust-openssl/pull/1805">sfackler/rust-openssl#1805</a></li>
<li>Enable additional capabilities for AWS-LC by <a href="https://github.com/skmcgrail"><code>@​skmcgrail</code></a> in <a href="https://redirect.github.com/sfackler/rust-openssl/pull/2386">sfackler/rust-openssl#2386</a></li>
<li>Use --experimental with bindgen-cli with aws-lc build by <a href="https://github.com/skmcgrail"><code>@​skmcgrail</code></a> in <a href="https://redirect.github.com/sfackler/rust-openssl/pull/2389">sfackler/rust-openssl#2389</a></li>
<li>Fixed two UAFs and bumped versions for release by <a href="https://github.com/alex"><code>@​alex</code></a> in <a href="https://redirect.github.com/sfackler/rust-openssl/pull/2390">sfackler/rust-openssl#2390</a></li>
</ul>
<h2>New Contributors</h2>
<ul>
<li><a href="https://github.com/Jakuje"><code>@​Jakuje</code></a> made their first contribution in <a href="https://redirect.github.com/sfackler/rust-openssl/pull/2378">sfackler/rust-openssl#2378</a></li>
<li><a href="https://github.com/skmcgrail"><code>@​skmcgrail</code></a> made their first contribution in <a href="https://redirect.github.com/sfackler/rust-openssl/pull/1805">sfackler/rust-openssl#1805</a></li>
</ul>
<p><strong>Full Changelog</strong>: <a href="https://github.com/sfackler/rust-openssl/compare/openssl-v0.10.71...openssl-v0.10.72">https://github.com/sfackler/rust-openssl/compare/openssl-v0.10.71...openssl-v0.10.72</a></p>
<h2>openssl-v0.10.71</h2>
<h2>What's Changed</h2>
<ul>
<li>Expose rc2 ciphers on symm::Cipher by <a href="https://github.com/alex"><code>@​alex</code></a> in <a href="https://redirect.github.com/sfackler/rust-openssl/pull/2361">sfackler/rust-openssl#2361</a></li>
<li>add full Apache license file to openssl by <a href="https://github.com/frncs-rss"><code>@​frncs-rss</code></a> in <a href="https://redirect.github.com/sfackler/rust-openssl/pull/2366">sfackler/rust-openssl#2366</a></li>
<li>Release openssl v0.10.71 and openssl-sys v0.9.106 by <a href="https://github.com/alex"><code>@​alex</code></a> in <a href="https://redirect.github.com/sfackler/rust-openssl/pull/2369">sfackler/rust-openssl#2369</a></li>
</ul>
<h2>New Contributors</h2>
<ul>
<li><a href="https://github.com/frncs-rss"><code>@​frncs-rss</code></a> made their first contribution in <a href="https://redirect.github.com/sfackler/rust-openssl/pull/2366">sfackler/rust-openssl#2366</a></li>
</ul>
<p><strong>Full Changelog</strong>: <a href="https://github.com/sfackler/rust-openssl/compare/openssl-v0.10.70...openssl-v0.10.71">https://github.com/sfackler/rust-openssl/compare/openssl-v0.10.70...openssl-v0.10.71</a></p>
<h2>openssl v0.10.70</h2>
<h2>What's Changed</h2>
<ul>
<li>Attempt to fix CI by pinning to the Ubuntu 22.04 image by <a href="https://github.com/alex"><code>@​alex</code></a> in <a href="https://redirect.github.com/sfackler/rust-openssl/pull/2357">sfackler/rust-openssl#2357</a></li>
<li>Remove EC_METHOD and EC_GROUP_new for LibreSSL 4.1 by <a href="https://github.com/botovq"><code>@​botovq</code></a> in <a href="https://redirect.github.com/sfackler/rust-openssl/pull/2356">sfackler/rust-openssl#2356</a></li>
<li>Test against 3.4.0 final release by <a href="https://github.com/alex"><code>@​alex</code></a> in <a href="https://redirect.github.com/sfackler/rust-openssl/pull/2359">sfackler/rust-openssl#2359</a></li>
<li>Expose <code>SslMethod::{dtls_client,dtls_server}</code> by <a href="https://github.com/alex"><code>@​alex</code></a> in <a href="https://redirect.github.com/sfackler/rust-openssl/pull/2358">sfackler/rust-openssl#2358</a></li>
<li>Fix lifetimes in ssl::select_next_proto by <a href="https://github.com/sfackler"><code>@​sfackler</code></a> in <a href="https://redirect.github.com/sfackler/rust-openssl/pull/2360">sfackler/rust-openssl#2360</a></li>
</ul>
<!-- raw HTML omitted -->
</blockquote>
<p>... (truncated)</p>
</details>
<details>
<summary>Commits</summary>
<ul>
<li><a href="https://github.com/sfackler/rust-openssl/commit/e6209d43c55a972b602972a8f219d60c5fb2fe70"><code>e6209d4</code></a> Merge pull request <a href="https://redirect.github.com/sfackler/rust-openssl/issues/2415">#2415</a> from alex/bump-version</li>
<li><a href="https://github.com/sfackler/rust-openssl/commit/9ca6cfe2e68e676afb9f160a6efc656473d26e6c"><code>9ca6cfe</code></a> Release openssl v0.10.73 and openssl-sys v0.9.109</li>
<li><a href="https://github.com/sfackler/rust-openssl/commit/c42d49c1cac3e4cc0b68f7ea632892b2eb71324f"><code>c42d49c</code></a> Merge pull request <a href="https://redirect.github.com/sfackler/rust-openssl/issues/2414">#2414</a> from alex/boringssl-fix</li>
<li><a href="https://github.com/sfackler/rust-openssl/commit/5e24219c18c69f99b18e5a0d6d4ec4552593648f"><code>5e24219</code></a> Attempt to fix with vcpkg</li>
<li><a href="https://github.com/sfackler/rust-openssl/commit/93f30ff3726b76b72044142bc817892016d0d005"><code>93f30ff</code></a> fixed building on the latest boringssl</li>
<li><a href="https://github.com/sfackler/rust-openssl/commit/eb88fb0533c3593cc2fff6d03cf2befea8ecbe27"><code>eb88fb0</code></a> Merge pull request <a href="https://redirect.github.com/sfackler/rust-openssl/issues/2403">#2403</a> from botovq/ctest</li>
<li><a href="https://github.com/sfackler/rust-openssl/commit/79a304a364711cbf562763f3de4d49f2af07f5e4"><code>79a304a</code></a> Replace ctest2 with ctest</li>
<li><a href="https://github.com/sfackler/rust-openssl/commit/132418b3a1f7adf59f0b47261d5fe817c44359cd"><code>132418b</code></a> Merge pull request <a href="https://redirect.github.com/sfackler/rust-openssl/issues/2399">#2399</a> from alex/release-sys</li>
<li><a href="https://github.com/sfackler/rust-openssl/commit/f7a692bc2fd330c925085c3f66ec9ba6ffe55211"><code>f7a692b</code></a> Release openssl-sys v0.9.108</li>
<li><a href="https://github.com/sfackler/rust-openssl/commit/2f9b4965210cd42c2215cc42e6da67b7dfb772e4"><code>2f9b496</code></a> Merge pull request <a href="https://redirect.github.com/sfackler/rust-openssl/issues/2398">#2398</a> from botovq/libressl-4.1</li>
<li>Additional commits viewable in <a href="https://github.com/sfackler/rust-openssl/compare/openssl-v0.10.64...openssl-v0.10.73">compare view</a></li>
</ul>
</details>
<br />


[![Dependabot compatibility score](https://dependabot-badges.githubapp.com/badges/compatibility_score?dependency-name=openssl&package-manager=cargo&previous-version=0.10.64&new-version=0.10.73)](https://docs.github.com/en/github/managing-security-vulnerabilities/about-dependabot-security-updates#about-compatibility-scores)

You can trigger a rebase of this PR by commenting `@dependabot rebase`.

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
You can disable automated security fix PRs for this repo from the [Security Alerts page](https://github.com/clap-rs/clap/network/alerts).

</details>

> **Note**
> Automatic rebases have been disabled on this pull request as it has been open for over 30 days.


---
