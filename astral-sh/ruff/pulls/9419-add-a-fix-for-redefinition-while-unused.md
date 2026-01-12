```yaml
number: 9419
title: "Add a fix for `redefinition-while-unused`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - fixes
  - preview
assignees: []
merged: true
base: main
head: charlie/F811
created_at: 2024-01-07T01:59:38Z
updated_at: 2024-01-09T00:37:06Z
url: https://github.com/astral-sh/ruff/pull/9419
synced_at: 2026-01-12T15:55:28Z
```

# Add a fix for `redefinition-while-unused`

---

_@charliermarsh_

## Summary

This PR enables Ruff to remove redefined imports, as in:

```python
import os
import os

print(os)
```

Previously, Ruff would flag `F811` on the second `import os`, but couldn't fix it.

For now, this fix is marked as safe, but only available in preview.

Closes https://github.com/astral-sh/ruff/issues/3477.


---

_Label `autofix` added by @charliermarsh on 2024-01-07 01:59_

---

_Review requested from @zanieb by @charliermarsh on 2024-01-07 01:59_

---

_Comment by @github-actions[bot] on 2024-01-07 02:12_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+0 -0 violations, +4 -0 fixes in 1 projects; 42 projects unchanged)

<details><summary><a href="https://github.com/aws/aws-sam-cli">aws/aws-sam-cli</a> (+0 -0 violations, +4 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/aws/aws-sam-cli/blob/f51e020f1fe2ef3221c32f2cd4d4f21fa66c292e/tests/unit/commands/deploy/test_deploy_context.py#L10'>tests/unit/commands/deploy/test_deploy_context.py:10:47:</a> F811 Redefinition of unused `DeployFailedError` from line 8
+ <a href='https://github.com/aws/aws-sam-cli/blob/f51e020f1fe2ef3221c32f2cd4d4f21fa66c292e/tests/unit/commands/deploy/test_deploy_context.py#L10'>tests/unit/commands/deploy/test_deploy_context.py:10:47:</a> F811 [*] Redefinition of unused `DeployFailedError` from line 8
- <a href='https://github.com/aws/aws-sam-cli/blob/f51e020f1fe2ef3221c32f2cd4d4f21fa66c292e/tests/unit/local/docker/test_lambda_image.py#L8'>tests/unit/local/docker/test_lambda_image.py:8:27:</a> F811 Redefinition of unused `parameterized` from line 5
+ <a href='https://github.com/aws/aws-sam-cli/blob/f51e020f1fe2ef3221c32f2cd4d4f21fa66c292e/tests/unit/local/docker/test_lambda_image.py#L8'>tests/unit/local/docker/test_lambda_image.py:8:27:</a> F811 [*] Redefinition of unused `parameterized` from line 5
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| F811 | 4 | 0 | 0 | 4 | 0 |

</p>
</details>




---

_@zanieb approved on 2024-01-08 21:59_

---

_Comment by @zanieb on 2024-01-08 21:59_

> I think it can be safe if we merge https://github.com/astral-sh/ruff/pull/9418?

Probably?  Seems safer to start this way.

---

_Comment by @charliermarsh on 2024-01-08 23:35_

@zanieb - I'm not sure how to justify or explain that the fix is unsafe though, now that we removed branching. It should always be safe?

---

_Comment by @zanieb on 2024-01-08 23:52_

@charliermarsh I guess so! I can't think of a case. We could gate the fix with preview for now?

---

_Label `preview` added by @charliermarsh on 2024-01-09 00:23_

---

_Merged by @charliermarsh on 2024-01-09 00:29_

---

_Closed by @charliermarsh on 2024-01-09 00:29_

---

_Branch deleted on 2024-01-09 00:29_

---
