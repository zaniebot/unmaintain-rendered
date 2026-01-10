```yaml
number: 13129
title: "Request for `--root` Support for Multi-Stage Docker Builds"
type: issue
state: open
author: vivodi
labels:
  - needs-mre
assignees: []
created_at: 2025-04-27T13:05:33Z
updated_at: 2025-09-17T05:49:08Z
url: https://github.com/astral-sh/uv/issues/13129
synced_at: 2026-01-10T03:23:54Z
```

# Request for `--root` Support for Multi-Stage Docker Builds

---

_Issue opened by @vivodi on 2025-04-27 13:05_

### Summary

### Feature Request

Currently, `uv` does not support an equivalent to `pip install --root`, which would allow users to install packages with a specific root directory, ensuring they are placed in the appropriate system-wide path.

### Why is this important?

In Docker **multi-stage builds**, it is *impossible* to use `UV_PROJECT_ENVIRONMENT=/usr/local` to **install packages to the system-wide path across stages**.  
A `uv sync --root` option would allow for efficient package installation into a system-wide directory, ensuring the packages are available for use in later build stages without relying on virtual environments.

### Suggested Solution

It would be helpful to have a `--root` parameter (e.g., `uv sync --root`) to install packages directly into a specific directory structure like `/usr/local/lib/python3.x/site-packages/`.

### Example Usage

```bash
uv sync --root /tmp/custom-root
```

This would install the packages into `/tmp/custom-root/usr/local/lib/python3.x/site-packages/`.

### Dockerfile Example 
```dockerfile
FROM python:3.12-alpine
RUN pip install flexget --root /root-dir    # something like `uv sync --root /root-dir` not supported

FROM python:3.12-alpine
COPY --from=0 /root-dir /
```

---

_Label `enhancement` added by @vivodi on 2025-04-27 13:05_

---

_Comment by @konstin on 2025-04-28 12:46_

Can you use `--system` and copy the relevant system directories?

For `--root`, can you describe how it is different from a venv and why you prefer it over using a venv?

---

_Closed by @konstin on 2025-04-28 12:46_

---

_Comment by @vivodi on 2025-05-01 11:14_

> Can you use `--system` and copy the relevant system directories?

First, `uv sync` does not support the `--system` argument; only `uv pip install` supports it. Second, using the `--system` argument and then copying the relevant system directories will copy other unrelated files and Python packages, whereas `--root` only includes files relevant to the current command, ensuring that no unrelated files are included.

> For `--root`, can you describe how it is different from a venv and why you prefer it over using a venv?

The `--root` option is used to install packages to the system-wide path, rather than installing them into a virtual environment (venv). Installing them into a venv would unnecessarily increase space usage, and for a single-function Docker image, installing them into a venv doesn't make sense because it adds unnecessary complexity and overhead, especially when the image is intended to perform a specific task efficiently without managing isolated environments.

Additionally, installing into a venv would prevent our users from installing additional packages in the entrypoint, because the packages installed in the entrypoint would not be present in the venv.

If you still don't understand the necessity of the `--root` option, think about why pip provides this option.



---

_Reopened by @konstin on 2025-05-02 09:13_

---

_Comment by @konstin on 2025-05-02 09:13_

(i didn't mean to close this)

---

_Comment by @konstin on 2025-05-02 09:16_

There are setups using `UV_PROJECT_ENVIRONMENT` with their multi-stage Dockerfiles (https://github.com/search?q=path%3A**%2FDockerfile*+UV_PROJECT_ENVIRONMENT&type=code), what makes it impossible for you to use this option?

---

_Label `needs-mre` added by @charliermarsh on 2025-05-04 12:40_

---

_Label `enhancement` removed by @charliermarsh on 2025-05-04 12:40_

---

_Comment by @vivodi on 2025-09-09 19:21_

> There are setups using UV_PROJECT_ENVIRONMENT with their multi-stage Dockerfiles (https://github.com/search?q=path%3A**%2FDockerfile*+UV_PROJECT_ENVIRONMENT&type=code), what makes it impossible for you to use this option?

Using `uv sync` with `UV_PROJECT_ENVIRONMENT` in multi-stage Dockerfiles is problematic because it creates a virtual environment structure, including unnecessary files like `pyvenv.cfg` and activate scripts. This is not ideal for building minimal Docker images, which only require a single, global package installation.

The `pip install --root <directory>` command is better suited for this. It installs packages into a clean staging directory that mirrors the final system path, perfect for being copied into the next Docker stage (`COPY --from=0 /stagingdir /`). This method avoids any venv-related overhead.

Currently, `uv sync` lacks a direct equivalent to this behavior.

**Feature Proposal:** Add a `--root` option to `uv`, such as **`uv sync --root <directory>`**, to directly emulate `pip install --root`. This would allow developers to leverage `uv`'s speed to create clean, relocatable package directories, greatly simplifying the creation of lean, production-ready Docker images.

---

_Comment by @vivodi on 2025-09-09 19:26_

In short, `pip install --root` has two advantages over using `uv sync` with `UV_PROJECT_ENVIRONMENT`:

1.  It doesn't generate superfluous files that unnecessarily increase the Docker image size.
2.  Its output can be easily copied into the next Docker stage to serve as system-wide site packages, whereas the approach with `UV_PROJECT_ENVIRONMENT` requires manual path handling.

---

_Comment by @vivodi on 2025-09-16 13:48_

> I don't think the virtual environment itself will add much size to your artifact, but understand the desire for zero ambiguity :)
> 
> Setting `UV_PROJECT_ENVIRONMENT` to the system environment should also work, without the `export` and `pip` hacks — no need to have uv around after that.
>
> _Originally posted by @zanieb in https://github.com/astral-sh/uv/issues/8085#issuecomment-2707864546_

@konstin

I think @zanieb’s comment also illustrates that currently, uv cannot avoid creating a virtual environment in a non-system environment, which would help reduce artifact size.

Installing into the system environment in multi-stage Docker builds doesn’t work either, because the system environment may contain other unnecessary packages that you don’t want to copy into the final stage Docker image.






---

_Comment by @konstin on 2025-09-16 14:57_

For what it's worth, venvs are tiny. A freshly created Python 3.13 on my Ubuntu 24.04 machine is 88KB.

---

_Comment by @vivodi on 2025-09-16 15:20_

Although a venv is not large, in principle it is unnecessary to use a venv in a single-purpose container.

Moreover, implementing an equivalent of `pip install --root <directory>` as `uv sync --root <directory>` is not complicated.

Therefore, there is no need to compromise on this issue merely because a venv is small.


---

_Comment by @zanieb on 2025-09-16 19:16_

I think you're underestimating the complexity of maintaining support for something like `--root`.

---
