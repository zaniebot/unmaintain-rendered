```yaml
number: 9465
title: "Configurable Precedence for `--env-file`"
type: issue
state: open
author: pedroprates
labels:
  - configuration
  - needs-decision
assignees: []
created_at: 2024-11-27T12:21:35Z
updated_at: 2025-10-21T18:34:56Z
url: https://github.com/astral-sh/uv/issues/9465
synced_at: 2026-01-10T03:23:53Z
```

# Configurable Precedence for `--env-file`

---

_Issue opened by @pedroprates on 2024-11-27 12:21_

### Problem Description

Currently, when using the --env-file flag in the uv package, system environment variables take precedence over those defined in the .env file. This behavior can lead to inconsistencies and potential reproducibility issues, especially when the system's environment differs across users or systems, despite having identical .env configurations.

## Proposed Solution

Introduce a new flag (e.g., --env-file-precedence or --force-env-file) to control which source of environment variables takes precedence in case of conflicts. The following options could be considered:

- **Default (current behavior)**: System environment variables override .env file values.
- **New option**: .env file variables take precedence, ensuring uniformity across different environments.

## Benefits

- **Reproducibility**: This change would enhance reproducibility, as the environment would depend solely on the .env file when configured accordingly.
- **Flexibility**: Users can choose the behavior that suits their needs, whether prioritizing system configurations or ensuring .env file consistency.

## Additional Context

This feature would align with best practices seen in tools like [python-dotenv](https://pypi.org/project/python-dotenv/), which prioritize .env files if `override` flag is set to `True`.

---

_Comment by @CardosoGuiVi on 2024-11-27 13:09_

This is a great topic! The current behavior of prioritizing system environment variables over `.env` file values indeed has its merits, but it can create challenges for reproducibility across different setups. Introducing a flag to control this behavior, as suggested, would bring much-needed flexibility.

Here’s an example to illustrate the issue:

1. Setup: I have a `~/.zshrc` file with a `PYTHONPATH` variable set as follows:
```nvim
export PYTHONPATH="/my/path"
```

2. Project `.env` file: My project defines a different `PYTHONPATH` in a `.env` file:
```plain
PYTHONPATH="~/my/specific/path"
```

3. Python script: A simple script `example.py` prints the `PYTHONPATH` value:
```python
import os

print("PYTHONPATH is:", os.getenv("PYTHONPATH"))
```

4. Command to run:
```shell
$ uv run --env-file .env python example.py
```

**Current Output**: Despite the `.env` file specifying `~/my/specific/path`, the script prints:

```shell
PYTHONPATH is: /my/path
```

This is because the system environment variable takes precedence over the `.env` file. While this may be expected for some, it can lead to unintended behavior when developers assume `.env` files are definitive for a project.

**Proposed Solution in Action**: With a flag like `--force-env-file`, I could enforce the `.env` file's value to take precedence, ensuring the output would instead be:

```shell
PYTHONPATH is: ~/my/specific/path
```

What does everyone else think? Would the proposed flag be a good middle ground to cater to different use cases?

---

_Label `configuration` added by @zanieb on 2025-01-07 19:55_

---

_Label `needs-decision` added by @zanieb on 2025-01-07 19:55_

---

_Comment by @zanieb on 2025-01-07 19:57_

Loosely, we're hesitant to add more configuration flags without strong demand since we have so many flags. If there's continual demand for this, we can add it.

---

_Comment by @padraic-padraic on 2025-01-24 09:20_

I'm looking to roll out uv for project management myself, and this behaviour (defaulting to `dotenvy::from_path`) caused some unexpected headaches. I appreciate dotenv isn't a standard, but in general the (my?) expectation is that if an env varaible is defined multiple times, the last declaration wins. Other tools in our toolchain (just, make, docker-compose) all behave this way, making uv the odd one out.

(Though, now looking at this, I do see that the node `dotenv` package defauls to a similar behaviour...)

This is exactly how it works when multiple `--env-file` arguments are passed, but if you instead have a single `.env` file made by catting together several env files "upstream" then uv will only ever use the first variable declaration, and this feels a bit counterituitive!

EDIT: On second thought, these seem to be two different behaviours: Do we override existing env variables vs how do we set the env correctly when parsing a file that has a new variable defined multiple times, and this question seems to be one for the `dotenvy` maintainers who, it looks like, will remedy this behaviour in an upcoming release)


---

_Comment by @baggiponte on 2025-04-16 14:41_

> Loosely, we're hesitant to add more configuration flags without strong demand since we have so many flags. If there's continual demand for this, we can add it.

Makes sense not to want to add a flag. How about a simple configuration section in the `pyproject.toml` table? I found myself explaining this a couple of time already to colleagues switching to uv - this also includes the fact to explicitly call `--env-file=.env` or setting `UV_ENV_FILE` whenever they run a command with `uv`. marimo has a [similar feature](https://docs.marimo.io/guides/configuration/runtime_configuration/?h=.env#env-files).

In my case, it would help me avoid explaining some uv details that most of my colleagues took as a "given" or "internals" they would not like to have to know about when migrating.

---

_Comment by @j4mie on 2025-04-23 13:55_

Just adding a +1 to this. I've just spent half an hour trying to figure out why `uv run` wasn't reading the correct `GITHUB_TOKEN` (for a project that interacts with the GitHub API) from `.env`. It turns out that I have a globally-set (in `.bashrc`) value for `GITHUB_TOKEN`, which is an entirely separate token that I use for a different command-line tool.

My hunch is that values in `.env` should _always_ override values that are already in the environment. The `.env` is a project-local space, and every other tool I've used previously that reads `.env` behaves "correctly" here. I'm aware this is a backwards-incompatible change but I think the current behaviour is confusing and actually might be risky in some circumstances.

---

_Comment by @jerny-stoiclane on 2025-04-29 17:53_

> Loosely, we're hesitant to add more configuration flags without strong demand since we have so many flags. If there's continual demand for this, we can add it.

+1: This seems like its enough of a footgun that not having the option to set it as a cli-flag, env var, or config option is something worth adding.  The strong demand is there and its seems reasonable that if you are supporting a .env-file flag having the ability to configure `override` should also be there.

Appreciate the work on UV

---

_Comment by @honi-at-simspace on 2025-10-21 18:33_

As long as the current behavior is enforced where existing env variables can't be redefined in a .env file, is there any recommended way for us to pass new environment variables to a `uv run` command that are already defined in the outer environment?

---
