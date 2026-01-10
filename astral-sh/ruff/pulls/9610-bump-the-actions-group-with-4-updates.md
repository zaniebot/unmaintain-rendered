```yaml
number: 9610
title: Bump the actions group with 4 updates
type: pull_request
state: closed
author: dependabot
labels:
  - internal
assignees: []
base: main
head: dependabot/github_actions/actions-70d00c9cf9
created_at: 2024-01-22T08:27:19Z
updated_at: 2024-01-22T08:45:01Z
url: https://github.com/astral-sh/ruff/pull/9610
synced_at: 2026-01-10T22:57:09Z
```

# Bump the actions group with 4 updates

---

_Pull request opened by @dependabot on 2024-01-22 08:27_

Bumps the actions group with 4 updates: [tj-actions/changed-files](https://github.com/tj-actions/changed-files), [actions/upload-artifact](https://github.com/actions/upload-artifact), [actions/download-artifact](https://github.com/actions/download-artifact) and [actions/cache](https://github.com/actions/cache).

Updates `tj-actions/changed-files` from 41 to 42
<details>
<summary>Release notes</summary>
<p><em>Sourced from <a href="https://github.com/tj-actions/changed-files/releases">tj-actions/changed-files's releases</a>.</em></p>
<blockquote>
<h2>v42</h2>
<h1>Changes in v42.0.0</h1>
<h2>🔥🔥 BREAKING CHANGE 🔥🔥</h2>
<ul>
<li>Input file patterns that end with a <code>/</code> would now match all sub-files within the directory without requiring you to specify the globstar pattern.</li>
</ul>
<h3></h3>
<pre lang="yaml"><code>...
      - name: Get changed files
        id: changed-files
        uses: tj-actions/changed-files@v42
        with:
          files: 'dir/'  # Would also be the same as dir/** 
</code></pre>
<h2>What's Changed</h2>
<ul>
<li>Upgraded to v41.1.2 by <a href="https://github.com/tj-actions-bot"><code>@​tj-actions-bot</code></a> in <a href="https://redirect.github.com/tj-actions/changed-files/pull/1869">tj-actions/changed-files#1869</a></li>
<li>chore(deps): update dependency prettier to v3.2.4 by <a href="https://github.com/renovate"><code>@​renovate</code></a> in <a href="https://redirect.github.com/tj-actions/changed-files/pull/1871">tj-actions/changed-files#1871</a></li>
<li>fix: update input warning by <a href="https://github.com/jackton1"><code>@​jackton1</code></a> in <a href="https://redirect.github.com/tj-actions/changed-files/pull/1870">tj-actions/changed-files#1870</a></li>
<li>rename: unsupported REST API inputs constant name by <a href="https://github.com/jackton1"><code>@​jackton1</code></a> in <a href="https://redirect.github.com/tj-actions/changed-files/pull/1872">tj-actions/changed-files#1872</a></li>
<li>feat: add support for include/exclude all nested files when a directory is specified and ends with a slash by <a href="https://github.com/jackton1"><code>@​jackton1</code></a> in <a href="https://redirect.github.com/tj-actions/changed-files/pull/1873">tj-actions/changed-files#1873</a></li>
</ul>
<p><strong>Full Changelog</strong>: <a href="https://github.com/tj-actions/changed-files/compare/v41...v42.0.0">https://github.com/tj-actions/changed-files/compare/v41...v42.0.0</a></p>
<hr />
<h2>v42.0.0</h2>
<h2>🔥🔥 BREAKING CHANGE 🔥🔥</h2>
<ul>
<li>Input file patterns that end with a <code>/</code> would now match all sub-files within the directory without requiring you to specify the globstar pattern.</li>
</ul>
<h3></h3>
<pre lang="yaml"><code>...
      - name: Get changed files
        id: changed-files
        uses: tj-actions/changed-files@v42
        with:
          files: 'dir/'  # Would also be the same as dir/** 
</code></pre>
<h2>What's Changed</h2>
<ul>
<li>Upgraded to v41.1.2 by <a href="https://github.com/tj-actions-bot"><code>@​tj-actions-bot</code></a> in <a href="https://redirect.github.com/tj-actions/changed-files/pull/1869">tj-actions/changed-files#1869</a></li>
<li>chore(deps): update dependency prettier to v3.2.4 by <a href="https://github.com/renovate"><code>@​renovate</code></a> in <a href="https://redirect.github.com/tj-actions/changed-files/pull/1871">tj-actions/changed-files#1871</a></li>
<li>fix: update input warning by <a href="https://github.com/jackton1"><code>@​jackton1</code></a> in <a href="https://redirect.github.com/tj-actions/changed-files/pull/1870">tj-actions/changed-files#1870</a></li>
<li>rename: unsupported REST API inputs constant name by <a href="https://github.com/jackton1"><code>@​jackton1</code></a> in <a href="https://redirect.github.com/tj-actions/changed-files/pull/1872">tj-actions/changed-files#1872</a></li>
<li>feat: add support for include/exclude all nested files when a directory is specified and ends with a slash by <a href="https://github.com/jackton1"><code>@​jackton1</code></a> in <a href="https://redirect.github.com/tj-actions/changed-files/pull/1873">tj-actions/changed-files#1873</a></li>
</ul>
<!-- raw HTML omitted -->
</blockquote>
<p>... (truncated)</p>
</details>
<details>
<summary>Changelog</summary>
<p><em>Sourced from <a href="https://github.com/tj-actions/changed-files/blob/main/HISTORY.md">tj-actions/changed-files's changelog</a>.</em></p>
<blockquote>
<h1>Changelog</h1>
<h1><a href="https://github.com/tj-actions/changed-files/compare/v41.1.2...v42.0.0">42.0.0</a> - (2024-01-18)</h1>
<h2><!-- raw HTML omitted -->🚀 Features</h2>
<ul>
<li>Add support for include/exclude all nested files when a directory is specified and ends with a slash (<a href="https://redirect.github.com/tj-actions/changed-files/issues/1873">#1873</a>) (<a href="https://github.com/tj-actions/changed-files/commit/ae82ed4ae04587b665efad2f206578aa6f0e8539">ae82ed4</a>)  - (Tonye Jack)</li>
</ul>
<h2><!-- raw HTML omitted -->🐛 Bug Fixes</h2>
<ul>
<li>Update input warning (<a href="https://redirect.github.com/tj-actions/changed-files/issues/1870">#1870</a>) (<a href="https://github.com/tj-actions/changed-files/commit/6c9dcea4432fd0eb2f3e07c9149eab2807ce44b2">6c9dcea</a>)  - (Tonye Jack)</li>
</ul>
<h2><!-- raw HTML omitted -->📝 Rename</h2>
<ul>
<li>Unsupported REST API inputs constant name (<a href="https://redirect.github.com/tj-actions/changed-files/issues/1872">#1872</a>) (<a href="https://github.com/tj-actions/changed-files/commit/cbd59070e8276b539ecdfa0f2316db7b1599ea0f">cbd5907</a>)  - (Tonye Jack)</li>
</ul>
<h2><!-- raw HTML omitted -->⚙️ Miscellaneous Tasks</h2>
<ul>
<li><strong>deps:</strong> Update dependency prettier to v3.2.4 (<a href="https://github.com/tj-actions/changed-files/commit/79b060d4450764e6b54a73696c2d99134757db95">79b060d</a>)  - (renovate[bot])</li>
</ul>
<h2><!-- raw HTML omitted -->⬆️ Upgrades</h2>
<ul>
<li>Upgraded to v41.1.2 (<a href="https://redirect.github.com/tj-actions/changed-files/issues/1869">#1869</a>)</li>
</ul>
<p>Co-authored-by: jackton1 <a href="mailto:jackton1@users.noreply.github.com">jackton1@users.noreply.github.com</a> (<a href="https://github.com/tj-actions/changed-files/commit/434b67ebc3051662cf28de12b8a7adb77aea522a">434b67e</a>)  - (tj-actions[bot])</p>
<h1><a href="https://github.com/tj-actions/changed-files/compare/v41.1.1...v41.1.2">41.1.2</a> - (2024-01-17)</h1>
<h2><!-- raw HTML omitted -->🚀 Features</h2>
<ul>
<li>Enhance error handling and working directory resolution (<a href="https://redirect.github.com/tj-actions/changed-files/issues/1859">#1859</a>) (<a href="https://github.com/tj-actions/changed-files/commit/a60bf3759e069549b60c8da1284ec83e0398a1a4">a60bf37</a>)  - (Tonye Jack)</li>
</ul>
<h2><!-- raw HTML omitted -->🐛 Bug Fixes</h2>
<ul>
<li>Bug with incorrect action path (<a href="https://redirect.github.com/tj-actions/changed-files/issues/1866">#1866</a>) (<a href="https://github.com/tj-actions/changed-files/commit/3f8189989ba6fd9b3b9593ffe650236b3f1fcb55">3f81899</a>)  - (Tonye Jack)</li>
</ul>
<h2><!-- raw HTML omitted -->⚙️ Miscellaneous Tasks</h2>
<ul>
<li><strong>deps:</strong> Update dependency <code>@​types/node</code> to v20.11.5 (<a href="https://github.com/tj-actions/changed-files/commit/cbda684547adc8c052d50711417fa61b428a9f88">cbda684</a>)  - (renovate[bot])</li>
<li><strong>deps:</strong> Update dependency prettier to v3.2.3 (<a href="https://github.com/tj-actions/changed-files/commit/67a1f54f6f5ec7ee87c57eb7876a7d6dfdcc59a1">67a1f54</a>)  - (renovate[bot])</li>
<li><strong>deps:</strong> Update tj-actions/eslint-changed-files action to v22 (<a href="https://redirect.github.com/tj-actions/changed-files/issues/1864">#1864</a>) (<a href="https://github.com/tj-actions/changed-files/commit/99248a443855d73284abf52520f897dba851b914">99248a4</a>)  - (renovate[bot])</li>
<li><strong>deps:</strong> Update dependency <code>@​types/node</code> to v20.11.4 (<a href="https://github.com/tj-actions/changed-files/commit/878743189ba0ba42d467a736923b07102f0e348c">8787431</a>)  - (renovate[bot])</li>
<li><strong>deps:</strong> Update dependency <code>@​types/node</code> to v20.11.3 (<a href="https://github.com/tj-actions/changed-files/commit/98d1d84e2f7a404c425df4e44dceb74a03920ac8">98d1d84</a>)  - (renovate[bot])</li>
<li><strong>deps:</strong> Update typescript-eslint monorepo to v6.19.0 (<a href="https://github.com/tj-actions/changed-files/commit/bc46e4c4222c3926a70378d183f0b387d3a9e9a8">bc46e4c</a>)  - (renovate[bot])</li>
<li><strong>deps:</strong> Update dependency <code>@​types/node</code> to v20.11.2 (<a href="https://github.com/tj-actions/changed-files/commit/fba40673489d49e860c15a444c134d887ead1f3a">fba4067</a>)  - (renovate[bot])</li>
<li><strong>deps:</strong> Update dependency <code>@​types/node</code> to v20.11.1 (<a href="https://github.com/tj-actions/changed-files/commit/e4b86747326bc58eb230d62188ebdd66b73721a9">e4b8674</a>)  - (renovate[bot])</li>
<li><strong>deps:</strong> Lock file maintenance (<a href="https://github.com/tj-actions/changed-files/commit/bc2b5aef20add66cbe21d1093f0f1d37a353d376">bc2b5ae</a>)  - (renovate[bot])</li>
<li><strong>deps:</strong> Update dependency prettier to v3.2.2 (<a href="https://github.com/tj-actions/changed-files/commit/63c36a563a30544b4c03a8426277dca4b00e4fd1">63c36a5</a>)  - (renovate[bot])</li>
</ul>
<h2><!-- raw HTML omitted -->⬆️ Upgrades</h2>
<!-- raw HTML omitted -->
</blockquote>
<p>... (truncated)</p>
</details>
<details>
<summary>Commits</summary>
<ul>
<li><a href="https://github.com/tj-actions/changed-files/commit/ae82ed4ae04587b665efad2f206578aa6f0e8539"><code>ae82ed4</code></a> feat: add support for include/exclude all nested files when a directory is sp...</li>
<li><a href="https://github.com/tj-actions/changed-files/commit/cbd59070e8276b539ecdfa0f2316db7b1599ea0f"><code>cbd5907</code></a> rename: unsupported REST API inputs constant name (<a href="https://redirect.github.com/tj-actions/changed-files/issues/1872">#1872</a>)</li>
<li><a href="https://github.com/tj-actions/changed-files/commit/6c9dcea4432fd0eb2f3e07c9149eab2807ce44b2"><code>6c9dcea</code></a> fix: update input warning (<a href="https://redirect.github.com/tj-actions/changed-files/issues/1870">#1870</a>)</li>
<li><a href="https://github.com/tj-actions/changed-files/commit/79b060d4450764e6b54a73696c2d99134757db95"><code>79b060d</code></a> chore(deps): update dependency prettier to v3.2.4</li>
<li><a href="https://github.com/tj-actions/changed-files/commit/434b67ebc3051662cf28de12b8a7adb77aea522a"><code>434b67e</code></a> Upgraded to v41.1.2 (<a href="https://redirect.github.com/tj-actions/changed-files/issues/1869">#1869</a>)</li>
<li>See full diff in <a href="https://github.com/tj-actions/changed-files/compare/v41...v42">compare view</a></li>
</ul>
</details>
<br />

Updates `actions/upload-artifact` from 3 to 4
<details>
<summary>Release notes</summary>
<p><em>Sourced from <a href="https://github.com/actions/upload-artifact/releases">actions/upload-artifact's releases</a>.</em></p>
<blockquote>
<h2>v4.0.0</h2>
<h2>What's Changed</h2>
<p>The release of upload-artifact@v4 and download-artifact@v4 are major changes to the backend architecture of Artifacts. They have numerous performance and behavioral improvements.</p>
<p>ℹ️ However, this is a major update that includes breaking changes. Artifacts created with versions v3 and below are not compatible with the v4 actions. Uploads and downloads <em>must</em> use the same major actions versions. There are also key differences from previous versions that may require updates to your workflows.</p>
<p>For more information, please see:</p>
<ol>
<li>The <a href="https://github.blog/changelog/2023-12-14-github-actions-artifacts-v4-is-now-generally-available/">changelog</a> post.</li>
<li>The <a href="https://github.com/actions/upload-artifact/blob/main/README.md">README</a>.</li>
<li>The <a href="https://github.com/actions/upload-artifact/blob/main/docs/MIGRATION.md">migration documentation</a>.</li>
<li>As well as the underlying npm package, <a href="https://github.com/actions/toolkit/tree/main/packages/artifact"><code>@​actions/artifact</code></a> documentation.</li>
</ol>
<h2>New Contributors</h2>
<ul>
<li><a href="https://github.com/vmjoseph"><code>@​vmjoseph</code></a> made their first contribution in <a href="https://redirect.github.com/actions/upload-artifact/pull/464">actions/upload-artifact#464</a></li>
</ul>
<p><strong>Full Changelog</strong>: <a href="https://github.com/actions/upload-artifact/compare/v3...v4.0.0">https://github.com/actions/upload-artifact/compare/v3...v4.0.0</a></p>
<h2>v3.1.3</h2>
<h2>What's Changed</h2>
<ul>
<li>chore(github): remove trailing whitespaces by <a href="https://github.com/ljmf00"><code>@​ljmf00</code></a> in <a href="https://redirect.github.com/actions/upload-artifact/pull/313">actions/upload-artifact#313</a></li>
<li>Bump <code>@​actions/artifact</code> version to v1.1.2 by <a href="https://github.com/bethanyj28"><code>@​bethanyj28</code></a> in <a href="https://redirect.github.com/actions/upload-artifact/pull/436">actions/upload-artifact#436</a></li>
</ul>
<p><strong>Full Changelog</strong>: <a href="https://github.com/actions/upload-artifact/compare/v3...v3.1.3">https://github.com/actions/upload-artifact/compare/v3...v3.1.3</a></p>
<h2>v3.1.2</h2>
<ul>
<li>Update all <code>@actions/*</code> NPM packages to their latest versions- <a href="https://redirect.github.com/actions/upload-artifact/issues/374">#374</a></li>
<li>Update all dev dependencies to their most recent versions - <a href="https://redirect.github.com/actions/upload-artifact/issues/375">#375</a></li>
</ul>
<h2>v3.1.1</h2>
<ul>
<li>Update actions/core package to latest version to remove <code>set-output</code> deprecation warning <a href="https://redirect.github.com/actions/upload-artifact/issues/351">#351</a></li>
</ul>
<h2>v3.1.0</h2>
<h2>What's Changed</h2>
<ul>
<li>Bump <code>@​actions/artifact</code> to v1.1.0 (<a href="https://redirect.github.com/actions/upload-artifact/pull/327">actions/upload-artifact#327</a>)
<ul>
<li>Adds checksum headers on artifact upload (<a href="https://redirect.github.com/actions/toolkit/pull/1095">actions/toolkit#1095</a>) (<a href="https://redirect.github.com/actions/toolkit/pull/1063">actions/toolkit#1063</a>)</li>
</ul>
</li>
</ul>
</blockquote>
</details>
<details>
<summary>Commits</summary>
<ul>
<li><a href="https://github.com/actions/upload-artifact/commit/694cdabd8bdb0f10b2cea11669e1bf5453eed0a6"><code>694cdab</code></a> Merge pull request <a href="https://redirect.github.com/actions/upload-artifact/issues/501">#501</a> from actions/robherley/overwrite-artifact</li>
<li><a href="https://github.com/actions/upload-artifact/commit/05d4fe67025ed6d5b746e3502573715a2c37e318"><code>05d4fe6</code></a> run licensed against version that matches ci</li>
<li><a href="https://github.com/actions/upload-artifact/commit/40b305282182ab3d3265c0c45fd82dee1e605ace"><code>40b3052</code></a> update readme</li>
<li><a href="https://github.com/actions/upload-artifact/commit/49552fcb82fc92c668b492a7d36fc7860a20c81d"><code>49552fc</code></a> add overwrite tests to workflow</li>
<li><a href="https://github.com/actions/upload-artifact/commit/79615904cc1dfafb77b85c0d3a21f89dba2a38c2"><code>7961590</code></a> licensed cache</li>
<li><a href="https://github.com/actions/upload-artifact/commit/11ff42c7b1b52130283d8a02bc960d1e1de95000"><code>11ff42c</code></a> add new overwrite input &amp; docs</li>
<li><a href="https://github.com/actions/upload-artifact/commit/1eb3cb2b3e0f29609092a73eb033bb759a334595"><code>1eb3cb2</code></a> Merge pull request <a href="https://redirect.github.com/actions/upload-artifact/issues/497">#497</a> from actions/robherley/update-readme-limit</li>
<li><a href="https://github.com/actions/upload-artifact/commit/8688a86492f53c8d67423223a877bc9e3768fe95"><code>8688a86</code></a> Update readme to reflect new artifact/job limit</li>
<li><a href="https://github.com/actions/upload-artifact/commit/73d8b66ede50d06e26f1d69f28e1652c702c56d8"><code>73d8b66</code></a> Support artifact-url output (<a href="https://redirect.github.com/actions/upload-artifact/issues/496">#496</a>)</li>
<li><a href="https://github.com/actions/upload-artifact/commit/c320f57948d137eb8c7f8e781ddcc0f61b04e834"><code>c320f57</code></a> Update README.md (<a href="https://redirect.github.com/actions/upload-artifact/issues/492">#492</a>)</li>
<li>Additional commits viewable in <a href="https://github.com/actions/upload-artifact/compare/v3...v4">compare view</a></li>
</ul>
</details>
<br />

Updates `actions/download-artifact` from 3 to 4
<details>
<summary>Release notes</summary>
<p><em>Sourced from <a href="https://github.com/actions/download-artifact/releases">actions/download-artifact's releases</a>.</em></p>
<blockquote>
<h2>v4.0.0</h2>
<h2>What's Changed</h2>
<p>The release of upload-artifact@v4 and download-artifact@v4 are major changes to the backend architecture of Artifacts. They have numerous performance and behavioral improvements.</p>
<p>ℹ️ However, this is a major update that includes breaking changes. Artifacts created with versions v3 and below are not compatible with the v4 actions. Uploads and downloads <em>must</em> use the same major actions versions. There are also key differences from previous versions that may require updates to your workflows.</p>
<p>For more information, please see:</p>
<ol>
<li>The <a href="https://github.blog/changelog/2023-12-14-github-actions-artifacts-v4-is-now-generally-available/">changelog</a> post.</li>
<li>The <a href="https://github.com/actions/download-artifact/blob/main/README.md">README</a>.</li>
<li>The <a href="https://github.com/actions/upload-artifact/blob/main/docs/MIGRATION.md">migration documentation</a>.</li>
<li>As well as the underlying npm package, <a href="https://github.com/actions/toolkit/tree/main/packages/artifact"><code>@​actions/artifact</code></a> documentation.</li>
</ol>
<h2>New Contributors</h2>
<ul>
<li><a href="https://github.com/bflad"><code>@​bflad</code></a> made their first contribution in <a href="https://redirect.github.com/actions/download-artifact/pull/194">actions/download-artifact#194</a></li>
</ul>
<p><strong>Full Changelog</strong>: <a href="https://github.com/actions/download-artifact/compare/v3...v4.0.0">https://github.com/actions/download-artifact/compare/v3...v4.0.0</a></p>
<h2>v3.0.2</h2>
<ul>
<li>Bump <code>@actions/artifact</code> to v1.1.1 - <a href="https://redirect.github.com/actions/download-artifact/pull/195">actions/download-artifact#195</a></li>
<li>Fixed a bug in Node16 where if an HTTP download finished too quickly (&lt;1ms, e.g. when it's mocked) we attempt to delete a temp file that has not been created yet <a href="hhttps://redirect.github.com/actions/toolkit/pull/1278">actions/toolkit#1278</a></li>
</ul>
<h2>v3.0.1</h2>
<ul>
<li><a href="https://redirect.github.com/actions/download-artifact/pull/178">Bump <code>@​actions/core</code> to 1.10.0</a></li>
</ul>
</blockquote>
</details>
<details>
<summary>Commits</summary>
<ul>
<li><a href="https://github.com/actions/download-artifact/commit/6b208ae046db98c579e8a3aa621ab581ff575935"><code>6b208ae</code></a> Merge pull request <a href="https://redirect.github.com/actions/download-artifact/issues/274">#274</a> from actions/vmjoseph/timeout-patch</li>
<li><a href="https://github.com/actions/download-artifact/commit/6c5b5806e1d833ffbeb6c412b38ba07d67086dc6"><code>6c5b580</code></a> only adding updated license</li>
<li><a href="https://github.com/actions/download-artifact/commit/5f5015dc38f8eb6cafa9c4d68689495abffbea38"><code>5f5015d</code></a> readding index</li>
<li><a href="https://github.com/actions/download-artifact/commit/1fddaaf0f1912c34f984d46c39e99a66afa07172"><code>1fddaaf</code></a> Revert &quot;updating licenses&quot;</li>
<li><a href="https://github.com/actions/download-artifact/commit/8aa9e2115bb091db1962857dc393a054568a7851"><code>8aa9e21</code></a> Revert &quot;updating dist&quot;</li>
<li><a href="https://github.com/actions/download-artifact/commit/657edd9b813fc9cdb0cddf8601de283f4ecea661"><code>657edd9</code></a> updating licenses</li>
<li><a href="https://github.com/actions/download-artifact/commit/555a2fc1299fb63c5a4e673a65c8b9e6e3474b22"><code>555a2fc</code></a> updating dist</li>
<li><a href="https://github.com/actions/download-artifact/commit/4fc4d70d4c55025435f9bfd763515cecf048fc86"><code>4fc4d70</code></a> updating lock</li>
<li><a href="https://github.com/actions/download-artifact/commit/072ac9dcebef67fdbbc9612b2879103e0e877f16"><code>072ac9d</code></a> updating version no</li>
<li><a href="https://github.com/actions/download-artifact/commit/038dc0329f0e804ae981db8af9f43b00a55b10de"><code>038dc03</code></a> updating version no</li>
<li>Additional commits viewable in <a href="https://github.com/actions/download-artifact/compare/v3...v4">compare view</a></li>
</ul>
</details>
<br />

Updates `actions/cache` from 3 to 4
<details>
<summary>Release notes</summary>
<p><em>Sourced from <a href="https://github.com/actions/cache/releases">actions/cache's releases</a>.</em></p>
<blockquote>
<h2>v4.0.0</h2>
<h2>What's Changed</h2>
<ul>
<li>Update action to node20 by <a href="https://github.com/takost"><code>@​takost</code></a> in <a href="https://redirect.github.com/actions/cache/pull/1284">actions/cache#1284</a></li>
<li>feat: save-always flag by <a href="https://github.com/to-s"><code>@​to-s</code></a> in <a href="https://redirect.github.com/actions/cache/pull/1242">actions/cache#1242</a></li>
</ul>
<h2>New Contributors</h2>
<ul>
<li><a href="https://github.com/takost"><code>@​takost</code></a> made their first contribution in <a href="https://redirect.github.com/actions/cache/pull/1284">actions/cache#1284</a></li>
<li><a href="https://github.com/to-s"><code>@​to-s</code></a> made their first contribution in <a href="https://redirect.github.com/actions/cache/pull/1242">actions/cache#1242</a></li>
</ul>
<p><strong>Full Changelog</strong>: <a href="https://github.com/actions/cache/compare/v3...v4.0.0">https://github.com/actions/cache/compare/v3...v4.0.0</a></p>
<h2>v3.3.3</h2>
<h2>What's Changed</h2>
<ul>
<li>Cache v3.3.3 by <a href="https://github.com/robherley"><code>@​robherley</code></a> in <a href="https://redirect.github.com/actions/cache/pull/1302">actions/cache#1302</a></li>
</ul>
<h2>New Contributors</h2>
<ul>
<li><a href="https://github.com/robherley"><code>@​robherley</code></a> made their first contribution in <a href="https://redirect.github.com/actions/cache/pull/1302">actions/cache#1302</a></li>
</ul>
<p><strong>Full Changelog</strong>: <a href="https://github.com/actions/cache/compare/v3...v3.3.3">https://github.com/actions/cache/compare/v3...v3.3.3</a></p>
<h2>v3.3.2</h2>
<h2>What's Changed</h2>
<ul>
<li>Fixed readme with new segment timeout values by <a href="https://github.com/kotewar"><code>@​kotewar</code></a> in <a href="https://redirect.github.com/actions/cache/pull/1133">actions/cache#1133</a></li>
<li>Readme fixes by <a href="https://github.com/kotewar"><code>@​kotewar</code></a> in <a href="https://redirect.github.com/actions/cache/pull/1134">actions/cache#1134</a></li>
<li>Updated description of the lookup-only input for main action by <a href="https://github.com/kotewar"><code>@​kotewar</code></a> in <a href="https://redirect.github.com/actions/cache/pull/1130">actions/cache#1130</a></li>
<li>Change two new actions mention as quoted text by <a href="https://github.com/bishal-pdMSFT"><code>@​bishal-pdMSFT</code></a> in <a href="https://redirect.github.com/actions/cache/pull/1131">actions/cache#1131</a></li>
<li>Update Cross-OS Caching tips by <a href="https://github.com/pdotl"><code>@​pdotl</code></a> in <a href="https://redirect.github.com/actions/cache/pull/1122">actions/cache#1122</a></li>
<li>Bazel example (Take <a href="https://redirect.github.com/actions/cache/issues/2">#2</a>️⃣) by <a href="https://github.com/vorburger"><code>@​vorburger</code></a> in <a href="https://redirect.github.com/actions/cache/pull/1132">actions/cache#1132</a></li>
<li>Remove actions to add new PRs and issues to a project board by <a href="https://github.com/jorendorff"><code>@​jorendorff</code></a> in <a href="https://redirect.github.com/actions/cache/pull/1187">actions/cache#1187</a></li>
<li>Consume latest toolkit and fix dangling promise bug by <a href="https://github.com/chkimes"><code>@​chkimes</code></a> in <a href="https://redirect.github.com/actions/cache/pull/1217">actions/cache#1217</a></li>
<li>Bump action version to 3.3.2 by <a href="https://github.com/bethanyj28"><code>@​bethanyj28</code></a> in <a href="https://redirect.github.com/actions/cache/pull/1236">actions/cache#1236</a></li>
</ul>
<h2>New Contributors</h2>
<ul>
<li><a href="https://github.com/vorburger"><code>@​vorburger</code></a> made their first contribution in <a href="https://redirect.github.com/actions/cache/pull/1132">actions/cache#1132</a></li>
<li><a href="https://github.com/jorendorff"><code>@​jorendorff</code></a> made their first contribution in <a href="https://redirect.github.com/actions/cache/pull/1187">actions/cache#1187</a></li>
<li><a href="https://github.com/chkimes"><code>@​chkimes</code></a> made their first contribution in <a href="https://redirect.github.com/actions/cache/pull/1217">actions/cache#1217</a></li>
<li><a href="https://github.com/bethanyj28"><code>@​bethanyj28</code></a> made their first contribution in <a href="https://redirect.github.com/actions/cache/pull/1236">actions/cache#1236</a></li>
</ul>
<p><strong>Full Changelog</strong>: <a href="https://github.com/actions/cache/compare/v3...v3.3.2">https://github.com/actions/cache/compare/v3...v3.3.2</a></p>
<h2>v3.3.1</h2>
<h2>What's Changed</h2>
<ul>
<li>Reduced download segment size to 128 MB and timeout to 10 minutes by <a href="https://github.com/kotewar"><code>@​kotewar</code></a> in <a href="https://redirect.github.com/actions/cache/pull/1129">actions/cache#1129</a></li>
</ul>
<p><strong>Full Changelog</strong>: <a href="https://github.com/actions/cache/compare/v3...v3.3.1">https://github.com/actions/cache/compare/v3...v3.3.1</a></p>
<h2>v3.3.0</h2>
<h2>What's Changed</h2>
<ul>
<li>Bug: Permission is missing in cache delete example by <a href="https://github.com/kotokaze"><code>@​kotokaze</code></a> in <a href="https://redirect.github.com/actions/cache/pull/1123">actions/cache#1123</a></li>
</ul>
<!-- raw HTML omitted -->
</blockquote>
<p>... (truncated)</p>
</details>
<details>
<summary>Changelog</summary>
<p><em>Sourced from <a href="https://github.com/actions/cache/blob/main/RELEASES.md">actions/cache's changelog</a>.</em></p>
<blockquote>
<h1>Releases</h1>
<h3>3.0.0</h3>
<ul>
<li>Updated minimum runner version support from node 12 -&gt; node 16</li>
</ul>
<h3>3.0.1</h3>
<ul>
<li>Added support for caching from GHES 3.5.</li>
<li>Fixed download issue for files &gt; 2GB during restore.</li>
</ul>
<h3>3.0.2</h3>
<ul>
<li>Added support for dynamic cache size cap on GHES.</li>
</ul>
<h3>3.0.3</h3>
<ul>
<li>Fixed avoiding empty cache save when no files are available for caching. (<a href="https://redirect.github.com/actions/cache/issues/624">issue</a>)</li>
</ul>
<h3>3.0.4</h3>
<ul>
<li>Fixed tar creation error while trying to create tar with path as <code>~/</code> home folder on <code>ubuntu-latest</code>. (<a href="https://redirect.github.com/actions/cache/issues/689">issue</a>)</li>
</ul>
<h3>3.0.5</h3>
<ul>
<li>Removed error handling by consuming actions/cache 3.0 toolkit, Now cache server error handling will be done by toolkit. (<a href="https://redirect.github.com/actions/cache/pull/834">PR</a>)</li>
</ul>
<h3>3.0.6</h3>
<ul>
<li>Fixed <a href="https://redirect.github.com/actions/cache/issues/809">#809</a> - zstd -d: no such file or directory error</li>
<li>Fixed <a href="https://redirect.github.com/actions/cache/issues/833">#833</a> - cache doesn't work with github workspace directory</li>
</ul>
<h3>3.0.7</h3>
<ul>
<li>Fixed <a href="https://redirect.github.com/actions/cache/issues/810">#810</a> - download stuck issue. A new timeout is introduced in the download process to abort the download if it gets stuck and doesn't finish within an hour.</li>
</ul>
<h3>3.0.8</h3>
<ul>
<li>Fix zstd not working for windows on gnu tar in issues <a href="https://redirect.github.com/actions/cache/issues/888">#888</a> and <a href="https://redirect.github.com/actions/cache/issues/891">#891</a>.</li>
<li>Allowing users to provide a custom timeout as input for aborting download of a cache segment using an environment variable <code>SEGMENT_DOWNLOAD_TIMEOUT_MINS</code>. Default is 60 minutes.</li>
</ul>
<h3>3.0.9</h3>
<ul>
<li>Enhanced the warning message for cache unavailablity in case of GHES.</li>
</ul>
<h3>3.0.10</h3>
<ul>
<li>Fix a bug with sorting inputs.</li>
<li>Update definition for restore-keys in README.md</li>
</ul>
<!-- raw HTML omitted -->
</blockquote>
<p>... (truncated)</p>
</details>
<details>
<summary>Commits</summary>
<ul>
<li><a href="https://github.com/actions/cache/commit/13aacd865c20de90d75de3b17ebe84f7a17d57d2"><code>13aacd8</code></a> Merge pull request <a href="https://redirect.github.com/actions/cache/issues/1242">#1242</a> from to-s/main</li>
<li><a href="https://github.com/actions/cache/commit/53b35c543921fe2e8b288765ff817de9de8d906f"><code>53b35c5</code></a> Merge branch 'main' into main</li>
<li><a href="https://github.com/actions/cache/commit/65b8989fab3bb394817bdb845a453dff480c2b51"><code>65b8989</code></a> Merge pull request <a href="https://redirect.github.com/actions/cache/issues/1284">#1284</a> from takost/update-to-node-20</li>
<li><a href="https://github.com/actions/cache/commit/d0be34d54485f31ca2ccbe66e6ea3d96544a807b"><code>d0be34d</code></a> Fix dist</li>
<li><a href="https://github.com/actions/cache/commit/66cf064d47313d2cccf392d01bd10925da2bd072"><code>66cf064</code></a> Merge branch 'main' into update-to-node-20</li>
<li><a href="https://github.com/actions/cache/commit/1326563738ddb735c5f2ce85cba8c79f33b728cd"><code>1326563</code></a> Merge branch 'main' into main</li>
<li><a href="https://github.com/actions/cache/commit/e71876755e268d6cc25a5d3e3c46ae447e35290a"><code>e718767</code></a> Fix format</li>
<li><a href="https://github.com/actions/cache/commit/01229828ffa049a8dee4db27bcb23ed33f2b451f"><code>0122982</code></a> Apply workaround for earlyExit</li>
<li><a href="https://github.com/actions/cache/commit/3185ecfd6135856ca6d904ae032cff4f39b8b365"><code>3185ecf</code></a> Update &quot;only-&quot; actions to node20</li>
<li><a href="https://github.com/actions/cache/commit/25618a0a675e8447e5ffc8ed9b7ddb2aaf927f65"><code>25618a0</code></a> Bump version</li>
<li>Additional commits viewable in <a href="https://github.com/actions/cache/compare/v3...v4">compare view</a></li>
</ul>
</details>
<br />


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
- `@dependabot ignore <dependency name> major version` will close this group update PR and stop Dependabot creating any more for the specific dependency's major version (unless you unignore this specific dependency's major version or upgrade to it yourself)
- `@dependabot ignore <dependency name> minor version` will close this group update PR and stop Dependabot creating any more for the specific dependency's minor version (unless you unignore this specific dependency's minor version or upgrade to it yourself)
- `@dependabot ignore <dependency name>` will close this group update PR and stop Dependabot creating any more for the specific dependency (unless you unignore this specific dependency or upgrade to it yourself)
- `@dependabot unignore <dependency name>` will remove all of the ignore conditions of the specified dependency
- `@dependabot unignore <dependency name> <ignore condition>` will remove the ignore condition of the specified dependency and ignore conditions


</details>

---

_Label `internal` added by @dependabot[bot] on 2024-01-22 08:27_

---

_Closed by @MichaReiser on 2024-01-22 08:29_

---

_Comment by @dependabot[bot] on 2024-01-22 08:29_

This pull request was built based on a group rule. Closing it will not ignore any of these versions in future pull requests.

---

_Branch deleted on 2024-01-22 08:29_

---

_Comment by @github-actions[bot] on 2024-01-22 08:44_

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
