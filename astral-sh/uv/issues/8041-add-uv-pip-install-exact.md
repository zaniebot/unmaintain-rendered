```yaml
number: 8041
title: "Add `uv pip install --exact`"
type: issue
state: closed
author: charliermarsh
labels:
  - enhancement
  - help wanted
  - cli
assignees: []
created_at: 2024-10-09T09:33:26Z
updated_at: 2024-10-16T00:10:44Z
url: https://github.com/astral-sh/uv/issues/8041
synced_at: 2026-01-12T15:59:19Z
```

# Add `uv pip install --exact`

---

_@charliermarsh_

To remove extraneous packages, similar to `uv sync --inexact`.

---

_Label `enhancement` added by @charliermarsh on 2024-10-09 09:33_

---

_Label `cli` added by @charliermarsh on 2024-10-09 09:33_

---

_Label `good first issue` added by @charliermarsh on 2024-10-09 09:33_

---

_Label `help wanted` added by @charliermarsh on 2024-10-09 09:33_

---

_Label `good first issue` removed by @charliermarsh on 2024-10-09 09:33_

---

_Closed by @charliermarsh on 2024-10-09 13:31_

---

_Closed by @charliermarsh on 2024-10-09 13:31_

---

_Comment by @ashb on 2024-10-15 12:54_

@charliermarsh I don't think this is 100% working. Or at least it's not working how I expected to be able to use it:

In the apache/airflow repo:

1. `uv venv build/.k8s-env`
2. `VIRTUAL_ENV=$PWD/build/.k8s-env uv pip install --exact -r scripts/ci/kubernetes/k8s_requirements.txt`
3. `VIRTUAL_ENV=$PWD/build/.k8s-env uv pip install kgb` (Or any module not currently installed)
4. Run `VIRTUAL_ENV=$PWD/build/.k8s-env uv pip install --exact -r scripts/ci/kubernetes/k8s_requirements.txt` and expect it to remove the `kgb` module I've just installed manually, but it doesn't.

Sorry, I should have thought to check the PR.

---

_Comment by @charliermarsh on 2024-10-15 13:00_

Thanks Ash, let me give it a try today. It should be working as you expect based on the above.

---

_Comment by @ashb on 2024-10-15 13:03_

If `uv pip sync` is a better command for this workflow I'd be happy to use that, but using it on that same requirments file I end up with a venv with only these three

```
-e file:///Users/ash/code/airflow/airflow
-e file:///Users/ash/code/airflow/airflow/task_sdk
-e file:///Users/ash/code/airflow/airflow/providers
```

And non of the deps or extras.

---

_Comment by @blueraft on 2024-10-15 13:09_

It works as expected for me 🤔

<img width="645" alt="Screenshot 2024-10-15 at 15 09 45" src="https://github.com/user-attachments/assets/6d07f02d-e6cd-4122-a54a-936a2c2125d0">


---

_Comment by @ashb on 2024-10-15 13:24_

Stange, it doesn't for me:

```
❯ VIRTUAL_ENV=$PWD/build/.k8s-env uv pip install kgb
Using Python 3.12.5 environment at build/.k8s-env
Resolved 1 package in 25ms
Installed 1 package in 2ms
 + kgb==7.1.1

❯ VIRTUAL_ENV=$PWD/build/.k8s-env uv pip install --exact -r scripts/ci/kubernetes/k8s_requirements.txt
Using Python 3.12.5 environment at build/.k8s-env
Audited 3 packages in 74ms

❯ uv version 
uv 0.4.21 (0c5d05d9e 2024-10-14)
```

---

_Comment by @charliermarsh on 2024-10-15 13:39_

@ashb -- Do you want to send me the `--verbose` logs from the second command? You can email if you prefer.

---

_Comment by @charliermarsh on 2024-10-15 13:39_

Oh you know what, I can imagine why this might be happening.

---

_Comment by @charliermarsh on 2024-10-15 13:40_

If you pass `--reinstall`, does it do what you expect?

---

_Comment by @blueraft on 2024-10-15 13:41_

I think it's the perf optimisation that checks if the env satisfies the requirements, I can reproduce it now by running the command a few times

---

_Comment by @charliermarsh on 2024-10-15 13:42_

Yeah exactly. We should check the `modifications` flag there and skip it if `--exact` is provided.

---

_Comment by @blueraft on 2024-10-15 13:45_

I'll create a PR in a bit

---

_Comment by @ashb on 2024-10-15 13:53_

Oh yes, passing `--reinstall` does fix it, and also there was def a timing element as you expected. I came back to it and just ran the `install --exact` and that time it did uninstall kgb.

Sounds like you don't need logs, right? 

---

_Comment by @charliermarsh on 2024-10-15 13:54_

Yeah no need -- we'll get it fixed.

---

_Comment by @ashb on 2024-10-16 00:10_

Thanks for the speedy fix! All working now. On step closer to my goal of the complete uv-ification of Airflow's CI!

---
