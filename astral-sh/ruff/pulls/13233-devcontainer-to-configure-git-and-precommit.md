```yaml
number: 13233
title: Devcontainer to configure git and precommit
type: pull_request
state: closed
author: brucearctor
labels:
  - internal
assignees: []
base: main
head: devcontainer-precommit
created_at: 2024-09-03T18:55:31Z
updated_at: 2024-09-05T16:19:40Z
url: https://github.com/astral-sh/ruff/pull/13233
synced_at: 2026-01-10T21:38:32Z
```

# Devcontainer to configure git and precommit

---

_Pull request opened by @brucearctor on 2024-09-03 18:55_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

This change configures git to be able to run from within the devcontainer.  

It also 'installs' pre-commit.

I was following https://docs.astral.sh/ruff/contributing/#prerequisites and realized that it is better to just build on top of current devcontainer infra that is already supported by the community, and have even fewer manual steps.



## Test Plan

This was tested by running locally.


## Naturally SQUASH the commits :-)


---

_Review comment by @dhruvmanila on `.devcontainer/post-create.sh`:14 on 2024-09-05 06:38_

I'm a bit hesitant to enforce pre-commit to contributors inside the dev containers or more generally I'm hesitant to install anything other than the required tools to be able to contribute to Ruff. I, personally, don't install the hooks and run them manually as I might commit a lot.

---

_Review comment by @dhruvmanila on `.devcontainer/post-create.sh`:11 on 2024-09-05 06:49_

Is there a reference to this requirement?

---

_@dhruvmanila reviewed on 2024-09-05 06:49_

---

_Label `internal` added by @dhruvmanila on 2024-09-05 06:49_

---

_@brucearctor reviewed on 2024-09-05 16:18_

---

_Review comment by @brucearctor on `.devcontainer/post-create.sh`:14 on 2024-09-05 16:18_

i guess it ultimately is comes down to the prevailing preference.  I generally prefer about anything I might want to be installed, and I can always disable.  BUT, I understand the alternative, so can close this PR.  

---

_Closed by @brucearctor on 2024-09-05 16:18_

---

_@dhruvmanila reviewed on 2024-09-05 16:19_

---

_Review comment by @dhruvmanila on `.devcontainer/post-create.sh`:14 on 2024-09-05 16:19_

Thank you for understanding :)

---
