```yaml
number: 7888
title: Add shellcheck CI
type: pull_request
state: merged
author: sobolevn
labels:
  - internal
assignees: []
merged: true
base: main
head: add-shellcheck
created_at: 2024-10-03T07:52:41Z
updated_at: 2024-10-08T18:58:05Z
url: https://github.com/astral-sh/uv/pull/7888
synced_at: 2026-01-10T12:53:58Z
```

# Add shellcheck CI

---

_Pull request opened by @sobolevn on 2024-10-03 07:52_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

I started learning `uv` by inspecting the source code.
I've noticed that your shell scripts are very good! Which is rare!

## Test Plan

I propose to add `shellcheck` to the CI.
It is a great tool to help finding bugs and style issues in shell code.

Techincal details:
- This CI job will only run when any `.sh` files are changed (or the job definition file)
- It takes just several seconds even on local machine:
```
» time shellcheck -S style **/*.sh
shellcheck -S style **/*.sh  0.02s user 0.05s system 61% cpu 0.123 total
```

- It is easy to use, for example: I just fixed the single problem you had in your code with `# shellcheck disable=SC1091`
- I am using this tool for around 8 years now and didn't have any issues. Examples: https://github.com/sobolevn/git-secret/blob/ca899f3b694187f9027161f537414422ee99c483/.github/workflows/test.yml#L22-L27 and https://github.com/wemake-services/wemake-django-template/blob/master/.github/workflows/shellcheck.yml

But, I understand that build / lint tools are very subjective. So, feel free to close :)

---

_Comment by @zanieb on 2024-10-03 14:36_

I _do_ really like shellcheck but given it didn't find any actual problems here I'm not sure if it's worth adding to CI? 

If we do add it, it'd probably make sense at https://github.com/astral-sh/uv/blob/14507a1793f12bf884786be49d716ca8f8322340/.github/workflows/ci.yml#L47 with our other lint jobs.

---

_Comment by @sobolevn on 2024-10-03 16:39_

@zanieb this is exactly right time to add a tool, when there are no complaints! 😄 
Because in other cases - it would be a much harder case, since it would require a refactoring of existing problems.

I added this a separate file, because I can specify to only run this on `*.sh` files to save some extra computation, when regular PRs don't touch shell scripts.

---

_Comment by @zanieb on 2024-10-03 16:43_

How long does it take to run on the full repository? Seconds right? I'm not sure it's worth the time saving — spinning up GitHub Actions runners has significant overhead.

---

_Comment by @sobolevn on 2024-10-03 17:07_

Locally it takes `0.02s user 0.05s system 61% cpu 0.123 total`, sure, will change to work in `lint:` job 👍 

---

_@mkniewallner reviewed on 2024-10-03 18:07_

It would probably be nice to pin a specific `shellcheck` version to use, per https://github.com/ludeeus/action-shellcheck?tab=readme-ov-file#run-a-specific-version-of-shellcheck. Otherwise if new versions that update or add rules are published, CI could break out of the blue.

By enabling https://docs.renovatebot.com/presets-customManagers/#custommanagersgithubactionsversions preset [here](https://github.com/astral-sh/uv/blob/312eeb8f573d36f6df658f85ecadc52799647bb3/.github/renovate.json5#L5) and setting something like:
```yaml
env:
  # renovate: datasource=github-tags depName=koalaman/shellcheck
  SHELLCHECK_VERSION: "v0.10.0"
```
in the GitHub Actions workflow, it should even be possible to have Renovate regularly update `shellcheck` version.

---

_Comment by @sobolevn on 2024-10-03 19:33_

Awesome! I didn't know that!

---

_Review comment by @mkniewallner on `.github/workflows/ci.yml`:90 on 2024-10-03 20:21_

```suggestion
        with:
          version: ${{ env.SHELLCHECK_VERSION }}
```
otherwise the env var itself will not do anything.

---

_@mkniewallner reviewed on 2024-10-03 20:21_

---

_@mkniewallner approved on 2024-10-03 20:27_

---

_@zanieb approved on 2024-10-08 18:57_

---

_Merged by @zanieb on 2024-10-08 18:58_

---

_Closed by @zanieb on 2024-10-08 18:58_

---

_Label `internal` added by @zanieb on 2024-10-08 18:58_

---
