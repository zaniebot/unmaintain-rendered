```yaml
number: 17286
title: Update dependency astral-sh/uv to v0.9.18
type: pull_request
state: merged
author: renovate
labels:
  - internal
assignees: []
merged: true
base: main
head: renovate/astral-sh-uv-0.x
created_at: 2026-01-02T15:32:09Z
updated_at: 2026-01-02T15:43:30Z
url: https://github.com/astral-sh/uv/pull/17286
synced_at: 2026-01-10T05:49:14Z
```

# Update dependency astral-sh/uv to v0.9.18

---

_Pull request opened by @renovate on 2026-01-02 15:32_

This PR contains the following updates:

| Package | Type | Update | Change | Pending |
|---|---|---|---|---|
| [astral-sh/uv](https://redirect.github.com/astral-sh/uv) | uses-with | patch | `0.9.13` → `0.9.18` | `0.9.21` (+1) |

---

### Release Notes

<details>
<summary>astral-sh/uv (astral-sh/uv)</summary>

### [`v0.9.18`](https://redirect.github.com/astral-sh/uv/blob/HEAD/CHANGELOG.md#0918)

[Compare Source](https://redirect.github.com/astral-sh/uv/compare/0.9.17...0.9.18)

Released on 2025-12-16.

##### Enhancements

- Add value hints to command line arguments to improve shell completion accuracy ([#&#8203;17080](https://redirect.github.com/astral-sh/uv/pull/17080))
- Improve error handling in `uv publish` ([#&#8203;17096](https://redirect.github.com/astral-sh/uv/pull/17096))
- Improve rendering of multiline error messages ([#&#8203;17132](https://redirect.github.com/astral-sh/uv/pull/17132))
- Support redirects in `uv publish` ([#&#8203;17130](https://redirect.github.com/astral-sh/uv/pull/17130))
- Include Docker images with the alpine version, e.g., `python3.x-alpine3.23` ([#&#8203;17100](https://redirect.github.com/astral-sh/uv/pull/17100))

##### Configuration

- Accept `--torch-backend` in `[tool.uv]` ([#&#8203;17116](https://redirect.github.com/astral-sh/uv/pull/17116))

##### Performance

- Speed up `uv cache size` ([#&#8203;17015](https://redirect.github.com/astral-sh/uv/pull/17015))
- Initialize S3 signer once ([#&#8203;17092](https://redirect.github.com/astral-sh/uv/pull/17092))

##### Bug fixes

- Avoid panics due to reads on failed requests ([#&#8203;17098](https://redirect.github.com/astral-sh/uv/pull/17098))
- Enforce latest-version in `@latest` requests ([#&#8203;17114](https://redirect.github.com/astral-sh/uv/pull/17114))
- Explicitly set `EntryType` for file entries in tar ([#&#8203;17043](https://redirect.github.com/astral-sh/uv/pull/17043))
- Ignore `pyproject.toml` index username in lockfile comparison ([#&#8203;16995](https://redirect.github.com/astral-sh/uv/pull/16995))
- Relax error when using `uv add` with `UV_GIT_LFS` set ([#&#8203;17127](https://redirect.github.com/astral-sh/uv/pull/17127))
- Support file locks on ExFAT on macOS ([#&#8203;17115](https://redirect.github.com/astral-sh/uv/pull/17115))
- Change schema for `exclude-newer` into optional string ([#&#8203;17121](https://redirect.github.com/astral-sh/uv/pull/17121))

##### Documentation

- Drop arm musl caveat from Docker documentation ([#&#8203;17111](https://redirect.github.com/astral-sh/uv/pull/17111))
- Fix version reference in resolver example ([#&#8203;17085](https://redirect.github.com/astral-sh/uv/pull/17085))
- Better documentation for `exclude-newer*` ([#&#8203;17079](https://redirect.github.com/astral-sh/uv/pull/17079))

### [`v0.9.17`](https://redirect.github.com/astral-sh/uv/blob/HEAD/CHANGELOG.md#0917)

[Compare Source](https://redirect.github.com/astral-sh/uv/compare/0.9.16...0.9.17)

Released on 2025-12-09.

##### Enhancements

- Add `torch-tensorrt` and `torchao` to the PyTorch list ([#&#8203;17053](https://redirect.github.com/astral-sh/uv/pull/17053))
- Add hint for misplaced `--verbose`  in `uv tool run` ([#&#8203;17020](https://redirect.github.com/astral-sh/uv/pull/17020))
- Add support for relative durations in `exclude-newer` (a.k.a., dependency cooldowns) ([#&#8203;16814](https://redirect.github.com/astral-sh/uv/pull/16814))
- Add support for relocatable nushell activation script ([#&#8203;17036](https://redirect.github.com/astral-sh/uv/pull/17036))

##### Bug fixes

- Respect dropped (but explicit) indexes in dependency groups ([#&#8203;17012](https://redirect.github.com/astral-sh/uv/pull/17012))

##### Documentation

- Improve `source-exclude` reference docs ([#&#8203;16832](https://redirect.github.com/astral-sh/uv/pull/16832))
- Recommend `UV_NO_DEV` in Docker installs ([#&#8203;17030](https://redirect.github.com/astral-sh/uv/pull/17030))
- Update `UV_VERSION` in docs for GitLab CI/CD ([#&#8203;17040](https://redirect.github.com/astral-sh/uv/pull/17040))

### [`v0.9.16`](https://redirect.github.com/astral-sh/uv/blob/HEAD/CHANGELOG.md#0916)

[Compare Source](https://redirect.github.com/astral-sh/uv/compare/0.9.15...0.9.16)

Released on 2025-12-06.

##### Python

- Add CPython 3.14.2
- Add CPython 3.13.11

##### Enhancements

- Add a 5m default timeout to acquiring file locks to fail faster on deadlock ([#&#8203;16342](https://redirect.github.com/astral-sh/uv/pull/16342))
- Add a stub `debug` subcommand to `uv pip` announcing its intentional absence ([#&#8203;16966](https://redirect.github.com/astral-sh/uv/pull/16966))
- Add bounds in `uv add --script` ([#&#8203;16954](https://redirect.github.com/astral-sh/uv/pull/16954))
- Add brew specific message for `uv self update` ([#&#8203;16838](https://redirect.github.com/astral-sh/uv/pull/16838))
- Error when built wheel is for the wrong platform ([#&#8203;16074](https://redirect.github.com/astral-sh/uv/pull/16074))
- Filter wheels from PEP 751 files based on `--no-binary` et al in `uv pip compile` ([#&#8203;16956](https://redirect.github.com/astral-sh/uv/pull/16956))
- Support `--target` and `--prefix` in `uv pip list`, `uv pip freeze`, and `uv pip show` ([#&#8203;16955](https://redirect.github.com/astral-sh/uv/pull/16955))
- Tweak language for build backend validation errors ([#&#8203;16720](https://redirect.github.com/astral-sh/uv/pull/16720))
- Use explicit credentials cache instead of global static ([#&#8203;16768](https://redirect.github.com/astral-sh/uv/pull/16768))
- Enable SIMD in HTML parsing ([#&#8203;17010](https://redirect.github.com/astral-sh/uv/pull/17010))

##### Preview features

- Fix missing preview warning in `uv workspace metadata` ([#&#8203;16988](https://redirect.github.com/astral-sh/uv/pull/16988))
- Add a `uv auth helper --protocol bazel` command ([#&#8203;16886](https://redirect.github.com/astral-sh/uv/pull/16886))

##### Bug fixes

- Fix Pyston wheel compatibility tags ([#&#8203;16972](https://redirect.github.com/astral-sh/uv/pull/16972))
- Allow redundant entries in `tool.uv.build-backend.module-name` but emit warnings ([#&#8203;16928](https://redirect.github.com/astral-sh/uv/pull/16928))
- Fix infinite loop in non-attribute re-treats during HTML parsing ([#&#8203;17010](https://redirect.github.com/astral-sh/uv/pull/17010))

##### Documentation

- Clarify `--project` flag help text to indicate project discovery ([#&#8203;16965](https://redirect.github.com/astral-sh/uv/pull/16965))
- Regenerate the crates.io READMEs on release ([#&#8203;16992](https://redirect.github.com/astral-sh/uv/pull/16992))
- Update Docker integration guide to prefer `COPY` over `ADD` for simple cases ([#&#8203;16883](https://redirect.github.com/astral-sh/uv/pull/16883))
- Update PyTorch documentation to include information about supporting CUDA 13.0.x ([#&#8203;16957](https://redirect.github.com/astral-sh/uv/pull/16957))
- Update the versioning policy ([#&#8203;16710](https://redirect.github.com/astral-sh/uv/pull/16710))
- Upgrade PyTorch documentation to latest versions ([#&#8203;16970](https://redirect.github.com/astral-sh/uv/pull/16970))

### [`v0.9.15`](https://redirect.github.com/astral-sh/uv/blob/HEAD/CHANGELOG.md#0915)

[Compare Source](https://redirect.github.com/astral-sh/uv/compare/0.9.14...0.9.15)

Released on 2025-12-02.

##### Python

- Add CPython 3.14.1
- Add CPython 3.13.10

##### Enhancements

- Add ROCm 6.4 to `--torch-backend=auto` ([#&#8203;16919](https://redirect.github.com/astral-sh/uv/pull/16919))
- Add a Windows manifest to uv binaries ([#&#8203;16894](https://redirect.github.com/astral-sh/uv/pull/16894))
- Add LFS toggle to Git sources ([#&#8203;16143](https://redirect.github.com/astral-sh/uv/pull/16143))
- Cache source reads during resolution ([#&#8203;16888](https://redirect.github.com/astral-sh/uv/pull/16888))
- Allow reading requirements from scripts without an extension ([#&#8203;16923](https://redirect.github.com/astral-sh/uv/pull/16923))
- Allow reading requirements from scripts with HTTP(S) paths ([#&#8203;16891](https://redirect.github.com/astral-sh/uv/pull/16891))

##### Configuration

- Add `UV_HIDE_BUILD_OUTPUT` to omit build logs ([#&#8203;16885](https://redirect.github.com/astral-sh/uv/pull/16885))

##### Bug fixes

- Fix `uv-trampoline-builder` builds from crates.io by moving bundled executables ([#&#8203;16922](https://redirect.github.com/astral-sh/uv/pull/16922))
- Respect `NO_COLOR` and always show the command as a header when paging `uv help` output ([#&#8203;16908](https://redirect.github.com/astral-sh/uv/pull/16908))
- Use `0o666` permissions for flock files instead of `0o777` ([#&#8203;16845](https://redirect.github.com/astral-sh/uv/pull/16845))
- Revert "Bump `astral-tl` to v0.7.10 ([#&#8203;16887](https://redirect.github.com/astral-sh/uv/issues/16887))" to narrow down a regression causing hangs in metadata retrieval ([#&#8203;16938](https://redirect.github.com/astral-sh/uv/pull/16938))

##### Documentation

- Link to the uv version in crates.io member READMEs ([#&#8203;16939](https://redirect.github.com/astral-sh/uv/pull/16939))

### [`v0.9.14`](https://redirect.github.com/astral-sh/uv/blob/HEAD/CHANGELOG.md#0914)

[Compare Source](https://redirect.github.com/astral-sh/uv/compare/0.9.13...0.9.14)

Released on 2025-12-01.

##### Performance

- Bump `astral-tl` to v0.7.10 to enable SIMD for HTML parsing ([#&#8203;16887](https://redirect.github.com/astral-sh/uv/pull/16887))

##### Bug fixes

- Allow earlier post releases with exclusive ordering ([#&#8203;16881](https://redirect.github.com/astral-sh/uv/pull/16881))
- Prefer updating existing `.zshenv` over creating a new one in `tool update-shell` ([#&#8203;16866](https://redirect.github.com/astral-sh/uv/pull/16866))
- Respect `-e` flags in `uv add` ([#&#8203;16882](https://redirect.github.com/astral-sh/uv/pull/16882))

##### Enhancements

- Attach subcommand to User-Agent string ([#&#8203;16837](https://redirect.github.com/astral-sh/uv/pull/16837))
- Prefer `UV_WORKING_DIR` over `UV_WORKING_DIRECTORY` for consistency ([#&#8203;16884](https://redirect.github.com/astral-sh/uv/pull/16884))

</details>

---

### Configuration

📅 **Schedule**: Branch creation - Between 12:00 AM and 03:59 AM, only on Monday ( * 0-3 * * 1 ) (UTC), Automerge - At any time (no schedule defined).

🚦 **Automerge**: Disabled by config. Please merge this manually once you are satisfied.

♻ **Rebasing**: Whenever PR becomes conflicted, or you tick the rebase/retry checkbox.

🔕 **Ignore**: Close this PR and you won't be reminded about this update again.

---

 - [ ] <!-- rebase-check -->If you want to rebase/retry this PR, check this box

---

This PR was generated by [Mend Renovate](https://mend.io/renovate/). View the [repository job log](https://developer.mend.io/github/astral-sh/uv).
<!--renovate-debug:eyJjcmVhdGVkSW5WZXIiOiI0Mi42OS4xIiwidXBkYXRlZEluVmVyIjoiNDIuNjkuMSIsInRhcmdldEJyYW5jaCI6Im1haW4iLCJsYWJlbHMiOlsiaW50ZXJuYWwiXX0=-->


---

_Label `internal` added by @renovate[bot] on 2026-01-02 15:32_

---

_@charliermarsh approved on 2026-01-02 15:35_

---

_Merged by @charliermarsh on 2026-01-02 15:43_

---

_Closed by @charliermarsh on 2026-01-02 15:43_

---

_Branch deleted on 2026-01-02 15:43_

---
