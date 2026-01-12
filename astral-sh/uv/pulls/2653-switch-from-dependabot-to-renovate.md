```yaml
number: 2653
title: Switch from dependabot to renovate
type: pull_request
state: merged
author: AlexWaygood
labels: []
assignees: []
merged: true
base: main
head: renovate-switch
created_at: 2024-03-25T18:53:42Z
updated_at: 2024-03-25T19:26:58Z
url: https://github.com/astral-sh/uv/pull/2653
synced_at: 2026-01-12T16:05:09Z
```

# Switch from dependabot to renovate

---

_@AlexWaygood_

## Summary

This PR removes our dependabot configuration, and replaces it with renovate. The motivations are:
- Renovate will also update our pre-commit dependencies
- It's good to keep this kind of configuration in sync between the two repositories, and we just switched Ruff over in https://github.com/astral-sh/ruff/pull/10567
- Renovate supports the `update-lockfile` strategy for Cargo dependencies, and dependabot [does not](https://github.com/dependabot/dependabot-core/issues/4009).

The only differences to the configuration we added to Ruff in https://github.com/astral-sh/ruff/pull/10567 are that I haven't selected the `npm` or `pep621` package managers for the `uv` repo. We don't seem to have any npm or PEP-621 dependencies in this repo right now (unlike in Ruff), and the PEP-621 manager in particular looks like it might have some false positives in the `scripts/` directory, which has lots of benchmark-related `pyproject.toml` files with dependencies that we probably don't really want to update.

## Test Plan

<!-- How was it tested? -->

I used renovate's CLI tool (which you install via npm) to validate the configuration locally:

```sh
(renovate-switch) % npx --yes --package renovate -- renovate-config-validator                  ~/dev/uv
(node:1829) [DEP0040] DeprecationWarning: The `punycode` module is deprecated. Please use a userland alternative instead.
(Use `node --trace-deprecation ...` to show where the warning was created)
 INFO: Validating .github/renovate.json5
 INFO: Config validated successfully
```

Once this PR is merged and the bot has been granted authorisation, it will either open an issue similar to https://github.com/astral-sh/ruff/issues/10578 (if the configuration is all okay) or an issue similar to https://github.com/python/typeshed/issues/11586 (if there are errors in the configuration).

---

_Review requested from @zanieb by @AlexWaygood on 2024-03-25 18:53_

---

_Assigned to @zanieb by @AlexWaygood on 2024-03-25 18:53_

---

_Unassigned @zanieb by @AlexWaygood on 2024-03-25 18:54_

---

_Comment by @zanieb on 2024-03-25 19:04_

We _could_ use PEP 621 for https://github.com/astral-sh/uv/blob/main/scripts/scenarios/requirements.in and https://github.com/astral-sh/uv/blob/main/scripts/release/requirements.in if we wanted

---

_@zanieb approved on 2024-03-25 19:13_

---

_Comment by @AlexWaygood on 2024-03-25 19:26_

> We _could_ use PEP 621 for [`main`/scripts/scenarios/requirements.in](https://github.com/astral-sh/uv/blob/main/scripts/scenarios/requirements.in?rgh-link-date=2024-03-25T19%3A04%3A10Z) and [`main`/scripts/release/requirements.in](https://github.com/astral-sh/uv/blob/main/scripts/release/requirements.in?rgh-link-date=2024-03-25T19%3A04%3A10Z) if we wanted

The PEP-621 manager is just for `pyproject.toml` files.

If we wanted those files to be autoupdated as they currently are, I think either we'd use the [`pip_requirements` manager](https://docs.renovatebot.com/modules/manager/pip_requirements/), in which case it would update the `requirements.in` file but not the `requirements.txt` file (not really what we want), or we'd use the [`pip-compile` manager](https://docs.renovatebot.com/modules/manager/pip-compile/), which would mean that renovate would generate the `requirements.txt` file from the `requirements.in` file using `pip-compile`. That's closer to what we want, but it feels kinda funny to have `uv` depend on `pip-compile` internally, even if it's only via renovate 😄. I'm also a big proponent of us eating our own dogfood where possible.

---

_Merged by @AlexWaygood on 2024-03-25 19:26_

---

_Closed by @AlexWaygood on 2024-03-25 19:26_

---

_Branch deleted on 2024-03-25 19:26_

---
