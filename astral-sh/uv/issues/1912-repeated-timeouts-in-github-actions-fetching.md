```yaml
number: 1912
title: Repeated timeouts in GitHub Actions fetching wheel for large packages
type: issue
state: closed
author: adamtheturtle
labels:
  - bug
assignees: []
created_at: 2024-02-23T13:11:04Z
updated_at: 2024-05-01T16:33:32Z
url: https://github.com/astral-sh/uv/issues/1912
synced_at: 2026-01-10T05:31:36Z
```

# Repeated timeouts in GitHub Actions fetching wheel for large packages

---

_Issue opened by @adamtheturtle on 2024-02-23 13:11_

In the last few days since switching to `uv`, I have seen errors that I have not seen before with `pip`.

I see:

```
error: Failed to download distributions
  Caused by: Failed to fetch wheel: torch==2.2.1
  Caused by: Failed to extract source distribution
  Caused by: request or response body error: operation timed out
  Caused by: operation timed out
Error: Process completed with exit code 2.
```

I see this on the CI for [`vws-python-mock`](https://github.com/VWS-Python/vws-python-mock), which requires installing 150 packages:

```
uv pip install --upgrade --editable .[dev]
...
Resolved 150 packages in 1.65s
Downloaded 141 packages in 21.41s
Installed 150 packages in 283ms
```

I do this in parallel across many jobs on GitHub Actions, mostly on `ubuntu-latest`.

This happened with `torch 2.2.0` before the recent release of `torch 2.2.1`.
It has not happened with any other dependencies.
The wheels for `torch` are pretty huge: https://pypi.org/project/torch/#files.

`uv` is always at the latest version as I run `curl -LsSf https://astral.sh/uv/install.sh | sh`. In the most recent example, this is `uv 0.1.9`.

Failures:

* https://github.com/VWS-Python/vws-python-mock/actions/runs/8017894929/job/21902693117
* https://github.com/VWS-Python/vws-python-mock/actions/runs/8000017693/job/21848747452
* https://github.com/VWS-Python/vws-python-mock/actions/runs/8000017693/job/21848749557


---

_Comment by @adamtheturtle on 2024-02-23 13:15_

Perhaps I just need to use `UV_HTTP_TIMEOUT` and I will, but I thought that this might be worth pointing out:

* If so, the error message could helpfully point to `UV_HTTP_TIMEOUT`
* Perhaps the default is too small if using GitHub Actions + a popular package times out

---

_Label `question` added by @zanieb on 2024-02-23 17:05_

---

_Comment by @zanieb on 2024-02-23 17:08_

Thanks for the feedback, I've opened issues for your requests

- https://github.com/astral-sh/uv/issues/1921
- https://github.com/astral-sh/uv/issues/1922

---

_Comment by @adamtheturtle on 2024-02-23 17:20_

Thank you @zanieb ! I don't know the value of having this issue open, but I'll leave it to you to close if desired.

---

_Comment by @zanieb on 2024-02-23 17:28_

In #1921 my co-worker noted that this might be a bug in the way we're specifying the timeout so I'll recategorize this one and leave it open.

---

_Label `question` removed by @zanieb on 2024-02-23 17:28_

---

_Label `bug` added by @zanieb on 2024-02-23 17:28_

---

_Assigned to @konstin by @konstin on 2024-02-28 13:02_

---

_Comment by @konstin on 2024-02-28 14:23_

Looking at the actions runs, all the passing actions take ~30s, while the failing ones error after 5min, which is our default timeout, so this looks like a network failure (in either github actions or rust)

---

_Comment by @konstin on 2024-03-01 11:37_

I'm not seeing any timeouts anymore with the two most recent versions (https://github.com/konstin/vws-python-mock/actions). Could you check if this now solved?

---

_Comment by @adamtheturtle on 2024-03-01 11:38_

I have not seen this issue since posting. Thank you for looking into this.

---

_Comment by @konstin on 2024-03-01 11:40_

I'll close it for now, please feel free to reopen should it reoccur

---

_Closed by @konstin on 2024-03-01 11:40_

---

_Comment by @adamtheturtle on 2024-03-04 01:35_

@konstin I do not have permissions to re-open this issue. I can create a new one, but it is probably easier if you re-open this.

This failure has reoccurred:

* https://github.com/VWS-Python/vws-python-mock/actions/runs/8134588970/job/22227737289
* https://github.com/VWS-Python/vws-python-mock/actions/runs/8134588970/job/22227737596

---

_Reopened by @konstin on 2024-03-04 08:38_

---

_Unassigned @konstin by @konstin on 2024-03-04 18:13_

---

_Comment by @hmc-cs-mdrissi on 2024-03-05 03:23_

I'm seeing very similar error message for non pytorch package that's also pretty large. It's ~400 MB wheel and consistently gives me,

```
(bento_uv2) pa-loaner@C02DVAQNMD6R training-platform % uv pip install --index-url=$REGISTRY_INDEX data-mesh-cli==0.0.66
error: Failed to download: data-mesh-cli==0.0.66
  Caused by: The wheel data_mesh_cli-0.0.66-py3-none-any.whl is not a valid zip file
  Caused by: an upstream reader returned an error: request or response body error: operation timed out
  Caused by: request or response body error: operation timed out
  Caused by: operation timed out
```

Package is company internal one though, but I think only notable thing is very large size (it vendors spark/java stuff).

edit: Pytorch weirdly installs fine for me pretty fast.

---

_Renamed from "Repeated timeouts in GitHub Actions fetching wheel for `torch`" to "Repeated timeouts in GitHub Actions fetching wheel for large packages" by @adamtheturtle on 2024-03-13 09:38_

---

_Comment by @adamtheturtle on 2024-03-13 09:39_

I have changed the title of this to not reference `torch`. It recently happened with `nvidia-cudnn-cu12`, another large download.

As another example, https://github.com/VWS-Python/vws-python-mock/actions/runs/8262236134 has 7 failures in one run.

---

_Comment by @astrojuanlu on 2024-03-18 20:13_

It can happen on Read the Docs as well, not only GHA https://beta.readthedocs.org/projects/kedro-datasets/builds/23790543/

---

_Comment by @astrojuanlu on 2024-03-24 06:27_

Spotted it locally today inside a local Docker image running under QEMU

```
error: Failed to download distributions
  Caused by: Failed to fetch wheel: nvidia-cublas-cu12==12.1.3.1
  Caused by: Failed to extract archive
  Caused by: Failed to download distribution due to network timeout. Try increasing UV_HTTP_TIMEOUT (current value: 300s).
```

---

_Comment by @njzjz on 2024-04-19 22:12_

I encountered the problem when I used either uv or pip to download large wheels (for pip, the issue is https://github.com/pypa/pip/issues/4796 and https://github.com/pypa/pip/issues/11153), so I think the root cause is the network. However, I am wondering if uv can be smarter to retry automatically, like something in https://github.com/pypa/pip/pull/11180.

---

_Comment by @astrojuanlu on 2024-04-20 06:56_

Worth trying 0.1.35, which includes https://github.com/astral-sh/uv/pull/3144

---

_Comment by @zanieb on 2024-04-21 15:32_

It seems likely that this is resolved by #3144 

---

_Comment by @OneCyrus on 2024-04-25 20:20_

> I encountered the problem when I used either uv or pip to download large wheels (for pip, the issue is https://github.com/pypa/pip/issues/4796 and https://github.com/pypa/pip/issues/11153), so I think the root cause is the network. However, I am wondering if uv can be smarter to retry automatically, like something in https://github.com/pypa/pip/pull/11180.

that would be a great feature. we have our dev environments behind TLS inspection and some packages often run into a timeout due too slow inspection. we can reproduce this with a browser and the download gets stuck until a timeout. in the browser we can just click resume and the browser reconnects snd downloads the remaining part. with uv we don't have a retry with resume. so it starts from scratch and gets stuck again.

+1 for retry with resume

---

_Comment by @charliermarsh on 2024-05-01 16:33_

Going to close for now, but we can re-open if this comes up again post-changing the timeout semantics.

---

_Closed by @charliermarsh on 2024-05-01 16:33_

---
