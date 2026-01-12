```yaml
number: 8347
title: Bump tj-actions/changed-files from 39 to 40
type: pull_request
state: merged
author: dependabot
labels:
  - internal
assignees: []
merged: true
base: main
head: dependabot/github_actions/tj-actions/changed-files-40
created_at: 2023-10-30T08:48:31Z
updated_at: 2023-10-30T09:07:04Z
url: https://github.com/astral-sh/ruff/pull/8347
synced_at: 2026-01-10T23:40:55Z
```

# Bump tj-actions/changed-files from 39 to 40

---

_Pull request opened by @dependabot on 2023-10-30 08:48_

Bumps [tj-actions/changed-files](https://github.com/tj-actions/changed-files) from 39 to 40.
<details>
<summary>Release notes</summary>
<p><em>Sourced from <a href="https://github.com/tj-actions/changed-files/releases">tj-actions/changed-files's releases</a>.</em></p>
<blockquote>
<h2>v40</h2>
<h1>Changes in v40.0.0</h1>
<h2>🔥 🔥  Breaking Change 🔥 🔥</h2>
<ul>
<li>Directory patterns now require explicit specification of the globstar pattern to match all sub paths.</li>
</ul>
<h3></h3>
<pre lang="diff"><code>...
      - name: Get specific changed files
        id: changed-files-specific
        uses: tj-actions/changed-files@v40
        with:
          files: |
-            dir
+            dir/**
</code></pre>
<h2>What's Changed</h2>
<ul>
<li>Upgraded to v39.2.4 by <a href="https://github.com/tj-actions-bot"><code>@​tj-actions-bot</code></a> in <a href="https://redirect.github.com/tj-actions/changed-files/pull/1664">tj-actions/changed-files#1664</a></li>
<li>chore(deps): lock file maintenance by <a href="https://github.com/renovate"><code>@​renovate</code></a> in <a href="https://redirect.github.com/tj-actions/changed-files/pull/1665">tj-actions/changed-files#1665</a></li>
<li>Bump <code>@​types/node</code> from 20.8.7 to 20.8.8 by <a href="https://github.com/dependabot"><code>@​dependabot</code></a> in <a href="https://redirect.github.com/tj-actions/changed-files/pull/1666">tj-actions/changed-files#1666</a></li>
<li>chore(deps): update dependency <code>@​types/node</code> to v20.8.9 by <a href="https://github.com/renovate"><code>@​renovate</code></a> in <a href="https://redirect.github.com/tj-actions/changed-files/pull/1668">tj-actions/changed-files#1668</a></li>
<li>remove: appending globstar pattern for directories to prevent bugs with path matching by <a href="https://github.com/jackton1"><code>@​jackton1</code></a> in <a href="https://redirect.github.com/tj-actions/changed-files/pull/1670">tj-actions/changed-files#1670</a></li>
<li>chore(deps): lock file maintenance by <a href="https://github.com/renovate"><code>@​renovate</code></a> in <a href="https://redirect.github.com/tj-actions/changed-files/pull/1671">tj-actions/changed-files#1671</a></li>
</ul>
<p><strong>Full Changelog</strong>: <a href="https://github.com/tj-actions/changed-files/compare/v39...v40.0.0">https://github.com/tj-actions/changed-files/compare/v39...v40.0.0</a></p>
<hr />
<h2>v40.0.0</h2>
<h2>🔥 🔥  Breaking Change 🔥 🔥</h2>
<ul>
<li>Directory patterns now require explicit specification of the globstar pattern to match all sub paths.</li>
</ul>
<h3></h3>
<pre lang="diff"><code>...
      - name: Get specific changed files
        id: changed-files-specific
        uses: tj-actions/changed-files@v40
        with:
          files: |
-            dir
+            dir/**
</code></pre>
<h2>What's Changed</h2>
<!-- raw HTML omitted -->
</blockquote>
<p>... (truncated)</p>
</details>
<details>
<summary>Changelog</summary>
<p><em>Sourced from <a href="https://github.com/tj-actions/changed-files/blob/main/HISTORY.md">tj-actions/changed-files's changelog</a>.</em></p>
<blockquote>
<h1>Changelog</h1>
<h1><a href="https://github.com/tj-actions/changed-files/compare/v39.2.4...v40.0.0">40.0.0</a> - (2023-10-26)</h1>
<h2><!-- raw HTML omitted -->📦 Bumps</h2>
<ul>
<li>Bump <code>@​types/node</code> from 20.8.7 to 20.8.8 (<a href="https://redirect.github.com/tj-actions/changed-files/issues/1666">#1666</a>)</li>
</ul>
<p>Signed-off-by: dependabot[bot] <a href="mailto:support@github.com">support@github.com</a>
Co-authored-by: dependabot[bot] <!-- raw HTML omitted --> (<a href="https://github.com/tj-actions/changed-files/commit/955cdc8d81246052a366a9a1c4e570774fe2b1c6">955cdc8</a>)  - (dependabot[bot])</p>
<h2><!-- raw HTML omitted -->➕ Add</h2>
<ul>
<li>Added missing changes and modified dist assets.
(<a href="https://github.com/tj-actions/changed-files/commit/af292f1e845a0377b596972698a8598734eb2796">af292f1</a>)  - (GitHub Action)</li>
<li>Added missing changes and modified dist assets.
(<a href="https://github.com/tj-actions/changed-files/commit/e48cacbca52c259aaa62f428dd90bc4e374a8cda">e48cacb</a>)  - (GitHub Action)</li>
</ul>
<h2><!-- raw HTML omitted -->➖ Remove</h2>
<ul>
<li>Appending globstar pattern for directories to prevent bugs with path matching (<a href="https://redirect.github.com/tj-actions/changed-files/issues/1670">#1670</a>) (<a href="https://github.com/tj-actions/changed-files/commit/3ce5a2970f30e3278949b12f08ed68f90cc8e482">3ce5a29</a>)  - (Tonye Jack)</li>
</ul>
<h2><!-- raw HTML omitted -->⚙️ Miscellaneous Tasks</h2>
<ul>
<li><strong>deps:</strong> Lock file maintenance (<a href="https://github.com/tj-actions/changed-files/commit/2ddcb04986856a468e97e777e87741f528cbad2b">2ddcb04</a>)  - (renovate[bot])</li>
<li>Update test.yml (<a href="https://github.com/tj-actions/changed-files/commit/d898dd09e4531bdc144c5149b2d4de056819d8a4">d898dd0</a>)  - (Tonye Jack)</li>
<li><strong>deps:</strong> Update dependency <code>@​types/node</code> to v20.8.9 (<a href="https://github.com/tj-actions/changed-files/commit/0e08afd95d354d7dfa67484142cc153c96b816df">0e08afd</a>)  - (renovate[bot])</li>
<li><strong>deps:</strong> Lock file maintenance (<a href="https://github.com/tj-actions/changed-files/commit/5aee57257118b7f1a34194ed31f5faf4d56a1b04">5aee572</a>)  - (renovate[bot])</li>
</ul>
<h2><!-- raw HTML omitted -->⬆️ Upgrades</h2>
<ul>
<li>Upgraded to v39.2.4 (<a href="https://redirect.github.com/tj-actions/changed-files/issues/1664">#1664</a>)</li>
</ul>
<p>Co-authored-by: jackton1 <a href="mailto:jackton1@users.noreply.github.com">jackton1@users.noreply.github.com</a> (<a href="https://github.com/tj-actions/changed-files/commit/c83cb31f5b2aea1959056c29027ee2b838d88747">c83cb31</a>)  - (tj-actions[bot])</p>
<h1><a href="https://github.com/tj-actions/changed-files/compare/v39.2.3...v39.2.4">39.2.4</a> - (2023-10-23)</h1>
<h2><!-- raw HTML omitted -->➕ Add</h2>
<ul>
<li>Added missing changes and modified dist assets.
(<a href="https://github.com/tj-actions/changed-files/commit/28cf22057fdc9b7c9328d0b5884e8c45b9316b22">28cf220</a>)  - (GitHub Action)</li>
<li>Added missing changes and modified dist assets.
(<a href="https://github.com/tj-actions/changed-files/commit/40e81cc72b38d108b2ba0fb7c01296a426dc775a">40e81cc</a>)  - (GitHub Action)</li>
</ul>
<h2><!-- raw HTML omitted -->⚙️ Miscellaneous Tasks</h2>
<ul>
<li><strong>deps:</strong> Update typescript-eslint monorepo to v6.9.0 (<a href="https://github.com/tj-actions/changed-files/commit/fea790cb660e33aef4bdf07304e28fedd77dfa13">fea790c</a>)  - (renovate[bot])</li>
<li><strong>deps:</strong> Update actions/setup-node action to v4 (<a href="https://redirect.github.com/tj-actions/changed-files/issues/1662">#1662</a>) (<a href="https://github.com/tj-actions/changed-files/commit/794c26fb9f1f00d846ee83388f7b31ce2f6512da">794c26f</a>)  - (renovate[bot])</li>
<li><strong>deps:</strong> Update actions/setup-node action to v3.8.2 (<a href="https://github.com/tj-actions/changed-files/commit/0f6525cd7da1375f0db035f17af23c36e1fb7782">0f6525c</a>)  - (renovate[bot])</li>
<li><strong>deps:</strong> Lock file maintenance (<a href="https://github.com/tj-actions/changed-files/commit/faedee1163969ecb7501e1ecc85c15a3bc64108a">faedee1</a>)  - (renovate[bot])</li>
</ul>
<!-- raw HTML omitted -->
</blockquote>
<p>... (truncated)</p>
</details>
<details>
<summary>Commits</summary>
<ul>
<li><a href="https://github.com/tj-actions/changed-files/commit/af292f1e845a0377b596972698a8598734eb2796"><code>af292f1</code></a> Added missing changes and modified dist assets.</li>
<li><a href="https://github.com/tj-actions/changed-files/commit/2ddcb04986856a468e97e777e87741f528cbad2b"><code>2ddcb04</code></a> chore(deps): lock file maintenance</li>
<li><a href="https://github.com/tj-actions/changed-files/commit/3ce5a2970f30e3278949b12f08ed68f90cc8e482"><code>3ce5a29</code></a> remove: appending globstar pattern for directories to prevent bugs with path ...</li>
<li><a href="https://github.com/tj-actions/changed-files/commit/d898dd09e4531bdc144c5149b2d4de056819d8a4"><code>d898dd0</code></a> chore: Update test.yml</li>
<li><a href="https://github.com/tj-actions/changed-files/commit/0e08afd95d354d7dfa67484142cc153c96b816df"><code>0e08afd</code></a> chore(deps): update dependency <code>@​types/node</code> to v20.8.9</li>
<li><a href="https://github.com/tj-actions/changed-files/commit/955cdc8d81246052a366a9a1c4e570774fe2b1c6"><code>955cdc8</code></a> Bump <code>@​types/node</code> from 20.8.7 to 20.8.8 (<a href="https://redirect.github.com/tj-actions/changed-files/issues/1666">#1666</a>)</li>
<li><a href="https://github.com/tj-actions/changed-files/commit/e48cacbca52c259aaa62f428dd90bc4e374a8cda"><code>e48cacb</code></a> Added missing changes and modified dist assets.</li>
<li><a href="https://github.com/tj-actions/changed-files/commit/5aee57257118b7f1a34194ed31f5faf4d56a1b04"><code>5aee572</code></a> chore(deps): lock file maintenance</li>
<li><a href="https://github.com/tj-actions/changed-files/commit/c83cb31f5b2aea1959056c29027ee2b838d88747"><code>c83cb31</code></a> Upgraded to v39.2.4 (<a href="https://redirect.github.com/tj-actions/changed-files/issues/1664">#1664</a>)</li>
<li>See full diff in <a href="https://github.com/tj-actions/changed-files/compare/v39...v40">compare view</a></li>
</ul>
</details>
<br />


[![Dependabot compatibility score](https://dependabot-badges.githubapp.com/badges/compatibility_score?dependency-name=tj-actions/changed-files&package-manager=github_actions&previous-version=39&new-version=40)](https://docs.github.com/en/github/managing-security-vulnerabilities/about-dependabot-security-updates#about-compatibility-scores)

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

_Label `internal` added by @dependabot[bot] on 2023-10-30 08:48_

---

_Merged by @MichaReiser on 2023-10-30 09:05_

---

_Closed by @MichaReiser on 2023-10-30 09:05_

---

_Branch deleted on 2023-10-30 09:05_

---

_Comment by @github-actions[bot] on 2023-10-30 09:07_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no linter changes.

✅ ecosystem check detected no format changes.



---
