```yaml
number: 19349
title: "[ty] Add ecosystem-report workflow"
type: pull_request
state: merged
author: sharkdp
labels:
  - ci
  - ty
assignees: []
merged: true
base: main
head: david/ecosystem-report
created_at: 2025-07-15T10:18:33Z
updated_at: 2025-07-15T10:29:46Z
url: https://github.com/astral-sh/ruff/pull/19349
synced_at: 2026-01-10T17:58:13Z
```

# [ty] Add ecosystem-report workflow

---

_Pull request opened by @sharkdp on 2025-07-15 10:18_

## Summary

Adds a new workflow that generates an ecosystem report like [this](https://shark.fish/ecosystem-report.html) and publishes it to Cloudflare pages.

## Test Plan

Not yet tested.

---

_Label `ci` added by @sharkdp on 2025-07-15 10:18_

---

_Label `ty` added by @sharkdp on 2025-07-15 10:18_

---

_Review requested from @Copilot by @sharkdp on 2025-07-15 10:21_

---

_@copilot-pull-request-reviewer[bot] reviewed on 2025-07-15 10:22_

## Pull Request Overview

Adds a scheduled GitHub Actions workflow that analyzes the repository, generates an ecosystem report, and deploys it to Cloudflare Pages.

- Introduces `ty-ecosystem-report.yaml` to run the ecosystem-analyzer on a weekly schedule (and on demand).
- Updates `.github/zizmor.yml` to include the new workflow in the repository’s workflow rules.

### Reviewed Changes

Copilot reviewed 2 out of 2 changed files in this pull request and generated no comments.

| File | Description |
| ---- | ----------- |
| .github/zizmor.yml | Added `ty-ecosystem-report.yaml` to the list of workflows tracked by zizmor. |
| .github/workflows/ty-ecosystem-report.yaml | New workflow to checkout code, run the ecosystem-analyzer tool, generate an HTML report, and deploy to Cloudflare Pages. |


<details>
<summary>Comments suppressed due to low confidence (4)</summary>

**.github/workflows/ty-ecosystem-report.yaml:38**
* [nitpick] The step name "Install Rust toolchain" doesn’t match the command `rustup show`, which only displays the current toolchain. Consider renaming the step to reflect its purpose (e.g., "Show Rust toolchain") or updating the command to actually install a specific toolchain.
```
      - name: Install Rust toolchain
```
**.github/workflows/ty-ecosystem-report.yaml:17**
* Defining `CF_API_TOKEN_EXISTS` as a boolean and then comparing it to the string `'true'` in the conditional may prevent the deploy step from running even when the token is set. You can simplify by using the secret directly in the `if`, for example: `if: ${{ secrets.CF_API_TOKEN != '' }}`.
```
  CF_API_TOKEN_EXISTS: ${{ secrets.CF_API_TOKEN != '' }}
```
**.github/workflows/ty-ecosystem-report.yaml:25**
* [nitpick] The checkout action is pinned to a specific commit SHA, which can make future updates harder to track. Consider using a release tag (e.g., `actions/checkout@v4`) for clearer intent and simpler upgrades.
```
      - uses: actions/checkout@11bd71901bbe5b1630ceea73d27597364c9af683 # v4.2.2
```
**.github/workflows/ty-ecosystem-report.yaml:3**
* The workflow’s `permissions: {}` configuration revokes all permissions by default. Verify that the deploy step (Cloudflare Pages via wrangler) has any required permissions (e.g., `pages: write` or `id-token: write`) or confirm that no additional permissions are needed.
```
permissions: {}
```
</details>



---

_Comment by @sharkdp on 2025-07-15 10:29_

Will merge to test this.

---

_Merged by @sharkdp on 2025-07-15 10:29_

---

_Closed by @sharkdp on 2025-07-15 10:29_

---

_Branch deleted on 2025-07-15 10:29_

---
