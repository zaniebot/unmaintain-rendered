---
number: 31
title: Dependency Dashboard
type: issue
state: open
author: renovate
labels:
  - internal
assignees: []
created_at: 2025-05-05T16:20:40Z
updated_at: 2026-01-09T20:35:59Z
url: https://github.com/astral-sh/ty/issues/31
synced_at: 2026-01-10T01:48:23Z
---

# Dependency Dashboard

---

_Issue opened by @renovate on 2025-05-05 16:20_

This issue lists Renovate updates and detected dependencies. Read the [Dependency Dashboard](https://docs.renovatebot.com/key-concepts/dashboard/) docs to learn more.<br>[View this repository on the Mend.io Web Portal](https://developer.mend.io/github/astral-sh/ty).

> [!NOTE]
These dependencies have not received updates for an extended period and may be unmaintained:

<details>
<summary>View abandoned dependencies (1)</summary>

| Datasource | Name | Last Updated |
|------------|------|-------------|
| github-actions | `addnab/docker-run-action` | `2021-03-17` |

</details>

Packages are marked as abandoned when they exceed the [`abandonmentThreshold`](https://docs.renovatebot.com/configuration-options/#abandonmentthreshold) since their last release.
Unlike deprecated packages with official notices, abandonment is detected by release inactivity.


## Awaiting Schedule

The following updates are awaiting their schedule. To get an update now, click on a checkbox below.

 - [ ] <!-- unschedule-branch=renovate/actions-checkout-digest -->Update actions/checkout digest to 0c366fd
 - [ ] <!-- unschedule-branch=renovate/pre-commit-dependencies -->Update pre-commit dependencies (`astral-sh/uv-pre-commit`, `crate-ci/typos`, `rhysd/actionlint`)
 - [ ] <!-- unschedule-branch=renovate/astral-sh-setup-uv-7.x -->Update astral-sh/setup-uv action to v7.2.0
 - [ ] <!-- create-all-awaiting-schedule-prs -->🔐 **Create all awaiting schedule PRs at once** 🔐

## Detected Dependencies

<details><summary>github-actions (7)</summary>
<blockquote>

<details><summary>.github/workflows/build-binaries.yml (42)</summary>

 - `actions/checkout v6.0.1@8e8c483db84b4bee98b60c0593521ed34d9990e8`
 - `actions/setup-python v6.1.0@83679a892e2d95755f2dac6acb0bfd1e9ac5d548`
 - `PyO3/maturin-action v1.49.4@86b9d133d34bc1b40018696f782949dac11bd380`
 - `actions/upload-artifact v6.0.0@b7c566a772e6b6bfb58ed0dc250532a479d7789f`
 - `actions/checkout v6.0.1@8e8c483db84b4bee98b60c0593521ed34d9990e8`
 - `actions/setup-python v6.1.0@83679a892e2d95755f2dac6acb0bfd1e9ac5d548`
 - `PyO3/maturin-action v1.49.4@86b9d133d34bc1b40018696f782949dac11bd380`
 - `actions/upload-artifact v6.0.0@b7c566a772e6b6bfb58ed0dc250532a479d7789f`
 - `actions/upload-artifact v6.0.0@b7c566a772e6b6bfb58ed0dc250532a479d7789f`
 - `actions/checkout v6.0.1@8e8c483db84b4bee98b60c0593521ed34d9990e8`
 - `actions/setup-python v6.1.0@83679a892e2d95755f2dac6acb0bfd1e9ac5d548`
 - `PyO3/maturin-action v1.49.4@86b9d133d34bc1b40018696f782949dac11bd380`
 - `actions/upload-artifact v6.0.0@b7c566a772e6b6bfb58ed0dc250532a479d7789f`
 - `actions/upload-artifact v6.0.0@b7c566a772e6b6bfb58ed0dc250532a479d7789f`
 - `actions/checkout v6.0.1@8e8c483db84b4bee98b60c0593521ed34d9990e8`
 - `actions/setup-python v6.1.0@83679a892e2d95755f2dac6acb0bfd1e9ac5d548`
 - `PyO3/maturin-action v1.49.4@86b9d133d34bc1b40018696f782949dac11bd380`
 - `actions/upload-artifact v6.0.0@b7c566a772e6b6bfb58ed0dc250532a479d7789f`
 - `actions/upload-artifact v6.0.0@b7c566a772e6b6bfb58ed0dc250532a479d7789f`
 - `actions/checkout v6.0.1@8e8c483db84b4bee98b60c0593521ed34d9990e8`
 - `actions/setup-python v6.1.0@83679a892e2d95755f2dac6acb0bfd1e9ac5d548`
 - `PyO3/maturin-action v1.49.4@86b9d133d34bc1b40018696f782949dac11bd380`
 - `actions/upload-artifact v6.0.0@b7c566a772e6b6bfb58ed0dc250532a479d7789f`
 - `actions/upload-artifact v6.0.0@b7c566a772e6b6bfb58ed0dc250532a479d7789f`
 - `actions/checkout v6.0.1@8e8c483db84b4bee98b60c0593521ed34d9990e8`
 - `actions/setup-python v6.1.0@83679a892e2d95755f2dac6acb0bfd1e9ac5d548`
 - `PyO3/maturin-action v1.49.4@86b9d133d34bc1b40018696f782949dac11bd380`
 - `uraimo/run-on-arch-action v3.0.1@d94c13912ea685de38fccc1109385b83fd79427d`
 - `actions/upload-artifact v6.0.0@b7c566a772e6b6bfb58ed0dc250532a479d7789f`
 - `actions/upload-artifact v6.0.0@b7c566a772e6b6bfb58ed0dc250532a479d7789f`
 - `actions/checkout v6.0.1@8e8c483db84b4bee98b60c0593521ed34d9990e8`
 - `actions/setup-python v6.1.0@83679a892e2d95755f2dac6acb0bfd1e9ac5d548`
 - `PyO3/maturin-action v1.49.4@86b9d133d34bc1b40018696f782949dac11bd380`
 - `addnab/docker-run-action v3@4f65fabd2431ebc8d299f8e5a018d79a769ae185`
 - `actions/upload-artifact v6.0.0@b7c566a772e6b6bfb58ed0dc250532a479d7789f`
 - `actions/upload-artifact v6.0.0@b7c566a772e6b6bfb58ed0dc250532a479d7789f`
 - `actions/checkout v6.0.1@8e8c483db84b4bee98b60c0593521ed34d9990e8`
 - `actions/setup-python v6.1.0@83679a892e2d95755f2dac6acb0bfd1e9ac5d548`
 - `PyO3/maturin-action v1.49.4@86b9d133d34bc1b40018696f782949dac11bd380`
 - `uraimo/run-on-arch-action v3.0.1@d94c13912ea685de38fccc1109385b83fd79427d`
 - `actions/upload-artifact v6.0.0@b7c566a772e6b6bfb58ed0dc250532a479d7789f`
 - `actions/upload-artifact v6.0.0@b7c566a772e6b6bfb58ed0dc250532a479d7789f`

</details>

<details><summary>.github/workflows/build-docker.yml (18)</summary>

 - `actions/checkout v6.0.1@8e8c483db84b4bee98b60c0593521ed34d9990e8`
 - `docker/setup-buildx-action v3.12.0@8d2750c68a42422c14e847fe6c8ac0403b4cbd6f`
 - `docker/login-action v3.6.0@5e57cd118135c172c3672efd75eb46360885c0ef`
 - `docker/metadata-action v5.10.0@c299e40c65443455700f0fdfc63efafe5b349051`
 - `docker/build-push-action v6.18.0@263435318d21b8e681c14492fe198d362a7d2c83`
 - `actions/upload-artifact v6.0.0@b7c566a772e6b6bfb58ed0dc250532a479d7789f`
 - `actions/download-artifact v7.0.0@37930b1c2abaa49bbe596cd826c3c89aef350131`
 - `docker/setup-buildx-action v3.12.0@8d2750c68a42422c14e847fe6c8ac0403b4cbd6f`
 - `docker/metadata-action v5.10.0@c299e40c65443455700f0fdfc63efafe5b349051`
 - `docker/login-action v3.6.0@5e57cd118135c172c3672efd75eb46360885c0ef`
 - `docker/setup-buildx-action v3.12.0@8d2750c68a42422c14e847fe6c8ac0403b4cbd6f`
 - `docker/login-action v3.6.0@5e57cd118135c172c3672efd75eb46360885c0ef`
 - `docker/metadata-action v5.10.0@c299e40c65443455700f0fdfc63efafe5b349051`
 - `docker/build-push-action v6.18.0@263435318d21b8e681c14492fe198d362a7d2c83`
 - `actions/download-artifact v7.0.0@37930b1c2abaa49bbe596cd826c3c89aef350131`
 - `docker/setup-buildx-action v3.12.0@8d2750c68a42422c14e847fe6c8ac0403b4cbd6f`
 - `docker/metadata-action v5.10.0@c299e40c65443455700f0fdfc63efafe5b349051`
 - `docker/login-action v3.6.0@5e57cd118135c172c3672efd75eb46360885c0ef`

</details>

<details><summary>.github/workflows/ci.yaml (12)</summary>

 - `actions/checkout v6.0.1@8e8c483db84b4bee98b60c0593521ed34d9990e8`
 - `actions/setup-python v6.1.0@83679a892e2d95755f2dac6acb0bfd1e9ac5d548`
 - `Swatinem/rust-cache v2.8.2@779680da715d629ac1d338a641029a2f4372abb5`
 - `PyO3/maturin-action v1.49.4@86b9d133d34bc1b40018696f782949dac11bd380`
 - `actions/checkout v6.0.1@8e8c483db84b4bee98b60c0593521ed34d9990e8`
 - `astral-sh/setup-uv v7.1.6@681c641aba71e4a1c380be3ab5e12ad51f415867` → [Updates: `v7.2.0`]
 - `actions/cache v5.0.1@9255dc7a253b0ccc959486e2bca901246202afeb`
 - `actions/checkout v6.0.1@8e8c483db84b4bee98b60c0593521ed34d9990e8`
 - `astral-sh/setup-uv v7.1.6@681c641aba71e4a1c380be3ab5e12ad51f415867` → [Updates: `v7.2.0`]
 - `actions/checkout v6.0.1@8e8c483db84b4bee98b60c0593521ed34d9990e8`
 - `astral-sh/setup-uv v7.1.6@681c641aba71e4a1c380be3ab5e12ad51f415867` → [Updates: `v7.2.0`]
 - `actions/setup-python v6.1.0@83679a892e2d95755f2dac6acb0bfd1e9ac5d548`

</details>

<details><summary>.github/workflows/daily_property_tests.yml (4)</summary>

 - `actions/checkout v6.0.1@8e8c483db84b4bee98b60c0593521ed34d9990e8`
 - `rui314/setup-mold v1@725a8794d15fc7563f59595bd9556495c0564878`
 - `Swatinem/rust-cache v2.8.2@779680da715d629ac1d338a641029a2f4372abb5`
 - `actions/github-script v8.0.0@ed597411d8f924073f98dfc5c65a23a2325f34cd`

</details>

<details><summary>.github/workflows/publish-docs.yml (3)</summary>

 - `actions/checkout v6.0.1@8e8c483db84b4bee98b60c0593521ed34d9990e8`
 - `actions/setup-python v6.1.0@83679a892e2d95755f2dac6acb0bfd1e9ac5d548`
 - `python 3.14`

</details>

<details><summary>.github/workflows/publish-pypi.yml (2)</summary>

 - `astral-sh/setup-uv v7.1.6@681c641aba71e4a1c380be3ab5e12ad51f415867` → [Updates: `v7.2.0`]
 - `actions/download-artifact v7.0.0@37930b1c2abaa49bbe596cd826c3c89aef350131`

</details>

<details><summary>.github/workflows/release.yml (13)</summary>

 - `actions/checkout 8e8c483db84b4bee98b60c0593521ed34d9990e8` → [Updates: `undefined`]
 - `actions/upload-artifact b7c566a772e6b6bfb58ed0dc250532a479d7789f`
 - `actions/upload-artifact b7c566a772e6b6bfb58ed0dc250532a479d7789f`
 - `actions/checkout 8e8c483db84b4bee98b60c0593521ed34d9990e8` → [Updates: `undefined`]
 - `actions/download-artifact 37930b1c2abaa49bbe596cd826c3c89aef350131`
 - `actions/download-artifact 37930b1c2abaa49bbe596cd826c3c89aef350131`
 - `actions/upload-artifact b7c566a772e6b6bfb58ed0dc250532a479d7789f`
 - `actions/checkout 8e8c483db84b4bee98b60c0593521ed34d9990e8` → [Updates: `undefined`]
 - `actions/download-artifact 37930b1c2abaa49bbe596cd826c3c89aef350131`
 - `actions/download-artifact 37930b1c2abaa49bbe596cd826c3c89aef350131`
 - `actions/upload-artifact b7c566a772e6b6bfb58ed0dc250532a479d7789f`
 - `actions/checkout 8e8c483db84b4bee98b60c0593521ed34d9990e8` → [Updates: `undefined`]
 - `actions/download-artifact 37930b1c2abaa49bbe596cd826c3c89aef350131`

</details>

</blockquote>
</details>

<details><summary>pre-commit (1)</summary>
<blockquote>

<details><summary>.pre-commit-config.yaml (13)</summary>

 - `astral-sh/uv-pre-commit 0.9.18` → [Updates: `0.9.21`]
 - `pre-commit/pre-commit-hooks v6.0.0`
 - `astral-sh/ruff-pre-commit v0.14.10` → [Updates: `v0.14.11`]
 - `abravalheri/validate-pyproject v0.24.1`
 - `mdformat-mkdocs ==5.1.1` → [Updates: `==5.1.2`]
 - `mdformat-footnote ==0.1.2`
 - `executablebooks/mdformat 1.0.0`
 - `igorshubovych/markdownlint-cli v0.47.0`
 - `crate-ci/typos v1.40.0` → [Updates: `v1.41.0`]
 - `rbubley/mirrors-prettier v3.7.4`
 - `zizmorcore/zizmor-pre-commit v1.19.0` → [Updates: `v1.20.0`]
 - `python-jsonschema/check-jsonschema 0.36.0`
 - `rhysd/actionlint v1.7.9` → [Updates: `v1.7.10`]

</details>

</blockquote>
</details>

---

- [ ] <!-- manual job -->Check this box to trigger a request for Renovate to run again on this repository



---

_Label `dependencies` added by @MichaReiser on 2025-05-06 09:39_

---

_Label `internal` added by @AlexWaygood on 2025-05-11 07:59_

---

_Added to milestone `Z post-stable` by @carljm on 2025-11-14 14:57_

---

_Removed from milestone `Z post-stable` by @carljm on 2025-11-18 16:10_

---

_Label `dependencies` removed by @AlexWaygood on 2025-12-19 12:07_

---
