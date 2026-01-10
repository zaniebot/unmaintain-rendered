```yaml
number: 6467
title: "`uv run` with stdin"
type: issue
state: closed
author: billy-doyle
labels:
  - enhancement
  - help wanted
assignees: []
created_at: 2024-08-22T22:37:30Z
updated_at: 2024-08-23T15:49:57Z
url: https://github.com/astral-sh/uv/issues/6467
synced_at: 2026-01-10T04:45:09Z
```

# `uv run` with stdin

---

_Issue opened by @billy-doyle on 2024-08-22 22:37_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->

inspired by https://docs.astral.sh/uv/guides/scripts/#declaring-script-dependencies and some tweets being able to just send someone a command and boom they can run a py script

MRE:
```bash
billy@sa ~/uv_eof % uv run - <<EOF
print("hello world")
EOF
error: Failed to spawn: `-`
  Caused by: No such file or directory (os error 2)
```

Short dash(-) here represents stdin, similar to how k8s allows:

```bash
kubectl apply -f - <<EOF
apiVersion: v1
kind: ServiceAccount
metadata:
  name: kubectl-actions
  namespace: my-v2-restore
EOF
```

Some more details and how kubectl does it (in go tho ofc) https://stackoverflow.com/a/72168173/9992341 and https://github.com/kubernetes/cli-runtime/blob/v0.21.0/pkg/resource/builder.go#L245

basically turns any snippet someone else wants to run with 

```bash
billy@sa ~/uv_eof % curl -LsSf https://astral.sh/uv/install.sh | sh && uv run - <<EOF               
print("hello world")
EOF
```

---

_Comment by @zanieb on 2024-08-22 22:42_

Yeah, we can support this.

---

_Label `enhancement` added by @zanieb on 2024-08-22 22:42_

---

_Label `help wanted` added by @zanieb on 2024-08-22 22:42_

---

_Comment by @charliermarsh on 2024-08-22 22:48_

This should be not-too-difficult to add if anyone is looking for a contribution :)

---

_Assigned to @charliermarsh by @charliermarsh on 2024-08-23 00:08_

---

_Comment by @charliermarsh on 2024-08-23 00:08_

I'm gonna do this as a break from hard issues!

---

_Closed by @charliermarsh on 2024-08-23 15:49_

---
