```yaml
number: 14938
title: Use double quotes consistently for shell scripts
type: pull_request
state: merged
author: dhruvmanila
labels:
  - release
assignees: []
merged: true
base: main
head: dhruv/double-quote
created_at: 2024-12-12T14:36:52Z
updated_at: 2024-12-13T10:39:57Z
url: https://github.com/astral-sh/ruff/pull/14938
synced_at: 2026-01-10T20:42:27Z
```

# Use double quotes consistently for shell scripts

---

_Pull request opened by @dhruvmanila on 2024-12-12 14:36_

## Summary

The release failed (https://github.com/astral-sh/ruff/actions/runs/12298190472/job/34321509636) because the shell script in the Docker release workflow was using single quotes instead of double quotes.

This is related to https://www.shellcheck.net/wiki/SC2016. I found it via [`actionlint`](https://github.com/rhysd/actionlint). Related #14893.

I also went ahead and fixed https://www.shellcheck.net/wiki/SC2086 which were raised in a couple of places.


---

_Label `release` added by @dhruvmanila on 2024-12-12 14:36_

---

_Review requested from @dylwil3 by @dhruvmanila on 2024-12-12 14:37_

---

_@dhruvmanila reviewed on 2024-12-12 14:37_

---

_Review comment by @dhruvmanila on `.github/workflows/build-docker.yml`:147 on 2024-12-12 14:37_

This is where the release failure happened.

---

_@dhruvmanila reviewed on 2024-12-12 14:37_

---

_Review comment by @dhruvmanila on `.github/workflows/build-docker.yml`:292 on 2024-12-12 14:37_

And here as well.

---

_@MichaReiser reviewed on 2024-12-12 14:40_

---

_Review comment by @MichaReiser on `.github/workflows/release.yml`:271 on 2024-12-12 14:40_

Isn't this file auto generated?

---

_Review comment by @dcreager on `.github/workflows/build-docker.yml`:146 on 2024-12-12 14:40_

Are the nested double quotes here not an issue because they're inside of a `$( )` construct?  I would have naively assumed you'd need to backslash-escape them.

---

_@dcreager reviewed on 2024-12-12 14:40_

---

_@AlexWaygood reviewed on 2024-12-12 14:41_

---

_Review comment by @AlexWaygood on `.github/workflows/release.yml`:271 on 2024-12-12 14:41_

it is

---

_@dhruvmanila reviewed on 2024-12-12 14:42_

---

_Review comment by @dhruvmanila on `.github/workflows/release.yml`:271 on 2024-12-12 14:42_

Yeah, right. Not sure why one is quoted while the other isn't. I'll revert.

---

_@dhruvmanila reviewed on 2024-12-12 14:44_

---

_Review comment by @dhruvmanila on `.github/workflows/build-docker.yml`:146 on 2024-12-12 14:44_

I think no because `$( )` should start a new context but I think in this specific case I should revert because of the glob.

---

_Merged by @dylwil3 on 2024-12-12 14:45_

---

_Closed by @dylwil3 on 2024-12-12 14:45_

---

_Branch deleted on 2024-12-12 14:45_

---

_Comment by @AlexWaygood on 2024-12-12 14:46_

Is it worth adding shellcheck to our pre-commit config so we can get these issues flagged in CI in the future? https://github.com/koalaman/shellcheck?tab=readme-ov-file#pre-commit

---

_Comment by @AlexWaygood on 2024-12-12 14:51_

Thanks for fixing -- sorry for the breakage :(

---

_Comment by @dhruvmanila on 2024-12-13 10:39_

> Is it worth adding shellcheck to our pre-commit config so we can get these issues flagged in CI in the future? [koalaman/shellcheck#pre-commit](https://github.com/koalaman/shellcheck?tab=readme-ov-file#pre-commit)

I don't know if shellcheck can parse workflow files but `actionlint` has a feature where it runs shellcheck on the shell scripts in a workflow files. I think it's worth adding it for that specific feature.

---
