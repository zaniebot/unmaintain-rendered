```yaml
number: 7144
title: "\"no wheels with a matching Python implementation tag\" with inequality constraint, but no issues with equality constraint"
type: issue
state: closed
author: colobas
labels:
  - question
assignees: []
created_at: 2024-09-06T22:20:18Z
updated_at: 2024-09-07T00:56:12Z
url: https://github.com/astral-sh/uv/issues/7144
synced_at: 2026-01-12T15:59:10Z
```

# "no wheels with a matching Python implementation tag" with inequality constraint, but no issues with equality constraint

---

_@colobas_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->

## Intro

Trying to support a package which should install different implementations of PyTorch and related packages based on the value of `UV_EXTRA_INDEX_URL`, but am having issues with their cpu index (`https://download.pytorch.org/whl/cpu`). Namely it seems that `torchX.X.X+cpu` as a dependency of `torchvision` doesn't get recognized unless I specify a strict version

Platform (`uname -sri`): `Linux 5.14.0-362.13.1.el9_3.x86_64 x86_64`
uv version: `uv 0.3.1`

## Steps to reproduce 

### Prep
- Create venv using `uv venv`: `$ uv venv -p python3.10`
- Activate it `souce .venv/bin/activate`

### Failure mode 1

- Try installing `torchvision>=0.18.0` with the aforementioned index url:
  - `$ uv pip install --index-url https://download.pytorch.org/whl/cpu "torchvision>=0.18.0"`
- <details><summary>Get the following error (click to unfold)</summary>

  ```
  × No solution found when resolving dependencies:
    ╰─▶ Because only the following versions of torchvision are available:
            torchvision<=0.18.0
            torchvision==0.18.0+cpu
            torchvision==0.18.1
            torchvision==0.18.1+cpu
            torchvision==0.19.0
            torchvision==0.19.0+cpu
            torchvision==0.19.1
            torchvision==0.19.1+cpu
        and torchvision==0.18.0 has no wheels with a matching Python implementation tag, we can conclude that torchvision>=0.18.0,<0.18.0+cpu cannot be used. (1)
  
        Because torch==2.3.0 has no wheels with a matching Python implementation tag and torchvision==0.18.0+cpu depends on torch==2.3.0, we can conclude that torchvision==0.18.0+cpu cannot
        be used.
        And because we know from (1) that torchvision>=0.18.0,<0.18.0+cpu cannot be used, we can conclude that torchvision>=0.18.0,<0.18.1 cannot be used.
        And because torchvision==0.18.1 has no wheels with a matching Python implementation tag, we can conclude that torchvision>=0.18.0,<0.18.1+cpu cannot be used. (2)
  
        Because torch==2.3.1 has no wheels with a matching Python implementation tag and torchvision==0.18.1+cpu depends on torch==2.3.1, we can conclude that torchvision==0.18.1+cpu cannot
        be used.
        And because we know from (2) that torchvision>=0.18.0,<0.18.1+cpu cannot be used, we can conclude that torchvision>=0.18.0,<0.19.0 cannot be used.
        And because torchvision==0.19.0 has no wheels with a matching Python implementation tag, we can conclude that torchvision>=0.18.0,<0.19.0+cpu cannot be used. (3)
  
        Because torch==2.4.0 has no wheels with a matching Python implementation tag and torchvision==0.19.0+cpu depends on torch==2.4.0, we can conclude that torchvision==0.19.0+cpu cannot
        be used.
        And because we know from (3) that torchvision>=0.18.0,<0.19.0+cpu cannot be used, we can conclude that torchvision>=0.18.0,<0.19.1 cannot be used.
        And because torchvision==0.19.1 has no wheels with a matching Python implementation tag, we can conclude that torchvision>=0.18.0,<0.19.1+cpu cannot be used. (4)
  
        Because torch==2.4.1 has no wheels with a matching Python implementation tag and torchvision==0.19.1+cpu depends on torch==2.4.1, we can conclude that torchvision==0.19.1+cpu cannot
        be used.
        And because we know from (4) that torchvision>=0.18.0,<0.19.1+cpu cannot be used, we can conclude that torchvision>=0.18.0 cannot be used.
        And because you require torchvision>=0.18.0, we can conclude that your requirements are unsatisfiable.
    ```
    </details>

### Failure mode 2

- Try installing `torchvision==0.18.0+cpu` with the aforementioned index url:
  - `$ uv pip install --index-url https://download.pytorch.org/whl/cpu "torchvision==0.18.0+cpu"`
- <details><summary>Get the following error (click to unfold)</summary>

  ```
  × No solution found when resolving dependencies:
  ╰─▶ Because torch==2.3.0 has no wheels with a matching Python implementation tag and torchvision==0.18.0+cpu depends on torch==2.3.0, we can conclude that torchvision==0.18.0+cpu cannot
      be used.
      And because you require torchvision==0.18.0+cpu, we can conclude that your requirements are unsatisfiable.
  ```
 
### Failure mode 3

- Try installing `torchvision==0.18.0` with the aforementioned index url:
  - `$ uv pip install --index-url https://download.pytorch.org/whl/cpu "torchvision==0.18.0"`
- <details><summary>Get the following error (click to unfold)</summary>

  ```
  × No solution found when resolving dependencies:
  ╰─▶ Because torchvision==0.18.0 has no wheels with a matching Python implementation tag and you require torchvision==0.18.0, we can conclude that your requirements are unsatisfiable.
  ```

### Pseudo-success case

- Install `torchvision==0.18.0+cpu` and `torch==2.3.0+cpu` explicitly:
  - `$ uv pip install --index-url https://download.pytorch.org/whl/cpu "torchvision==0.18.0+cpu" "torch==2.3.0+cpu"`
- Installation succeeds.

## Conclusion

Not quite sure how to interpret what's happening other than:

- It seems that `torchvision==0.18.0+cpu` wants `torch==2.3.0` as a dependency (failure mode 1 & 2)
- ...But it doesn't recognize `torch==2.3.0+cpu` as supplying `torch==2.3.0`?... (failure mode 2)
- ...Unless I install it explicitly? (pseudo-success case)

Not sure what to make of failure mode 3

---

_Comment by @colobas on 2024-09-06 22:46_

Seems related to #1497 

---

_Comment by @charliermarsh on 2024-09-06 23:09_

Have you read https://docs.astral.sh/uv/pip/compatibility/#local-version-identifiers?

---

_Label `question` added by @charliermarsh on 2024-09-06 23:10_

---

_Comment by @colobas on 2024-09-07 00:46_

Thanks for the quick reply!

I have now read the link you sent.
Apologies if this issue is adding to the noise. I'm working around this with the `--override` flag for now.
Feel free to close this if there's no plan for things to change in the near future.

Cheers

---

_Comment by @charliermarsh on 2024-09-07 00:56_

No worries at all! I'd like to improve it long-term but probably not in the immediate future :)

---

_Closed by @charliermarsh on 2024-09-07 00:56_

---
