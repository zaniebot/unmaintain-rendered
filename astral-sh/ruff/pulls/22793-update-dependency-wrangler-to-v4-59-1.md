```yaml
number: 22793
title: Update dependency wrangler to v4.59.1
type: pull_request
state: open
author: renovate
labels:
  - internal
  - security
assignees: []
base: main
head: renovate/npm-wrangler-vulnerability
created_at: 2026-01-22T01:47:02Z
updated_at: 2026-01-22T01:47:03Z
url: https://github.com/astral-sh/ruff/pull/22793
synced_at: 2026-01-22T02:09:04Z
```

# Update dependency wrangler to v4.59.1

---

_@renovate_

This PR contains the following updates:

| Package | Change | [Age](https://docs.renovatebot.com/merge-confidence/) | [Confidence](https://docs.renovatebot.com/merge-confidence/) |
|---|---|---|---|
| [wrangler](https://redirect.github.com/cloudflare/workers-sdk) ([source](https://redirect.github.com/cloudflare/workers-sdk/tree/HEAD/packages/wrangler)) | [`4.58.0` → `4.59.1`](https://renovatebot.com/diffs/npm/wrangler/4.58.0/4.59.1) | ![age](https://developer.mend.io/api/mc/badges/age/npm/wrangler/4.59.1?slim=true) | ![confidence](https://developer.mend.io/api/mc/badges/confidence/npm/wrangler/4.58.0/4.59.1?slim=true) |

### GitHub Vulnerability Alerts

#### [CVE-2026-0933](https://redirect.github.com/cloudflare/workers-sdk/security/advisories/GHSA-36p8-mvp6-cv38)

**Summary**

A command injection vulnerability (CWE-78) has been found to exist in the `wrangler pages deploy` command. The issue occurs because the `--commit-hash` parameter is passed directly to a shell command without proper validation or sanitization, allowing an attacker with control of `--commit-hash` to execute arbitrary commands on the system running Wrangler.

**Root cause**

The `commitHash` variable, derived from user input via the `--commit-hash` CLI argument, is interpolated directly into a shell command using template literals (e.g., ``execSync(`git show -s --format=%B ${commitHash}`)``). Shell metacharacters are interpreted by the shell, enabling command execution.

**Impact**

This vulnerability is generally hard to exploit, as it requires `--commit-hash` to be attacker controlled. The vulnerability primarily affects CI/CD environments where `wrangler pages deploy` is used in automated pipelines and the `--commit-hash` parameter is populated from external, potentially untrusted sources. An attacker could exploit this to:

- Run any shell command.
- Exfiltrate environment variables.
- Compromise the CI runner to install backdoors or modify build artifacts.

**Mitigation**

- Wrangler v4 users are requested to upgrade to Wrangler v4.59.1 or higher. 
- Wrangler v3 users are requested to upgrade to Wrangler v3.114.17 or higher. 
- Users on Wrangler v2 (EOL) should upgrade to a supported major version.

**Credits**

Disclosed responsibly by kny4hacker.

---

### Release Notes

<details>
<summary>cloudflare/workers-sdk (wrangler)</summary>

### [`v4.59.1`](https://redirect.github.com/cloudflare/workers-sdk/blob/HEAD/packages/wrangler/CHANGELOG.md#4591)

[Compare Source](https://redirect.github.com/cloudflare/workers-sdk/compare/wrangler@4.59.0...wrangler@4.59.1)

##### Patch Changes

- [#&#8203;11889](https://redirect.github.com/cloudflare/workers-sdk/pull/11889) [`99b1f32`](https://redirect.github.com/cloudflare/workers-sdk/commit/99b1f328a9afe181b49f1114ed47f15f6d25f0be) Thanks [@&#8203;emily-shen](https://redirect.github.com/emily-shen)! - Use argument array when executing git commands with `wrangler pages deploy`

  Pass user provided values from `--commit-hash` safely to underlying git command.

### [`v4.59.0`](https://redirect.github.com/cloudflare/workers-sdk/blob/HEAD/packages/wrangler/CHANGELOG.md#4590)

[Compare Source](https://redirect.github.com/cloudflare/workers-sdk/compare/wrangler@4.58.0...wrangler@4.59.0)

##### Minor Changes

- [#&#8203;11852](https://redirect.github.com/cloudflare/workers-sdk/pull/11852) [`ad65efa`](https://redirect.github.com/cloudflare/workers-sdk/commit/ad65efa73ae8b666e1669964ccacc2680b12c853) Thanks [@&#8203;NuroDev](https://redirect.github.com/NuroDev)! - Add `--check` flag to `wrangler types` command

  The new `--check` flag allows you to verify that your generated types file is up-to-date without regenerating it. This is useful for CI/CD pipelines, pre-commit hooks, or any scenario where you want to ensure types have been committed after configuration changes.

  When types are up-to-date, the command exits with code 0:

  ```bash
  $ wrangler types --check
  ✨ Types at worker-configuration.d.ts are up to date.
  ```

  When types are out-of-date, the command exits with code 1:

  ```bash
  $ wrangler types --check
  ✘ [ERROR] Types at worker-configuration.d.ts are out of date. Run `wrangler types` to regenerate.
  ```

  You can also use it with a custom output path:

  ```bash
  $ wrangler types ./custom-types.d.ts --check
  ```

- [#&#8203;11529](https://redirect.github.com/cloudflare/workers-sdk/pull/11529) [`43d5363`](https://redirect.github.com/cloudflare/workers-sdk/commit/43d5363c7b40191723e9bab9900edd70ecac5837) Thanks [@&#8203;matthewdavidrodgers](https://redirect.github.com/matthewdavidrodgers)! - Add ability to enable higher asset count limits for Pages deployments

  Wrangler can now read asset count limits from JWT claims during Pages deployments,
  allowing users to be enabled for higher limits (up to 100,000 assets) on a per-account
  basis. The default limit remains at 20,000 assets.

- [#&#8203;11755](https://redirect.github.com/cloudflare/workers-sdk/pull/11755) [`0f8d69d`](https://redirect.github.com/cloudflare/workers-sdk/commit/0f8d69d31071abeb567aa3c8478492536b5740fb) Thanks [@&#8203;nikitassharma](https://redirect.github.com/nikitassharma)! - Users can now specify `constraints.tiers` for their container applications. `tier` is deprecated in favor of `tiers`.
  If left unset, we will default to `tiers: [1, 2]`.
  Note that `constraints` is an experimental feature.

##### Patch Changes

- [#&#8203;11820](https://redirect.github.com/cloudflare/workers-sdk/pull/11820) [`b0e54b2`](https://redirect.github.com/cloudflare/workers-sdk/commit/b0e54b26f261234ec47dcc673a5240734ba03fcc) Thanks [@&#8203;MattieTK](https://redirect.github.com/MattieTK)! - Add AI agent detection to analytics events

  Wrangler now detects when commands are executed by AI coding agents (such as Claude Code, Cursor, GitHub Copilot, etc.) using the `am-i-vibing` library. This information is included as an `agent` property in all analytics events, helping Cloudflare understand how developers interact with Wrangler through AI assistants.

  The `agent` property will contain the agent ID (e.g., `"claude-code"`, `"cursor-agent"`) when detected, or `null` when running outside an agentic environment.

- [#&#8203;11494](https://redirect.github.com/cloudflare/workers-sdk/pull/11494) [`ed60c4f`](https://redirect.github.com/cloudflare/workers-sdk/commit/ed60c4f01c0e4ac9683a73fb5cf849ad74255b35) Thanks [@&#8203;jalmonter](https://redirect.github.com/jalmonter)! - Fix scheduled trigger warning showing `undefined` port

  When running `wrangler dev` with a worker that has cron triggers, the warning message displayed an invalid URL like `curl "http://localhost:undefined/cdn-cgi/handler/scheduled"` because the port wasn't yet determined when the warning was logged.

  Moved the warning to after the proxy server is fully ready, where the actual public URL and port are known.

- [#&#8203;11831](https://redirect.github.com/cloudflare/workers-sdk/pull/11831) [`faa5753`](https://redirect.github.com/cloudflare/workers-sdk/commit/faa5753fa685117065c801e5d1fcee3486a6f0bd) Thanks [@&#8203;dependabot](https://redirect.github.com/apps/dependabot)! - chore: update dependencies of "miniflare", "wrangler"

  The following dependency versions have been updated:

  | Dependency | From         | To           |
  | ---------- | ------------ | ------------ |
  | workerd    | 1.20260107.1 | 1.20260108.0 |

- [#&#8203;11844](https://redirect.github.com/cloudflare/workers-sdk/pull/11844) [`e574ef3`](https://redirect.github.com/cloudflare/workers-sdk/commit/e574ef3e73aa00ca82e84fe308da0fed768477d9) Thanks [@&#8203;dependabot](https://redirect.github.com/apps/dependabot)! - chore: update dependencies of "miniflare", "wrangler"

  The following dependency versions have been updated:

  | Dependency | From         | To           |
  | ---------- | ------------ | ------------ |
  | workerd    | 1.20260108.0 | 1.20260109.0 |

- [#&#8203;11872](https://redirect.github.com/cloudflare/workers-sdk/pull/11872) [`b6148ed`](https://redirect.github.com/cloudflare/workers-sdk/commit/b6148ed733f6d6873261df5ae61e71c475ba8a8d) Thanks [@&#8203;dependabot](https://redirect.github.com/apps/dependabot)! - chore: update dependencies of "miniflare", "wrangler"

  The following dependency versions have been updated:

  | Dependency | From         | To           |
  | ---------- | ------------ | ------------ |
  | workerd    | 1.20260109.0 | 1.20260111.0 |

- [#&#8203;11843](https://redirect.github.com/cloudflare/workers-sdk/pull/11843) [`ab3859c`](https://redirect.github.com/cloudflare/workers-sdk/commit/ab3859c597fe30cdcd9ffa67f9fb7865539bf592) Thanks [@&#8203;dario-piotrowicz](https://redirect.github.com/dario-piotrowicz)! - Update the Wrangler autoconfig logic to work with the latest version of Waku

  The latest version of Waku (`0.12.5-1.0.0-alpha.1-0`) requires a `src/waku.server.tsx` file instead of a `src/server-entry.tsx` one, so the Wrangler autoconfig logic (the logic being run as part of `wrangler setup` and `wrangler deploy --x-autoconfig` that configures a project to be deployable on Cloudflare) has been updated accordingly.

  Also the way the worker needs to handle static assets has been updated as recommended from the Waku team.

- [#&#8203;11848](https://redirect.github.com/cloudflare/workers-sdk/pull/11848) [`0eb973d`](https://redirect.github.com/cloudflare/workers-sdk/commit/0eb973deb57b8d8b9bb2fe4e5cb471fabab51bac) Thanks [@&#8203;petebacondarwin](https://redirect.github.com/petebacondarwin)! - Fix incorrect warning about multiple environments when using redirected config

  Previously, when using a redirected config (via `configPath` in another config file) that originated from a config with multiple environments, wrangler would incorrectly warn about missing environment specification. This fix ensures the warning is only shown when the actual config being used has multiple environments defined, not when the original config did.

- Updated dependencies \[[`ed60c4f`](https://redirect.github.com/cloudflare/workers-sdk/commit/ed60c4f01c0e4ac9683a73fb5cf849ad74255b35), [`5c59217`](https://redirect.github.com/cloudflare/workers-sdk/commit/5c5921768f928de4526a315bb508e3ed25a2ccad), [`faa5753`](https://redirect.github.com/cloudflare/workers-sdk/commit/faa5753fa685117065c801e5d1fcee3486a6f0bd), [`e574ef3`](https://redirect.github.com/cloudflare/workers-sdk/commit/e574ef3e73aa00ca82e84fe308da0fed768477d9), [`b6148ed`](https://redirect.github.com/cloudflare/workers-sdk/commit/b6148ed733f6d6873261df5ae61e71c475ba8a8d), [`beb96af`](https://redirect.github.com/cloudflare/workers-sdk/commit/beb96af470aefaae73237309244cf7369b329ff0), [`5c59217`](https://redirect.github.com/cloudflare/workers-sdk/commit/5c5921768f928de4526a315bb508e3ed25a2ccad), [`fc96e5f`](https://redirect.github.com/cloudflare/workers-sdk/commit/fc96e5fe117948c57e49bf0741d55955691f1c28)]:
  - miniflare\@&#8203;4.20260111.0
  - [@&#8203;cloudflare/unenv-preset](https://redirect.github.com/cloudflare/unenv-preset)@&#8203;2.9.0

</details>

---

### Configuration

📅 **Schedule**: Branch creation - "" (UTC), Automerge - At any time (no schedule defined).

🚦 **Automerge**: Disabled by config. Please merge this manually once you are satisfied.

♻ **Rebasing**: Whenever PR becomes conflicted, or you tick the rebase/retry checkbox.

🔕 **Ignore**: Close this PR and you won't be reminded about this update again.

---

 - [ ] <!-- rebase-check -->If you want to rebase/retry this PR, check this box

---

This PR was generated by [Mend Renovate](https://mend.io/renovate/). View the [repository job log](https://developer.mend.io/github/astral-sh/ruff).
<!--renovate-debug:eyJjcmVhdGVkSW5WZXIiOiI0Mi44NS4xIiwidXBkYXRlZEluVmVyIjoiNDIuODUuMSIsInRhcmdldEJyYW5jaCI6Im1haW4iLCJsYWJlbHMiOlsiaW50ZXJuYWwiLCJzZWN1cml0eSJdfQ==-->


---

_Label `internal` added by @renovate[bot] on 2026-01-22 01:47_

---

_Label `security` added by @renovate[bot] on 2026-01-22 01:47_

---

_Label `internal` added by @renovate[bot] on 2026-01-22 01:47_

---

_Label `security` added by @renovate[bot] on 2026-01-22 01:47_

---
