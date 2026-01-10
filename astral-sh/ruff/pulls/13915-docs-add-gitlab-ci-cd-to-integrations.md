```yaml
number: 13915
title: "Docs: Add GitLab CI/CD to integrations."
type: pull_request
state: merged
author: jvacek
labels:
  - documentation
assignees: []
merged: true
base: main
head: docs_gitlab-cicd
created_at: 2024-10-24T17:21:38Z
updated_at: 2024-10-27T11:58:33Z
url: https://github.com/astral-sh/ruff/pull/13915
synced_at: 2026-01-10T20:59:37Z
```

# Docs: Add GitLab CI/CD to integrations.

---

_Pull request opened by @jvacek on 2024-10-24 17:21_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary
Adds a sample cicd config for gitlab
<!-- What's the purpose of the change? What does it do, and why? -->

## Test Plan

<!-- How was it tested? -->


---

_Comment by @zanieb on 2024-10-24 17:48_

Related https://docs.astral.sh/uv/guides/integration/gitlab/

---

_@zanieb reviewed on 2024-10-24 17:48_

---

_Review comment by @zanieb on `docs/integrations.md`:176 on 2024-10-24 17:48_

Why the verbose flag on `format`?

---

_@jvacek reviewed on 2024-10-24 18:30_

---

_Review comment by @jvacek on `docs/integrations.md`:176 on 2024-10-24 18:30_

Ah yeah that's probably not necessary indeed, I do like to see which configs are being taken into account if I'm puzzled by a fail
```suggestion
    - ruff format --diff
```

---

_Comment by @jvacek on 2024-10-24 18:41_

Not sure if it's preferrable to split this up into two separate jobs so they can run in parallel? Guess the users can figure that out for themselves in my opinion.

---

_Label `documentation` added by @MichaReiser on 2024-10-25 07:12_

---

_Review comment by @MichaReiser on `docs/integrations.md`:160 on 2024-10-25 07:13_

Should we move this right after GitHub Actions to have all CI in one place?

---

_Review comment by @MichaReiser on `docs/integrations.md`:169 on 2024-10-25 07:15_

What's the motivation of using the docker image over using e.g. pip or uv pip to install the project's ruff version? 
That would be more aligned with our GitHub Actions example

---

_Review comment by @MichaReiser on `docs/integrations.md`:166 on 2024-10-25 07:17_

Should we use the `build` stage to ensure the job runs even if it is the only job?

> If a pipeline contains only jobs in the .pre or .post stages, it does not run. There must be at least one other job in a different stage. ([source](https://docs.gitlab.com/ee/ci/yaml/index.html#stage-pre))



---

_@MichaReiser reviewed on 2024-10-25 07:18_

Nice, thank you!

I haven't used GitLab in ages. So I'm sorry for any obvious questions. 

---

_@jvacek reviewed on 2024-10-25 08:20_

---

_Review comment by @jvacek on `docs/integrations.md`:166 on 2024-10-25 08:20_

Yeah fair, not a great idea to assume _anything_ about the end-users setups hah

---

_@jvacek reviewed on 2024-10-25 08:21_

---

_Review comment by @jvacek on `docs/integrations.md`:169 on 2024-10-25 08:21_

It's _way_ faster than installing ruff with `pip` every single time. That's one of the major advantages of the new images with extra tags you guys push.

---

_@jvacek reviewed on 2024-10-25 08:21_

---

_Review comment by @jvacek on `docs/integrations.md`:160 on 2024-10-25 08:21_

If you prefer to, sure

---

_@jvacek reviewed on 2024-10-25 08:42_

> Not sure if it's preferrable to split this up into two separate jobs so they can run in parallel? Guess the users can figure that out for themselves in my opinion. 

I split this up, let me know if you'd prefer the other

---

_@MichaReiser approved on 2024-10-26 16:10_

This looks good to me. 

Thanks a lot for contributing the workflow template!

---

_Merged by @MichaReiser on 2024-10-26 16:10_

---

_Closed by @MichaReiser on 2024-10-26 16:10_

---

_@MichaReiser reviewed on 2024-10-26 16:10_

---

_Review comment by @MichaReiser on `docs/integrations.md`:169 on 2024-10-26 16:10_

That makes sense. It does have the downside that they have to update the ruff version number locally in the project dependencies and CI. But I guess we can always add a second example if needed.

---

_Branch deleted on 2024-10-27 11:58_

---
