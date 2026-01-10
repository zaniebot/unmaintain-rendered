```yaml
number: 10567
title: Switch from dependabot to renovate
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ci
assignees: []
merged: true
base: main
head: setup-renovate
created_at: 2024-03-25T12:33:52Z
updated_at: 2024-03-25T17:30:12Z
url: https://github.com/astral-sh/ruff/pull/10567
synced_at: 2026-01-10T22:47:02Z
```

# Switch from dependabot to renovate

---

_Pull request opened by @AlexWaygood on 2024-03-25 12:33_

## Summary

This PR sets up a renovate config for updating our dependencies, and removes our dependabot config. Advantages of renovate over dependabot are:
- It will also update pre-commit dependencies for us
- It's highly configurable

Disadvantages are:
- It's highly configurable (so it's more difficult to wade through the docs to figure out exactly what configuration you want)

Note that this PR is necessary but not sufficient for enabling renovate on this repository, if we decide that this is the way we want to go. [Renovate's Github.com app](https://docs.renovatebot.com/getting-started/installing-onboarding/#hosted-githubcom-app) would also need to be enabled for this repository.

## Configuration details

I've used renovate before in https://github.com/AlexWaygood/typeshed-stats and https://github.com/python/typeshed, so I'm pretty confident that this configuration file is basically correct. I've used a `.json5` file rather than a `.json` file so that we can have comments in the file. (`json5` is a [supported file format](https://docs.renovatebot.com/configuration-options/) for renovate configuration.) Some notes on the specific configuration options I've chosen:
- Currently dependabot does not update our Python dependencies in `python/` and `scripts/`, nor our npm dependencies in `playground/`. I've added those in this PR (renovate will look for PEP-621 dependencies only in the `python/` and `scripts/` dependencies, and will look for npm dependencies only in the `playground/` directory), but I'm happy to take them out again if that's something we're not interested in.
- All update PRs will come on Monday mornings, same as with our current dependabot setup
- GitHub Actions updates will all be grouped into a single PR, same as our current dependabot setup. I've also grouped all our pre-commit dependencies into a single weekly PR, but haven't grouped any other dependencies together. I could do others as well, if we want them bunched up a bit more.
- The pre-commit manager dependency manager needs to be explicitly opted into because the pre-commit maintainers prefer you to use the pre-commit.ci bot to autoupdate your dependencies: https://github.com/renovatebot/renovate/issues/11166#issuecomment-896236031. I don't think that's something we particularly need to worry about, however.

## Test Plan

If the Renovate bot is enabled, it will validate this configuration file and open an issue for us if there are any errors.

---

_Review requested from @zanieb by @AlexWaygood on 2024-03-25 12:34_

---

_Comment by @AlexWaygood on 2024-03-25 12:35_

(This PR is a competing PR to https://github.com/astral-sh/ruff/pull/8411 -- we should either do that PR or this one, but not both.)

---

_Label `ci` added by @MichaReiser on 2024-03-25 13:59_

---

_@MichaReiser approved on 2024-03-25 14:00_

Looks good to me. Is there a way to test this or do we have to wait for next Monday?

---

_Comment by @AlexWaygood on 2024-03-25 14:03_

> Is there a way to test this or do we have to wait for next Monday?

If we merge this PR and enable the renovate bot, the bot will immediately do one of two things:
- Open an issue telling us there's an error in our configuration. The issue would look like this: https://github.com/python/typeshed/issues/11586
- Or, if there are no errors in our configuration, it will open a "dependency dashboard" issue that we will be able to use for tracking our scheduled dependency updates. (We can opt out of this, but I personally find it quite useful.) We can use the dependency-dashboard issue to manually trigger an update PR before Monday to try things out. The issue would look like this: https://github.com/python/typeshed/issues/11588

So we should get pretty quick feedback on any issues here if this is merged!

---

_Comment by @MichaReiser on 2024-03-25 14:11_

Sounds good. Thanks. Let's give @zanieb some time to take a look at this PR. They set up the dependantbot configuration initially. 

---

_@zanieb reviewed on 2024-03-25 14:13_

---

_Review comment by @zanieb on `.github/renovate.json5`:33 on 2024-03-25 14:13_

Can we still doing things like `@<bot> ignore this version`?

---

_@zanieb reviewed on 2024-03-25 14:13_

---

_Review comment by @zanieb on `.github/renovate.json5`:38 on 2024-03-25 14:13_

Can we only match the upload/download artifact group? e.g.

https://github.com/astral-sh/uv/blob/main/.github/dependabot.yml#L9-L12

---

_Review comment by @AlexWaygood on `.github/renovate.json5`:38 on 2024-03-25 14:14_

Yes! Sorry, I missed that.

---

_@AlexWaygood reviewed on 2024-03-25 14:14_

---

_@zanieb reviewed on 2024-03-25 14:15_

---

_Review comment by @zanieb on `.github/renovate.json5`:1 on 2024-03-25 14:15_

Do they not support YAML?

---

_@zanieb approved on 2024-03-25 14:16_

Thanks for doing this!

---

_@AlexWaygood reviewed on 2024-03-25 14:27_

---

_Review comment by @AlexWaygood on `.github/renovate.json5`:1 on 2024-03-25 14:27_

No -- we have the choice of JSON, JSON5, or `.renovaterc`: https://docs.renovatebot.com/configuration-options/

---

_@zanieb reviewed on 2024-03-25 14:30_

---

_Review comment by @zanieb on `.github/renovate.json5`:1 on 2024-03-25 14:30_

Bleh okay :)

---

_@zanieb reviewed on 2024-03-25 14:30_

---

_Review comment by @zanieb on `.github/renovate.json5`:38 on 2024-03-25 14:30_

It's different over here — good opportunity to fix.

---

_@AlexWaygood reviewed on 2024-03-25 14:54_

---

_Review comment by @AlexWaygood on `.github/renovate.json5`:33 on 2024-03-25 14:54_

I don't think you can `@` renovate in the same way you can with dependabot. It's possible to add config to this file specifying that it should permanently ignore certain versions (or version ranges), however. Additionally, if you close a renovate PR, it will generally assume that you don't want to see any future PRs updating the dependency to that version. Docs here: https://docs.renovatebot.com/key-concepts/pull-requests/#normal-prs

---

_Comment by @AlexWaygood on 2024-03-25 14:56_

I installed the `renovate` CLI tool (via npm) to validate this config, and it passed:

```sh
(setup-renovate)⚡ % npx --yes --package renovate -- renovate-config-validator               ~/dev/ruff
(node:98623) [DEP0040] DeprecationWarning: The `punycode` module is deprecated. Please use a userland alternative instead.
(Use `node --trace-deprecation ...` to show where the warning was created)
 INFO: Validating .github/renovate.json5
 INFO: Config validated successfully
```

---

_@AlexWaygood reviewed on 2024-03-25 16:23_

---

_Review comment by @AlexWaygood on `.github/renovate.json5`:38 on 2024-03-25 16:23_

Oh, so right now:
- Ruff:
  - doesn't do any updates for `actions/upload-artifact` and `actions/download-artifact`: https://github.com/astral-sh/ruff/blob/f7aab5ac69f9505a1776a4bcf20ea2415aa5e442/.github/dependabot.yml#L12-L15
  - all other GitHub Actions updates are bundled into a single weekly PR: https://github.com/astral-sh/ruff/blob/f7aab5ac69f9505a1776a4bcf20ea2415aa5e442/.github/dependabot.yml#L8-L11
- uv:
  - bundles `actions/upload-artifact` and `actions/artifact` updates into a single weekly PR: https://github.com/astral-sh/uv/blob/7a5571fa5c4fa72a26b0754f3273f1d75f3fa7d2/.github/dependabot.yml#L9-L12
  - all other GitHub Actions updates are done in individual PRs on a weekly basis

What do we want the new behaviour for Ruff to be?

---

_@MichaReiser reviewed on 2024-03-25 16:33_

---

_Review comment by @MichaReiser on `.github/renovate.json5`:38 on 2024-03-25 16:33_

I forgot to remove the actions from the dependabot configuration after doing the major version upgrade. I think upgrading them together (when both versions are bumped) does make sense, which is what UV does

---

_@AlexWaygood reviewed on 2024-03-25 16:44_

---

_Review comment by @AlexWaygood on `.github/renovate.json5`:38 on 2024-03-25 16:44_

Do we want:
1. All actions updates bundled together in one weekly PR?
2. The upload/download actions bundled into one weekly PR, but an individual weekly PR each for all other actions updates?
3. The upload/download actions bundled into one weekly PR, and all other actions updates bundled into a second weekly PR? 

---

_@zanieb reviewed on 2024-03-25 16:47_

---

_Review comment by @zanieb on `.github/renovate.json5`:38 on 2024-03-25 16:47_

2.

---

_Merged by @AlexWaygood on 2024-03-25 17:30_

---

_Closed by @AlexWaygood on 2024-03-25 17:30_

---

_Branch deleted on 2024-03-25 17:30_

---
