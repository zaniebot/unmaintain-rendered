```yaml
number: 1921
title: Increase or adjust scope of default HTTP timeout
type: issue
state: closed
author: zanieb
labels:
  - bug
  - external
assignees: []
created_at: 2024-02-23T17:07:09Z
updated_at: 2024-04-19T20:49:55Z
url: https://github.com/astral-sh/uv/issues/1921
synced_at: 2026-01-12T15:58:33Z
```

# Increase or adjust scope of default HTTP timeout

---

_@zanieb_

Our current HTTP timeout is not sufficient for some large packages e.g. `torch`

- #1920 
- #1912 

Increasing the timeout has a downside for people that have network problems while downloading smaller packages, e.g. it will wait longer before surfacing a problem.

What does pip use for a default timeout?

---

_Label `enhancement` added by @zanieb on 2024-02-23 17:07_

---

_Label `needs-decision` added by @zanieb on 2024-02-23 17:07_

---

_Comment by @MichaReiser on 2024-02-23 17:09_

I would expect that a server only sends an HTTP Timeout response if the connection is idle for too long, which makes me wonder if increasing the timeout solves the problem or only hides the root cause. 

---

_Comment by @zanieb on 2024-02-23 17:10_

Yeah good point, I wonder if we're setting the wrong timeout? We shouldn't be enforcing this timeout when data transfer is actively occurring.

---

_Label `enhancement` removed by @zanieb on 2024-02-23 17:10_

---

_Renamed from "Increase default HTTP timeout" to "Increase or adjust scope of default HTTP timeout" by @zanieb on 2024-02-23 17:15_

---

_Comment by @samypr100 on 2024-02-24 19:20_

FWIW the default in PIP is 15 seconds https://github.com/pypa/pip/blob/main/src/pip/_internal/cli/cmdoptions.py#L294, so I'm not sure uv's 30? second default is at fault.

---

_Comment by @konstin on 2024-02-26 09:13_

Could it be that we need to reduce the number of parallel requests? It could also be that they timeout because some cpu intensive task is blocking the main thread, esp. with something large like pytorch.

---

_Comment by @zanieb on 2024-02-26 23:05_

It seems like the next steps are to:

1. Determine what exactly the timeout we are configuring applies to
2. Create a simple MRE
3. Determine what is happening with the request when the error is raised

---

_Comment by @zanieb on 2024-04-05 22:15_

See https://github.com/seanmonstar/reqwest/issues/2237

It looks like we're using a deadline for the full request not a read timeout as desired. We need this functionality to be added upstream.

---

_Comment by @zanieb on 2024-04-12 19:59_

See https://github.com/seanmonstar/reqwest/pull/2241

---

_Label `needs-decision` removed by @zanieb on 2024-04-14 16:20_

---

_Label `bug` added by @zanieb on 2024-04-14 16:20_

---

_Label `upstream` added by @zanieb on 2024-04-14 16:20_

---

_Comment by @mariosasko on 2024-04-16 00:33_

Hi @zanieb! When can we expect this to be fixed? 


`huggingface/datasets` CI is quite consistently failing because of this issue (I'd assume) as of yesterday (e.g., check the "Install dependencies" step of [this](https://github.com/huggingface/datasets/actions/runs/8692786216/job/23838173446) CI run).

---

_Comment by @charliermarsh on 2024-04-16 00:36_

@mariosasko - How often are you seeing this? We'll ship the HTTP timeout change soon (this week for sure), but I'm still wondering if there's something else going on here and would use `huggingface/datasets` to investigate if it's at least somewhat frequent.

---

_Comment by @mariosasko on 2024-04-16 01:04_

@charliermarsh It's pretty consistent, e.g., see the first two commit's CI runs in https://github.com/huggingface/datasets/pull/6811 (it's more likely to happen than not based on recent CI runs). 

PS: Not sure if this info is valuable, but both Windows and Ubuntu GH runners are susceptible to this issue

---

_Comment by @charliermarsh on 2024-04-16 01:16_

OK thanks. I'm gonna do some testing using that CI workflow.

---

_Comment by @charliermarsh on 2024-04-16 16:24_

So the first issue I ran into is that Windows sometimes fails with pytest missing, because the "Install dependencies" step silently fails:

```
error: Failed to download: scikit-learn==0.23.1
  Caused by: HTTP status server error (503 Service Unavailable) for url (https://files.pythonhosted.org/packages/7e/e5/888491b7e2c16718a68dfd8498325e8927003410b2d19ba255d875[13](https://github.com/charliermarsh/datasets/actions/runs/8708243419/job/23885167918?pr=1#step:7:14)38a5/scikit_learn-0.23.1-cp38-cp38-win_amd64.whl.metadata)
Resolved 4 packages in 41ms
Downloaded 4 packages in 2.54s
Installed 4 packages in 28ms
 + bleurt==0.0.2 (from git+https://github.com/google-research/bleurt.git@cebe7e6f996b40910cfaa520a63db47807e3bf5c)
 + coval==0.0.1 (from git+https://github.com/ns-moosavi/coval.git@87071a6293dc2e786dcfe2ed78e9369c[17](https://github.com/charliermarsh/datasets/actions/runs/8708243419/job/23885167918?pr=1#step:7:18)e41b3b)
 + math-equivalence==0.0.0 (from git+https://github.com/hendrycks/math.git@357963a7f5501a6c1708cf3f3fb0cdf525642761)
 + unbabel-comet==2.2.2
```

We can make that _not_ silent by changing the shell configuration (so that it exits as soon as one command fails, rather than proceeding). But I'm not sure why it's failing in the first place. 503 hitting that static endpoint?


---

_Closed by @charliermarsh on 2024-04-19 20:49_

---
