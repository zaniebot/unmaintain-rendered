---
number: 4135
title: CI fails to create PR comment when output is too long
type: issue
state: closed
author: dhruvmanila
labels:
  - bug
assignees: []
created_at: 2023-04-27T17:17:28Z
updated_at: 2023-09-14T19:47:13Z
url: https://github.com/astral-sh/ruff/issues/4135
synced_at: 2026-01-10T01:22:43Z
---

# CI fails to create PR comment when output is too long

---

_Issue opened by @dhruvmanila on 2023-04-27 17:17_

Example: https://github.com/charliermarsh/ruff/actions/runs/4820701572/jobs/8585562172

As per [GitHub documentation](https://docs.github.com/en/actions/using-jobs/defining-outputs-for-jobs), this might be occurring because:

> Outputs are Unicode strings, and can be a maximum of 1 MB. The total of all outputs in a workflow run can be a maximum of 50 MB.

If so, the solution would be to [comment using a `filePath`](https://github.com/thollander/actions-comment-pull-request#comment-a-file-content) instead of passing it to the `message` key.

But then comes the limitation of the comment itself which is 65536 characters (tested it out on my own repo):

```
Error: Validation Failed: {"resource":"IssueComment","code":"custom","field":"body","message":"body is too long (maximum is 65536 characters)"}
```

Now, the solution would be:
1. Move the output to a file (could use `GITHUB_WORKSPACE`)
2. Use the `filePath` to post the content of PR checks
3. Truncate the content to limit it to < 65536 characters and add a link to the summary page


---

_Label `bug` added by @charliermarsh on 2023-04-27 17:56_

---

_Comment by @charliermarsh on 2023-09-14 19:47_

This now gets truncated.

---

_Closed by @charliermarsh on 2023-09-14 19:47_

---
