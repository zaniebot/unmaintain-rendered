```yaml
number: 17262
title: Update astral-sh/setup-uv action to v5.4.1
type: pull_request
state: merged
author: renovate
labels:
  - internal
assignees: []
merged: true
base: main
head: renovate/astral-sh-setup-uv-5.x
created_at: 2025-04-07T02:05:43Z
updated_at: 2025-04-07T06:47:36Z
url: https://github.com/astral-sh/ruff/pull/17262
synced_at: 2026-01-10T19:40:37Z
```

# Update astral-sh/setup-uv action to v5.4.1

---

_Pull request opened by @renovate on 2025-04-07 02:05_

This PR contains the following updates:

| Package | Type | Update | Change |
|---|---|---|---|
| [astral-sh/setup-uv](https://redirect.github.com/astral-sh/setup-uv) | action | minor | `v5` -> `v5.4.1` |

---

> [!WARNING]
> Some dependencies could not be looked up. Check the Dependency Dashboard for more information.

---

### Release Notes

<details>
<summary>astral-sh/setup-uv (astral-sh/setup-uv)</summary>

### [`v5.4.1`](https://redirect.github.com/astral-sh/setup-uv/releases/tag/v5.4.1): 🌈 Add support for pep440 version specifiers

[Compare Source](https://redirect.github.com/astral-sh/setup-uv/compare/v5.4.0...v5.4.1)

##### Changes

With this release you can also use [pep440 version specifiers](https://peps.python.org/pep-0440/#version-specifiers) as `required-version` in files`uv.toml`, `pyroject.toml` and in the `version` input:

```yaml
- name: Install a pep440-specifier-satisfying version of uv
  uses: astral-sh/setup-uv@v5
  with:
    version: ">=0.4.25,<0.5"
```

##### 🐛 Bug fixes

-   Add support for pep440 version identifiers [@&#8203;eifinger](https://redirect.github.com/eifinger) ([#&#8203;353](https://redirect.github.com/astral-sh/setup-uv/issues/353))

##### 🧰 Maintenance

-   chore: update known checksums for 0.6.10 @&#8203;[github-actions\[bot\]](https://redirect.github.com/apps/github-actions) ([#&#8203;345](https://redirect.github.com/astral-sh/setup-uv/issues/345))

##### 📚 Documentation

-   Add pep440 to docs header [@&#8203;eifinger](https://redirect.github.com/eifinger) ([#&#8203;355](https://redirect.github.com/astral-sh/setup-uv/issues/355))
-   Fix glob syntax link [@&#8203;flying-sheep](https://redirect.github.com/flying-sheep) ([#&#8203;349](https://redirect.github.com/astral-sh/setup-uv/issues/349))
-   Add link to supported glob patterns [@&#8203;eifinger](https://redirect.github.com/eifinger) ([#&#8203;348](https://redirect.github.com/astral-sh/setup-uv/issues/348))

### [`v5.4.0`](https://redirect.github.com/astral-sh/setup-uv/releases/tag/v5.4.0): 🌈 uv and uvx path as outputs

[Compare Source](https://redirect.github.com/astral-sh/setup-uv/compare/v5.3.1...v5.4.0)

#### Changes

The absolute paths to the uv and uvx binaries can now be accessed via the outputs `uv-path` and `uvx-path`.

`setup-uv` now also issues a warning if the working directory is empty. This makes users aware of the common mistake to run `setup-uv` before `actions/checkout`. You can remove the warning by setting `ignore-empty-workdir: true`

#### 🚀 Enhancements

-   Add uv-path and uvx-path output [@&#8203;eifinger](https://redirect.github.com/eifinger) ([#&#8203;341](https://redirect.github.com/astral-sh/setup-uv/issues/341))
-   Warn when the workdir is empty [@&#8203;eifinger](https://redirect.github.com/eifinger) ([#&#8203;322](https://redirect.github.com/astral-sh/setup-uv/issues/322))

#### 🧰 Maintenance

-   chore: update known checksums for 0.6.9 @&#8203;[github-actions\[bot\]](https://redirect.github.com/apps/github-actions) ([#&#8203;339](https://redirect.github.com/astral-sh/setup-uv/issues/339))
-   Merge workflows and add all-tests-passed [@&#8203;eifinger](https://redirect.github.com/eifinger) ([#&#8203;331](https://redirect.github.com/astral-sh/setup-uv/issues/331))
-   chore: update known checksums for 0.6.8 @&#8203;[github-actions\[bot\]](https://redirect.github.com/apps/github-actions) ([#&#8203;332](https://redirect.github.com/astral-sh/setup-uv/issues/332))
-   chore: update known checksums for 0.6.7 @&#8203;[github-actions\[bot\]](https://redirect.github.com/apps/github-actions) ([#&#8203;330](https://redirect.github.com/astral-sh/setup-uv/issues/330))
-   Set required workflow permissions [@&#8203;eifinger](https://redirect.github.com/eifinger) ([#&#8203;329](https://redirect.github.com/astral-sh/setup-uv/issues/329))
-   Add workflow_dispatch triggers to every workflow [@&#8203;eifinger](https://redirect.github.com/eifinger) ([#&#8203;326](https://redirect.github.com/astral-sh/setup-uv/issues/326))
-   Bump dependencies [@&#8203;eifinger](https://redirect.github.com/eifinger) ([#&#8203;324](https://redirect.github.com/astral-sh/setup-uv/issues/324))
-   Inline action-update-semver [@&#8203;eifinger](https://redirect.github.com/eifinger) ([#&#8203;323](https://redirect.github.com/astral-sh/setup-uv/issues/323))
-   chore: update known checksums for 0.6.6 @&#8203;[github-actions\[bot\]](https://redirect.github.com/apps/github-actions) ([#&#8203;318](https://redirect.github.com/astral-sh/setup-uv/issues/318))
-   chore: update known checksums for 0.6.5 @&#8203;[github-actions\[bot\]](https://redirect.github.com/apps/github-actions) ([#&#8203;313](https://redirect.github.com/astral-sh/setup-uv/issues/313))

#### 📚 Documentation

-   Fix wrong warning message in FAQ [@&#8203;eifinger](https://redirect.github.com/eifinger) ([#&#8203;337](https://redirect.github.com/astral-sh/setup-uv/issues/337))
-   Warn when the workdir is empty [@&#8203;eifinger](https://redirect.github.com/eifinger) ([#&#8203;322](https://redirect.github.com/astral-sh/setup-uv/issues/322))
-   Remove apk add python3 for musl test [@&#8203;eifinger](https://redirect.github.com/eifinger) ([#&#8203;319](https://redirect.github.com/astral-sh/setup-uv/issues/319))

#### ⬆️ Dependency updates

-   Bump [@&#8203;actions/cache](https://redirect.github.com/actions/cache) from 4.0.2 to 4.0.3 @&#8203;[dependabot\[bot\]](https://redirect.github.com/apps/dependabot) ([#&#8203;334](https://redirect.github.com/astral-sh/setup-uv/issues/334))

### [`v5.3.1`](https://redirect.github.com/astral-sh/setup-uv/releases/tag/v5.3.1): 🌈 - Fix issues with GHES and HTTP proxies

[Compare Source](https://redirect.github.com/astral-sh/setup-uv/compare/v5.3.0...v5.3.1)

##### Changes

This release fixes some issues when this action was used behind a HTTP proxy or with GHES.
If you have been seeing `ENOTFOUND` or timeout errors, this release should fix that.

A huge thank you to everyone who helped investigating this and testing the fixes:

-   [@&#8203;siryessuhr](https://redirect.github.com/siryessuhr)
-   [@&#8203;my1e5](https://redirect.github.com/my1e5)
-   [@&#8203;dennis-m-e](https://redirect.github.com/dennis-m-e)
-   [@&#8203;PaarthShah](https://redirect.github.com/PaarthShah)

##### 🐛 Bug fixes

-   Always fall back to anonymous download [@&#8203;eifinger](https://redirect.github.com/eifinger) ([#&#8203;304](https://redirect.github.com/astral-sh/setup-uv/issues/304))

##### 🧰 Maintenance

-   chore: update known checksums for 0.6.3 @&#8203;[github-actions\[bot\]](https://redirect.github.com/apps/github-actions) ([#&#8203;300](https://redirect.github.com/astral-sh/setup-uv/issues/300))

##### 📚 Documentation

-   📚 Document automatically enabled cache on GitHub-hosted runners [@&#8203;jerr0328](https://redirect.github.com/jerr0328) ([#&#8203;302](https://redirect.github.com/astral-sh/setup-uv/issues/302))

##### ⬆️ Dependency updates

-   bump dependencies [@&#8203;eifinger](https://redirect.github.com/eifinger) ([#&#8203;308](https://redirect.github.com/astral-sh/setup-uv/issues/308))
-   Bump peter-evans/create-pull-request from 7.0.6 to 7.0.7 @&#8203;[dependabot\[bot\]](https://redirect.github.com/apps/dependabot) ([#&#8203;299](https://redirect.github.com/astral-sh/setup-uv/issues/299))

### [`v5.3.0`](https://redirect.github.com/astral-sh/setup-uv/releases/tag/v5.3.0): 🌈 Support MUSL, s390x and powerpc

[Compare Source](https://redirect.github.com/astral-sh/setup-uv/compare/v5.2.2...v5.3.0)

In this release we add support for MUSL based systems.
This is helpful if you are running your workflow inside a docker image based on [alpine](https://hub.docker.com/\_/alpine).

> \[!TIP]
> Please be aware that you have to make sure a python interpreter is already present (`apk add python3`), see also https://docs.astral.sh/uv/concepts/python-versions/#cpython-distributions and [https://github.com/astral-sh/uv/issues/6890](https://redirect.github.com/astral-sh/uv/issues/6890)

[@&#8203;Zxilly](https://redirect.github.com/Zxilly) also added support for running this action on self-hosted runners using s390x and powerpc architectures. Thank you!

This release also includes more debug logs which makes tracking down issues easier in the future.

##### 🐛 Bug fixes

-   Add more debug logs [@&#8203;eifinger](https://redirect.github.com/eifinger) ([#&#8203;297](https://redirect.github.com/astral-sh/setup-uv/issues/297))

##### 🚀 Enhancements

-   Support OS using musl [@&#8203;eifinger](https://redirect.github.com/eifinger) ([#&#8203;284](https://redirect.github.com/astral-sh/setup-uv/issues/284))
-   feat: support s390x and powerpc [@&#8203;Zxilly](https://redirect.github.com/Zxilly) ([#&#8203;289](https://redirect.github.com/astral-sh/setup-uv/issues/289))

##### 🧰 Maintenance

-   chore: update known checksums for 0.6.2 @&#8203;[github-actions\[bot\]](https://redirect.github.com/apps/github-actions) ([#&#8203;295](https://redirect.github.com/astral-sh/setup-uv/issues/295))
-   chore: update known checksums for 0.6.1 @&#8203;[github-actions\[bot\]](https://redirect.github.com/apps/github-actions) ([#&#8203;293](https://redirect.github.com/astral-sh/setup-uv/issues/293))
-   chore: update known checksums for 0.6.0 @&#8203;[github-actions\[bot\]](https://redirect.github.com/apps/github-actions) ([#&#8203;288](https://redirect.github.com/astral-sh/setup-uv/issues/288))
-   chore: update known checksums for 0.5.31 @&#8203;[github-actions\[bot\]](https://redirect.github.com/apps/github-actions) ([#&#8203;277](https://redirect.github.com/astral-sh/setup-uv/issues/277))
-   Run update-known-checksums every night [@&#8203;eifinger](https://redirect.github.com/eifinger) ([#&#8203;273](https://redirect.github.com/astral-sh/setup-uv/issues/273))
-   chore: update known checksums for 0.5.29 @&#8203;[github-actions\[bot\]](https://redirect.github.com/apps/github-actions) ([#&#8203;272](https://redirect.github.com/astral-sh/setup-uv/issues/272))
-   chore: update known checksums for 0.5.28 @&#8203;[github-actions\[bot\]](https://redirect.github.com/apps/github-actions) ([#&#8203;270](https://redirect.github.com/astral-sh/setup-uv/issues/270))
-   chore: update known checksums for 0.5.27 @&#8203;[github-actions\[bot\]](https://redirect.github.com/apps/github-actions) ([#&#8203;267](https://redirect.github.com/astral-sh/setup-uv/issues/267))
-   chore: update known checksums for 0.5.26 @&#8203;[github-actions\[bot\]](https://redirect.github.com/apps/github-actions) ([#&#8203;263](https://redirect.github.com/astral-sh/setup-uv/issues/263))

##### 📚 Documentation

-   Add FAQ on resolution strategy and cache not found warnings [@&#8203;eifinger](https://redirect.github.com/eifinger) ([#&#8203;296](https://redirect.github.com/astral-sh/setup-uv/issues/296))

### [`v5.2.2`](https://redirect.github.com/astral-sh/setup-uv/releases/tag/v5.2.2): 🌈 Full support for GHES

[Compare Source](https://redirect.github.com/astral-sh/setup-uv/compare/v5.2.1...v5.2.2)

##### Changes

This release fixes some issues that prevented use with GitHub Enterprise Server instances.

##### 🐛 Bug fixes

-   Do not expect GITHUB_TOKEN to be set or valid [@&#8203;eifinger](https://redirect.github.com/eifinger) ([#&#8203;262](https://redirect.github.com/astral-sh/setup-uv/issues/262))
-   Fallback if toml file parsing failed [@&#8203;eifinger](https://redirect.github.com/eifinger) ([#&#8203;246](https://redirect.github.com/astral-sh/setup-uv/issues/246))

##### 🧰 Maintenance

-   chore: update known checksums for 0.5.25 @&#8203;[github-actions\[bot\]](https://redirect.github.com/apps/github-actions) ([#&#8203;259](https://redirect.github.com/astral-sh/setup-uv/issues/259))
-   chore: update known checksums for 0.5.24 @&#8203;[github-actions\[bot\]](https://redirect.github.com/apps/github-actions) ([#&#8203;256](https://redirect.github.com/astral-sh/setup-uv/issues/256))
-   chore: update known checksums for 0.5.23 @&#8203;[github-actions\[bot\]](https://redirect.github.com/apps/github-actions) ([#&#8203;252](https://redirect.github.com/astral-sh/setup-uv/issues/252))
-   chore: update known checksums for 0.5.22 @&#8203;[github-actions\[bot\]](https://redirect.github.com/apps/github-actions) ([#&#8203;250](https://redirect.github.com/astral-sh/setup-uv/issues/250))
-   chore: update known checksums for 0.5.21 @&#8203;[github-actions\[bot\]](https://redirect.github.com/apps/github-actions) ([#&#8203;247](https://redirect.github.com/astral-sh/setup-uv/issues/247))

##### 📚 Documentation

-   Fix TOC [@&#8203;eifinger](https://redirect.github.com/eifinger) ([#&#8203;257](https://redirect.github.com/astral-sh/setup-uv/issues/257))

##### ⬆️ Dependency updates

-   Bump [@&#8203;types/node](https://redirect.github.com/types/node) from 22.10.10 to 22.12.0 @&#8203;[dependabot\[bot\]](https://redirect.github.com/apps/dependabot) ([#&#8203;258](https://redirect.github.com/astral-sh/setup-uv/issues/258))
-   Bump release-drafter/release-drafter from 6.0.0 to 6.1.0 @&#8203;[dependabot\[bot\]](https://redirect.github.com/apps/dependabot) ([#&#8203;249](https://redirect.github.com/astral-sh/setup-uv/issues/249))
-   Bump [@&#8203;types/node](https://redirect.github.com/types/node) from 22.10.9 to 22.10.10 @&#8203;[dependabot\[bot\]](https://redirect.github.com/apps/dependabot) ([#&#8203;254](https://redirect.github.com/astral-sh/setup-uv/issues/254))
-   Bump [@&#8203;types/node](https://redirect.github.com/types/node) from 22.10.7 to 22.10.9 @&#8203;[dependabot\[bot\]](https://redirect.github.com/apps/dependabot) ([#&#8203;253](https://redirect.github.com/astral-sh/setup-uv/issues/253))
-   Bump [@&#8203;actions/tool-cache](https://redirect.github.com/actions/tool-cache) from 2.0.1 to 2.0.2 @&#8203;[dependabot\[bot\]](https://redirect.github.com/apps/dependabot) ([#&#8203;244](https://redirect.github.com/astral-sh/setup-uv/issues/244))
-   Bump [@&#8203;types/node](https://redirect.github.com/types/node) from 22.10.6 to 22.10.7 @&#8203;[dependabot\[bot\]](https://redirect.github.com/apps/dependabot) ([#&#8203;243](https://redirect.github.com/astral-sh/setup-uv/issues/243))

### [`v5.2.1`](https://redirect.github.com/astral-sh/setup-uv/releases/tag/v5.2.1): 🌈 Support toml spec 1.0.0

[Compare Source](https://redirect.github.com/astral-sh/setup-uv/compare/v5.2.0...v5.2.1)

v5.2.0 introduced TOML parsing using [@&#8203;iarna/toml](https://www.npmjs.com/package/@&#8203;iarna/toml) because we already found out in `astral-sh/ruff-action` that [toml](https://www.npmjs.com/package/toml) has missing features.

As it turns out [@&#8203;iarna/toml](https://www.npmjs.com/package/@&#8203;iarna/toml) also is not fully TOML spec (1.0.0) compliant.

We now use [smol-toml](https://www.npmjs.com/package/smol-toml)

##### 🐛 Bug fixes

-   Support toml spec 1.0.0 [@&#8203;eifinger](https://redirect.github.com/eifinger) ([#&#8203;245](https://redirect.github.com/astral-sh/setup-uv/issues/245))

### [`v5.2.0`](https://redirect.github.com/astral-sh/setup-uv/releases/tag/v5.2.0): 🌈 Detect required-version from config file

[Compare Source](https://redirect.github.com/astral-sh/setup-uv/compare/v5.1.0...v5.2.0)

This release adds support to derive the version of uv to be installed from `pyproject.toml` and `uv.toml` files.
If no `version` input is defined the default is now to look for a [required-version](https://docs.astral.sh/uv/reference/settings/#required-version) in `uv.toml` and then `pyproject.toml` in the repository root. If it cannot find any it falls back to `latest`.

If your files are at a different place you can use the new inputs `uv-file` or `pyproject-file`.

##### 🐛 Bug fixes

-   Add venv/bin as absolute path to PATH [@&#8203;op](https://redirect.github.com/op) ([#&#8203;241](https://redirect.github.com/astral-sh/setup-uv/issues/241))
-   fix: make sure VIRTUAL_ENV is an absolute path [@&#8203;samypr100](https://redirect.github.com/samypr100) ([#&#8203;224](https://redirect.github.com/astral-sh/setup-uv/issues/224))

##### 🚀 Enhancements

-   Detect required-version from config file [@&#8203;eifinger](https://redirect.github.com/eifinger) ([#&#8203;233](https://redirect.github.com/astral-sh/setup-uv/issues/233))

##### 🧰 Maintenance

-   chore: update known checksums for 0.5.20 [@&#8203;github-actions](https://redirect.github.com/github-actions) ([#&#8203;238](https://redirect.github.com/astral-sh/setup-uv/issues/238))
-   chore: update known checksums for 0.5.19 [@&#8203;github-actions](https://redirect.github.com/github-actions) ([#&#8203;237](https://redirect.github.com/astral-sh/setup-uv/issues/237))
-   chore: update known checksums for 0.5.18 [@&#8203;github-actions](https://redirect.github.com/github-actions) ([#&#8203;232](https://redirect.github.com/astral-sh/setup-uv/issues/232))
-   chore: update known checksums for 0.5.17 [@&#8203;github-actions](https://redirect.github.com/github-actions) ([#&#8203;231](https://redirect.github.com/astral-sh/setup-uv/issues/231))
-   chore: update known checksums for 0.5.16 [@&#8203;github-actions](https://redirect.github.com/github-actions) ([#&#8203;228](https://redirect.github.com/astral-sh/setup-uv/issues/228))
-   chore: update known checksums for 0.5.15 [@&#8203;github-actions](https://redirect.github.com/github-actions) ([#&#8203;225](https://redirect.github.com/astral-sh/setup-uv/issues/225))
-   chore: update known checksums for 0.5.14 [@&#8203;github-actions](https://redirect.github.com/github-actions) ([#&#8203;222](https://redirect.github.com/astral-sh/setup-uv/issues/222))
-   chore: update known checksums for 0.5.12 [@&#8203;github-actions](https://redirect.github.com/github-actions) ([#&#8203;214](https://redirect.github.com/astral-sh/setup-uv/issues/214))

##### 📚 Documentation

-   docs: bump `astral-sh/setup-uv` to `v5` [@&#8203;njzjz](https://redirect.github.com/njzjz) ([#&#8203;205](https://redirect.github.com/astral-sh/setup-uv/issues/205))

##### ⬆️ Dependency updates

-   Bump [@&#8203;octokit/rest](https://redirect.github.com/octokit/rest) from 21.0.2 to 21.1.0 [@&#8203;dependabot](https://redirect.github.com/dependabot) ([#&#8203;229](https://redirect.github.com/astral-sh/setup-uv/issues/229))
-   Bump typescript from 5.7.2 to 5.7.3 [@&#8203;dependabot](https://redirect.github.com/dependabot) ([#&#8203;230](https://redirect.github.com/astral-sh/setup-uv/issues/230))
-   Bump [@&#8203;types/node](https://redirect.github.com/types/node) from 22.10.5 to 22.10.6 [@&#8203;dependabot](https://redirect.github.com/dependabot) ([#&#8203;236](https://redirect.github.com/astral-sh/setup-uv/issues/236))
-   Bump [@&#8203;types/node](https://redirect.github.com/types/node) from 22.10.3 to 22.10.5 [@&#8203;dependabot](https://redirect.github.com/dependabot) ([#&#8203;223](https://redirect.github.com/astral-sh/setup-uv/issues/223))
-   Bump [@&#8203;types/node](https://redirect.github.com/types/node) from 22.10.2 to 22.10.3 [@&#8203;dependabot](https://redirect.github.com/dependabot) ([#&#8203;220](https://redirect.github.com/astral-sh/setup-uv/issues/220))
-   Bump peter-evans/create-pull-request from 7.0.5 to 7.0.6 [@&#8203;dependabot](https://redirect.github.com/dependabot) ([#&#8203;218](https://redirect.github.com/astral-sh/setup-uv/issues/218))

### [`v5.1.0`](https://redirect.github.com/astral-sh/setup-uv/releases/tag/v5.1.0): 🌈 Fewer cache invalidations

[Compare Source](https://redirect.github.com/astral-sh/setup-uv/compare/v5.0.1...v5.1.0)

##### Changes

This release includes less frequently invalidated caches and a fix for setting the correct `VIRTUAL_ENV`

##### 🐛 Bug fixes

-   Set VIRTUAL_ENV to .venv instead of .venv/bin [@&#8203;eifinger](https://redirect.github.com/eifinger) ([#&#8203;210](https://redirect.github.com/astral-sh/setup-uv/issues/210))

##### 🚀 Enhancements

-   Remove uv version from cache key [@&#8203;eifinger](https://redirect.github.com/eifinger) ([#&#8203;206](https://redirect.github.com/astral-sh/setup-uv/issues/206))

##### 📚 Documentation

-   Align use of `actions/setup-python` with uv docu [@&#8203;eifinger](https://redirect.github.com/eifinger) ([#&#8203;207](https://redirect.github.com/astral-sh/setup-uv/issues/207))

### [`v5.0.1`](https://redirect.github.com/astral-sh/setup-uv/releases/tag/v5.0.1): 🌈 The christmas elves overlooked something

[Compare Source](https://redirect.github.com/astral-sh/setup-uv/compare/v5...v5.0.1)

##### Changes

With so many breaking changes so close to the end of the year we missed something.

Thank you [@&#8203;ryanhiebert](https://redirect.github.com/ryanhiebert) for quickly reporting that our new defaults fail the workflow if neither a `uv.lock` nor a `requirements*.txt` can be found. This is now a warning instead.

##### 🐛 Bug fixes

-   Fix wrong cacheDependencyPathHash [@&#8203;eifinger](https://redirect.github.com/eifinger) ([#&#8203;201](https://redirect.github.com/astral-sh/setup-uv/issues/201))
-   Warn instead of fail for no-dependency-glob [@&#8203;eifinger](https://redirect.github.com/eifinger) ([#&#8203;200](https://redirect.github.com/astral-sh/setup-uv/issues/200))

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
<!--renovate-debug:eyJjcmVhdGVkSW5WZXIiOiIzOS4yMjcuMyIsInVwZGF0ZWRJblZlciI6IjM5LjIyNy4zIiwidGFyZ2V0QnJhbmNoIjoibWFpbiIsImxhYmVscyI6WyJpbnRlcm5hbCJdfQ==-->


---

_Label `internal` added by @renovate[bot] on 2025-04-07 02:05_

---

_Comment by @github-actions[bot] on 2025-04-07 02:08_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Closed by @MichaReiser on 2025-04-07 06:33_

---

_Reopened by @MichaReiser on 2025-04-07 06:33_

---

_Merged by @MichaReiser on 2025-04-07 06:44_

---

_Closed by @MichaReiser on 2025-04-07 06:44_

---

_Branch deleted on 2025-04-07 06:44_

---

_Comment by @github-actions[bot] on 2025-04-07 06:47_

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
