```yaml
number: 13162
title: Update astral-sh/setup-uv action to v6
type: pull_request
state: merged
author: renovate
labels:
  - internal
assignees: []
merged: true
base: main
head: renovate/astral-sh-setup-uv-6.x
created_at: 2025-04-28T02:46:21Z
updated_at: 2025-04-28T08:53:36Z
url: https://github.com/astral-sh/uv/pull/13162
synced_at: 2026-01-12T16:10:34Z
```

# Update astral-sh/setup-uv action to v6

---

_@renovate_

This PR contains the following updates:

| Package | Type | Update | Change |
|---|---|---|---|
| [astral-sh/setup-uv](https://redirect.github.com/astral-sh/setup-uv) | action | major | `v5.4.2` -> `v6.0.0` |

---

### Release Notes

<details>
<summary>astral-sh/setup-uv (astral-sh/setup-uv)</summary>

### [`v6.0.0`](https://redirect.github.com/astral-sh/setup-uv/releases/tag/v6.0.0): 🌈 activate-environment and working-directory

[Compare Source](https://redirect.github.com/astral-sh/setup-uv/compare/v5.4.2...v6.0.0)

##### Changes

This version contains some breaking changes which have been gathering up for a while. Lets dive into them:

-   [Activate environment](#activate-environment)
-   [Working Directory](#working-directory)
-   [Default `cache-dependency-glob`](#default-cache-dependency-glob)
-   [Use default cache dir on self hosted runners](#use-default-cache-dir-on-self-hosted-runners)

##### Activate environment

In previous versions using the input `python-version` automatically activated a venv at the repository root.
This led to some unwanted side-effects, was sometimes unexpected and not flexible enough.

The venv activation is now explicitly controlled with the new input `activate-environment` (false by default):

```yaml
- name: Install the latest version of uv and activate the environment
  uses: astral-sh/setup-uv@v6
  with:
    activate-environment: true
- run: uv pip install pip
```

The venv gets created by the [`uv venv`](https://docs.astral.sh/uv/pip/environments/) command so the python version is controlled by the `python-version` input or the files `pyproject.toml`, `uv.toml`, `.python-version` in the `working-directory`.

##### Working Directory

The new input `working-directory` controls where we look for `pyproject.toml`, `uv.toml` and `.python-version` files
which are used to determine the version of uv and python to install.

It can also be used to control where the venv gets created.

```yaml
- name: Install uv based on the config files in the working-directory
  uses: astral-sh/setup-uv@v6
  with:
    working-directory: my/subproject/dir
```

> \[!CAUTION]
>
> The inputs `pyproject-file` and `uv-file` have been removed.

##### Default `cache-dependency-glob`

[@&#8203;ssbarnea](https://redirect.github.com/ssbarnea) found out that the default `cache-dependency-glob` was not suitable for a lot of users.

The old default

```yaml
cache-dependency-glob: |
  **/requirements*.txt
  **/uv.lock
```

is changed and should cover over 99.5% of use cases:

```yaml
cache-dependency-glob: |
  **/*(requirements|constraints)*.(txt|in)
  **/pyproject.toml
  **/uv.lock
```

> \[!NOTE]
>
> This shouldn't be a breaking change. The only thing you may notice is that your caches get invalidated once.

##### Use default cache dir on self hosted runners

The directory where uv stores its cache was always set to a directory in `RUNNER_TEMP`. For self-hosted runners this made no sense as this gets cleaned after every run and led to slower runs than necessary.

On self-hosted runners `UV_CACHE_DIR` is no longer set and the [default cache directory](https://docs.astral.sh/uv/concepts/cache/#cache-directory) is used instead.

##### 🚨 Breaking changes

-   Change default cache-dependency-glob [@&#8203;eifinger](https://redirect.github.com/eifinger) ([#&#8203;352](https://redirect.github.com/astral-sh/setup-uv/issues/352))
-   No default UV_CACHE_DIR on selfhosted runners [@&#8203;eifinger](https://redirect.github.com/eifinger) ([#&#8203;380](https://redirect.github.com/astral-sh/setup-uv/issues/380))
-   new inputs activate-environment and working-directory [@&#8203;eifinger](https://redirect.github.com/eifinger) ([#&#8203;381](https://redirect.github.com/astral-sh/setup-uv/issues/381))

##### 🧰 Maintenance

-   chore: update known checksums for 0.6.16 @&#8203;[github-actions\[bot\]](https://redirect.github.com/apps/github-actions) ([#&#8203;378](https://redirect.github.com/astral-sh/setup-uv/issues/378))
-   chore: update known checksums for 0.6.15 @&#8203;[github-actions\[bot\]](https://redirect.github.com/apps/github-actions) ([#&#8203;377](https://redirect.github.com/astral-sh/setup-uv/issues/377))

##### 📚 Documentation

-   bump to v6 in README [@&#8203;eifinger](https://redirect.github.com/eifinger) ([#&#8203;382](https://redirect.github.com/astral-sh/setup-uv/issues/382))
-   log info on venv activation [@&#8203;eifinger](https://redirect.github.com/eifinger) ([#&#8203;375](https://redirect.github.com/astral-sh/setup-uv/issues/375))

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

This PR was generated by [Mend Renovate](https://mend.io/renovate/). View the [repository job log](https://developer.mend.io/github/astral-sh/uv).
<!--renovate-debug:eyJjcmVhdGVkSW5WZXIiOiIzOS4yNTcuMyIsInVwZGF0ZWRJblZlciI6IjM5LjI1Ny4zIiwidGFyZ2V0QnJhbmNoIjoibWFpbiIsImxhYmVscyI6WyJpbnRlcm5hbCJdfQ==-->


---

_Label `internal` added by @renovate[bot] on 2025-04-28 02:46_

---

_Merged by @konstin on 2025-04-28 08:53_

---

_Closed by @konstin on 2025-04-28 08:53_

---

_Branch deleted on 2025-04-28 08:53_

---
