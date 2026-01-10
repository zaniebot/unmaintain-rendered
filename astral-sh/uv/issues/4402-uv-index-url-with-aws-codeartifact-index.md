```yaml
number: 4402
title: "`UV_INDEX_URL` with AWS CodeArtifact index consistently fails on large packages"
type: issue
state: closed
author: ringohoffman
labels:
  - bug
  - network
assignees: []
created_at: 2024-06-18T20:22:01Z
updated_at: 2024-11-20T00:08:01Z
url: https://github.com/astral-sh/uv/issues/4402
synced_at: 2026-01-10T04:36:19Z
```

# `UV_INDEX_URL` with AWS CodeArtifact index consistently fails on large packages

---

_Issue opened by @ringohoffman on 2024-06-18 20:22_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->

I have a project with a ton of dependencies, and an AWS CodeArtifact PyPI server that I host my dependencies in. When I use it as my index URL, I am consistently getting failures as compared to when I do not.


Related:

* https://github.com/astral-sh/uv/issues/1426
* https://github.com/astral-sh/uv/issues/2586
* https://github.com/astral-sh/uv/issues/3514

**`requirements.txt`**:

```python
nvidia-cublas-cu12==12.1.3.1
nvidia-cuda-cupti-cu12==12.1.105
nvidia-cuda-nvrtc-cu12==12.1.105
nvidia-cuda-runtime-cu12==12.1.105
nvidia-cudnn-cu12==8.9.2.26
nvidia-cufft-cu12==11.0.2.54
nvidia-curand-cu12==10.3.2.106
nvidia-cusolver-cu12==11.4.5.107
nvidia-cusparse-cu12==12.1.0.106
nvidia-nccl-cu12==2.20.5
nvidia-nvjitlink-cu12==12.5.40
nvidia-nvtx-cu12==12.1.105
```
The above packages are the main offenders. Notably, they are all large. To pick a few (the ones that appear in the error messages):

* [`nvidia-nccl-cu12==2.20.5` is 176.2 MB](https://pypi.org/project/nvidia-nccl-cu12/2.20.5/#files)
* [`nvidia-cusparse-cu12==12.1.0.106` is 196.0 MB](https://pypi.org/project/nvidia-cusparse-cu12/12.1.0.106/#files)
* [`nvidia-cusolver-cu12==11.4.5.107` is 124.2 MB](https://pypi.org/project/nvidia-cusolver-cu12/11.4.5.107/#files)

To reproduce:

```console
$ rm -rf ~/.cache/uv
$ conda create -n uvtest python=3.10 -y
$ conda activate uvtest
$ pip install uv
...
Installing collected packages: uv
Successfully installed uv-0.2.13
$ aws sso login
$ ...
$ export UV_INDEX_URL="https://aws:${CODEARTIFACT_AUTH_TOKEN}@${CODEARTIFACT_DOMAIN}-${AWS_ACCOUNT_ID}.d.codeartifact.${AWS_PIP_REGION}.amazonaws.com/pypi/${CODEARTIFACT_REPO}"
$ export UV_HTTP_TIMEOUT=60
$ uv pip install -r requirements.txt
...
error: Failed to download distributions
  Caused by: Failed to fetch wheel: nvidia-nccl-cu12==2.20.5
  Caused by: Failed to extract archive
  Caused by: error decoding response body
  Caused by: request or response body error
  Caused by: error reading a body from connection
  Caused by: end of file before message length reached
```

a different time:

```console
$ uv pip install -r requirements.txt
error: Failed to download distributions
  Caused by: Failed to fetch wheel: nvidia-cusparse-cu12==12.1.0.106
  Caused by: Failed to extract archive
  Caused by: error decoding response body
  Caused by: request or response body error
  Caused by: error reading a body from connection
  Caused by: peer closed connection without sending TLS close_notify: https://docs.rs/rustls/latest/rustls/manual/_03_howto/index.html#unexpected-eof
```

a different time:

```console
$ uv pip install -r requirements.txt
error: Failed to download distributions
  Caused by: Failed to fetch wheel: nvidia-cusolver-cu12==11.4.5.107
  Caused by: Failed to extract archive
  Caused by: error decoding response body
  Caused by: request or response body error
  Caused by: error reading a body from connection
  Caused by: peer closed connection without sending TLS close_notify: https://docs.rs/rustls/latest/rustls/manual/_03_howto/index.html#unexpected-eof
```

I do **not** see these errors when running with `--verbose`??


---

_Label `bug` added by @zanieb on 2024-06-18 20:56_

---

_Label `network` added by @zanieb on 2024-06-18 20:56_

---

_Comment by @ThermodynamicBeta on 2024-11-19 23:03_

I got this today for a random big package, installed from a self-hosted Pypi. It would error on random dependencies from the big package, same error message: `Caused by: peer closed connection without sending TLS close_notify: https://docs.rs/rustls/latest/rustls/manual/_03_howto/index.html#unexpected-eof`

---

_Comment by @charliermarsh on 2024-11-19 23:23_

Lemme see if the retries I just added cover this case.

---

_Comment by @charliermarsh on 2024-11-19 23:26_

Okay I believe this is a form of `UnexpectedEof` so the error itself should be covered:

https://github.com/rustls/rustls/blob/20de56876d8bc45224c351339337c61126c1c954/rustls/src/conn.rs#L185

I'll test it.

---

_Comment by @ThermodynamicBeta on 2024-11-19 23:27_

@charliermarsh I love you

---

_Comment by @charliermarsh on 2024-11-20 00:07_

Ok based on my testing this should be solved by #9253. If you see it after the next release, ping note it here and I'll re-open.

---

_Closed by @charliermarsh on 2024-11-20 00:08_

---
