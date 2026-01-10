```yaml
number: 13739
title: Update Rust crate libcst to v1.5.0
type: pull_request
state: merged
author: renovate
labels:
  - internal
assignees: []
merged: true
base: main
head: renovate/libcst-1.x-lockfile
created_at: 2024-10-14T01:16:27Z
updated_at: 2024-10-14T07:55:30Z
url: https://github.com/astral-sh/ruff/pull/13739
synced_at: 2026-01-10T20:59:37Z
```

# Update Rust crate libcst to v1.5.0

---

_Pull request opened by @renovate on 2024-10-14 01:16_

This PR contains the following updates:

| Package | Type | Update | Change |
|---|---|---|---|
| [libcst](https://redirect.github.com/Instagram/LibCST) | workspace.dependencies | minor | `1.4.0` -> `1.5.0` |

---

### Release Notes

<details>
<summary>Instagram/LibCST (libcst)</summary>

### [`v1.5.0`](https://redirect.github.com/Instagram/LibCST/blob/HEAD/CHANGELOG.md#150---2024-10-10)

[Compare Source](https://redirect.github.com/Instagram/LibCST/compare/v1.4.0...v1.5.0)

#### Added

-   FullyQualifiedNameProvider: Optionally consider pyproject.toml files when determining a file's module name and package by [@&#8203;camillol](https://redirect.github.com/camillol) in [https://github.com/Instagram/LibCST/pull/1148](https://redirect.github.com/Instagram/LibCST/pull/1148)
-   Add validation for If node by [@&#8203;kiri11](https://redirect.github.com/kiri11) in [https://github.com/Instagram/LibCST/pull/1177](https://redirect.github.com/Instagram/LibCST/pull/1177)
-   include python 3.13 in build by [@&#8203;khameeteman](https://redirect.github.com/khameeteman) in [https://github.com/Instagram/LibCST/pull/1203](https://redirect.github.com/Instagram/LibCST/pull/1203)

#### Fixed

-   fix various Match statement visitation errors by [@&#8203;zsol](https://redirect.github.com/zsol) in [https://github.com/Instagram/LibCST/pull/1161](https://redirect.github.com/Instagram/LibCST/pull/1161)
-   Mention codemod -x flag in docs by [@&#8203;kiri11](https://redirect.github.com/kiri11) in [https://github.com/Instagram/LibCST/pull/1169](https://redirect.github.com/Instagram/LibCST/pull/1169)
-   Clear warnings for each file in codemod cli by [@&#8203;kiri11](https://redirect.github.com/kiri11) in [https://github.com/Instagram/LibCST/pull/1184](https://redirect.github.com/Instagram/LibCST/pull/1184)
-   Typo fix in codemods_tutorial.rst (trivial) by [@&#8203;wimglenn](https://redirect.github.com/wimglenn) in [https://github.com/Instagram/LibCST/pull/1208](https://redirect.github.com/Instagram/LibCST/pull/1208)
-   fix certain matchers breaking under multiprocessing by initializing them late by [@&#8203;kiri11](https://redirect.github.com/kiri11) in [https://github.com/Instagram/LibCST/pull/1204](https://redirect.github.com/Instagram/LibCST/pull/1204)

#### Updated

-   make libcst_native::tokenizer public by [@&#8203;zsol](https://redirect.github.com/zsol) in [https://github.com/Instagram/LibCST/pull/1182](https://redirect.github.com/Instagram/LibCST/pull/1182)
-   Use `license` instead of `license-file` by [@&#8203;michel-slm](https://redirect.github.com/michel-slm) in [https://github.com/Instagram/LibCST/pull/1189](https://redirect.github.com/Instagram/LibCST/pull/1189)
-   Drop codecov from CI and readme by [@&#8203;amyreese](https://redirect.github.com/amyreese) in [https://github.com/Instagram/LibCST/pull/1192](https://redirect.github.com/Instagram/LibCST/pull/1192)

#### New Contributors

-   [@&#8203;kiri11](https://redirect.github.com/kiri11) made their first contribution in [https://github.com/Instagram/LibCST/pull/1169](https://redirect.github.com/Instagram/LibCST/pull/1169)
-   [@&#8203;grievejia](https://redirect.github.com/grievejia) made their first contribution in [https://github.com/Instagram/LibCST/pull/1174](https://redirect.github.com/Instagram/LibCST/pull/1174)
-   [@&#8203;michel-slm](https://redirect.github.com/michel-slm) made their first contribution in [https://github.com/Instagram/LibCST/pull/1189](https://redirect.github.com/Instagram/LibCST/pull/1189)
-   [@&#8203;wimglenn](https://redirect.github.com/wimglenn) made their first contribution in [https://github.com/Instagram/LibCST/pull/1208](https://redirect.github.com/Instagram/LibCST/pull/1208)
-   [@&#8203;khameeteman](https://redirect.github.com/khameeteman) made their first contribution in [https://github.com/Instagram/LibCST/pull/1203](https://redirect.github.com/Instagram/LibCST/pull/1203)

**Full Changelog**: https://github.com/Instagram/LibCST/compare/v1.4.0...v1.5.0

</details>

---

### Configuration

📅 **Schedule**: Branch creation - "before 4am on Monday" (UTC), Automerge - At any time (no schedule defined).

🚦 **Automerge**: Disabled by config. Please merge this manually once you are satisfied.

♻ **Rebasing**: Whenever PR becomes conflicted, or you tick the rebase/retry checkbox.

🔕 **Ignore**: Close this PR and you won't be reminded about this update again.

---

 - [ ] <!-- rebase-check -->If you want to rebase/retry this PR, check this box

---

This PR was generated by [Mend Renovate](https://mend.io/renovate/). View the [repository job log](https://developer.mend.io/github/astral-sh/ruff).
<!--renovate-debug:eyJjcmVhdGVkSW5WZXIiOiIzOC4xMTUuMSIsInVwZGF0ZWRJblZlciI6IjM4LjExNS4xIiwidGFyZ2V0QnJhbmNoIjoibWFpbiIsImxhYmVscyI6WyJpbnRlcm5hbCJdfQ==-->


---

_Label `internal` added by @renovate[bot] on 2024-10-14 01:16_

---

_Comment by @github-actions[bot] on 2024-10-14 01:36_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **encountered linter errors**. (no lint changes; 1 project error)

<details><summary><a href="https://github.com/pandas-dev/pandas">pandas-dev/pandas</a> (+0 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
</pre>

</p>
</details>
<details><summary><a href="https://github.com/aws/aws-sam-cli">aws/aws-sam-cli</a> (error)</summary>
<p>

```
Failed to clone aws/aws-sam-cli: error: RPC failed; curl 92 HTTP/2 stream 5 was not closed cleanly: CANCEL (err 8)
error: 4698 bytes of body are still expected
fetch-pack: unexpected disconnect while reading sideband packet
fatal: early EOF
fatal: fetch-pack: invalid index-pack output
```

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **encountered linter errors**. (no lint changes; 1 project error)

<details><summary><a href="https://github.com/aws/aws-sam-cli">aws/aws-sam-cli</a> (error)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

```
Failed to clone aws/aws-sam-cli: error: RPC failed; curl 92 HTTP/2 stream 5 was not closed cleanly: CANCEL (err 8)
error: 3838 bytes of body are still expected
fetch-pack: unexpected disconnect while reading sideband packet
fatal: early EOF
fatal: fetch-pack: invalid index-pack output
```

</p>
</details>

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Comment by @MichaReiser on 2024-10-14 07:50_

The changes are mainly bumping the dependency versions. See https://github.com/Instagram/LibCST/commits/main/native

---

_Merged by @MichaReiser on 2024-10-14 07:51_

---

_Closed by @MichaReiser on 2024-10-14 07:51_

---

_Branch deleted on 2024-10-14 07:51_

---
