---
number: 14894
title: Problems with git installs in CI via Github Actions
type: issue
state: open
author: emdneto
labels:
  - bug
assignees: []
created_at: 2025-07-25T12:53:40Z
updated_at: 2025-07-28T17:30:29Z
url: https://github.com/astral-sh/uv/issues/14894
synced_at: 2026-01-10T01:25:50Z
---

# Problems with git installs in CI via Github Actions

---

_Issue opened by @emdneto on 2025-07-25 12:53_

### Summary

In opentelemetry-python, we have a lot of CI jobs that run tests using a specific version of the API/SDK against all instrumentation packages. To make this work, we install API/SDK packages pointing to a git address and pinned to a specific SHA commit, like this: 

```
uv pip install 'opentelemetry-api@ git+https://github.com/open-telemetry/opentelemetry-python.git@0f166e3136033f3230a692f82381fb66f39d417f#egg=opentelemetry-api&subdirectory=opentelemetry-api'
```

The thing is, the installation is failing randomly in our CI with:
```
× Failed to download and build `opentelemetry-api @
  │ git+[https://github.com/open-telemetry/opentelemetry-python.git@0f166e3136033f3230a692f82381fb66f39d417f#egg=opentelemetry-api&subdirectory=opentelemetry-api`](https://github.com/open-telemetry/opentelemetry-python.git@0f166e3136033f3230a692f82381fb66f39d417f#egg=opentelemetry-api&subdirectory=opentelemetry-api%60)
  ├─▶ Git operation failed
  ├─▶ failed to find branch, tag, or commit
  │   `0f166e3136033f3230a692f82381fb66f39d417f`
  ╰─▶ process didn't exit successfully: `/usr/bin/git rev-parse
      '0f166e3136033f3230a692f82381fb66f39d417f^0'` (exit status: 128)
      --- stdout
      0f166e3136033f3230a692f82381fb66f39d417f^0

      --- stderr
      fatal: ambiguous argument '0f166e3136033f3230a692f82381fb66f39d417f^0':
      unknown revision or path not in the working tree.
      Use '--' to separate paths from revisions, like this:
      'git <command> [<revision>...] -- [<file>...]'
```
The weird thing is that the SHA commit exists, and if you try to install it locally, it will work.

This issue occurs with approximately 10-15 CI jobs in every push. 

See example: 
1. https://github.com/open-telemetry/opentelemetry-python/actions/runs/16509062023/job/46686986557?pr=4692
2. https://github.com/open-telemetry/opentelemetry-python/actions/runs/16507371027/job/46681351748?pr=4695


### Platform

Ubuntu 24.04.2 LTS

### Version

latest

### Python version

3.9+

---

_Label `bug` added by @emdneto on 2025-07-25 12:53_

---

_Referenced in [astral-sh/uv#13054](../../astral-sh/uv/issues/13054.md) on 2025-07-25 12:54_

---

_Comment by @zanieb on 2025-07-25 13:02_

Is the issue spurious or consistently reproduced?

---

_Comment by @zanieb on 2025-07-25 13:03_

Do you think you could isolate this into a minimal reproduction in a separate repository?

---

_Referenced in [open-telemetry/opentelemetry-python-contrib#3352](../../open-telemetry/opentelemetry-python-contrib/issues/3352.md) on 2025-07-25 13:03_

---

_Comment by @emdneto on 2025-07-25 13:05_

> Is the issue spurious or consistently reproduced?

It's happening consistently. Like, every PR. If you open any PR in the repo, you'll see that there are Core Contrib Tests failing with that error message.


> Do you think you could isolate this into a minimal reproduction in a separate repository?

I think so. Like, we just need to have an action with:

```
name: Core Contrib Test

on:
  push:
    branches-ignore:
    - 'release/*'
  pull_request:

permissions:
  contents: read

jobs:
  contrib_0:
    uses: open-telemetry/opentelemetry-python-contrib/.github/workflows/core_contrib_test_0.yml@main
    with:
      CORE_REPO_SHA: ${{ github.sha }}
      CONTRIB_REPO_SHA: main
```

where ${{ github.sha }} is a commit sha from opentelemetry-python repo

---

_Comment by @emdneto on 2025-07-25 13:16_

@zanieb Created a minimal repo to reproduce: https://github.com/emdneto/otel-python-ci/actions/runs/16522889749/job/46728711816 

---

_Comment by @zanieb on 2025-07-25 13:23_

The issue only occurs in CI?

---

_Comment by @jomcgi on 2025-07-25 13:26_

> The issue only occurs in CI?

I've not hit it locally but I'm also not running as many concurrent builds locally.

---

_Comment by @emdneto on 2025-07-25 13:30_

> The issue only occurs in CI?

I was never able to reproduce the issue locally, because I don't need to run all tests, but it seems to be a problem only in CI. Locally, I usually point to `main` and it always works. I'll try run everything locally.

---

_Comment by @jomcgi on 2025-07-25 13:32_

Would this benefit from adding controls like this? with the associate retry logic
```diff
    /// The number of retries for HTTP requests. (default: 3)
    pub const UV_HTTP_RETRIES: &'static str = "UV_HTTP_RETRIES";
+
+   /// The number of retries for git operations. (default: 3)
+   pub const UV_GIT_RETRIES: &'static str = "UV_GIT_RETRIES";
```
Happy to share a PR if that makes sense.

---

_Comment by @zanieb on 2025-07-25 15:53_

I'm unsure why a retry would help here. It sounds like the issue isn't spurious?

I can't reproduce with 

```
uv pip install 'opentelemetry-api@ git+https://github.com/open-telemetry/opentelemetry-python.git@c3bef5d6c3ed054cac12bbc037db4f58a8cda30d#egg=opentelemetry-api&subdirectory=opentelemetry-api' 'opentelemetry-sdk@ git+https://github.com/open-telemetry/opentelemetry-python.git@c3bef5d6c3ed054cac12bbc037db4f58a8cda30d#egg=opentelemetry-sdk&subdirectory=opentelemetry-sdk' 'opentelemetry-semantic-conventions@ git+https://github.com/open-telemetry/opentelemetry-python.git@c3bef5d6c3ed054cac12bbc037db4f58a8cda30d#egg=opentelemetry-semantic-conventions&subdirectory=opentelemetry-semantic-conventions' 'opentelemetry-test-utils@ git+https://github.com/open-telemetry/opentelemetry-python.git@c3bef5d6c3ed054cac12bbc037db4f58a8cda30d#egg=opentelemetry-test-utils&subdirectory=tests/opentelemetry-test-utils'
```

locally.

Regarding a minimal reproduction, can you do one without tox and such please? It has to actually be minimized to the core problem for it to help us narrow things down.

---

_Comment by @emdneto on 2025-07-28 14:43_

> I'm unsure why a retry would help here. It sounds like the issue isn't spurious?
> 
> I can't reproduce with
> 
> ```
> uv pip install 'opentelemetry-api@ git+https://github.com/open-telemetry/opentelemetry-python.git@c3bef5d6c3ed054cac12bbc037db4f58a8cda30d#egg=opentelemetry-api&subdirectory=opentelemetry-api' 'opentelemetry-sdk@ git+https://github.com/open-telemetry/opentelemetry-python.git@c3bef5d6c3ed054cac12bbc037db4f58a8cda30d#egg=opentelemetry-sdk&subdirectory=opentelemetry-sdk' 'opentelemetry-semantic-conventions@ git+https://github.com/open-telemetry/opentelemetry-python.git@c3bef5d6c3ed054cac12bbc037db4f58a8cda30d#egg=opentelemetry-semantic-conventions&subdirectory=opentelemetry-semantic-conventions' 'opentelemetry-test-utils@ git+https://github.com/open-telemetry/opentelemetry-python.git@c3bef5d6c3ed054cac12bbc037db4f58a8cda30d#egg=opentelemetry-test-utils&subdirectory=tests/opentelemetry-test-utils'
> ```
> 
> locally.
> 
> Regarding a minimal reproduction, can you do one without tox and such please? It has to actually be minimized to the core problem for it to help us narrow things down.

We use tox to run the tests, which installs uv in the environment and generates the commands to run `uv pip install ...` 

---

_Comment by @zanieb on 2025-07-28 17:30_

Yeah but we'll need an example that's not using `tox` so we can narrow down where the problem is in uv specifically?

---
