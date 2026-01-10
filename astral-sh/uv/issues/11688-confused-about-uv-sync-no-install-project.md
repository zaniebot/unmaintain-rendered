```yaml
number: 11688
title: Confused about uv sync --no-install-project
type: issue
state: closed
author: dimaqq
labels:
  - question
assignees: []
created_at: 2025-02-21T05:36:01Z
updated_at: 2025-02-23T00:14:17Z
url: https://github.com/astral-sh/uv/issues/11688
synced_at: 2026-01-10T03:50:31Z
```

# Confused about uv sync --no-install-project

---

_Issue opened by @dimaqq on 2025-02-21 05:36_

### Question

```
Update the project's environment

Usage: uv sync [OPTIONS]

Options:

...

      --no-install-project
          Do not install the current project
```

However, when I run `uv sync` without arguments, it doesn't install the current project anyway. I need to manually `uv pip install [-e] .` to get what I want.

So what is the option for then?

P.S. am I holding it wrong?

### Platform

macos, linux

### Version

uv 0.6.2


### P.S.

In one project where I use pyo3/maturin, `uv sync` does install the current project.

In other projects that are pure Python, it doesn't.

---

_Label `question` added by @dimaqq on 2025-02-21 05:36_

---

_Comment by @Gankra on 2025-02-21 18:03_

Are any of the problem projects publicly viewable? Also do any of the ones that don't work have a build-backend enabled?

---

_Comment by @dimaqq on 2025-02-22 04:41_

Visible: dimaqq/otlp-json 


Build backend… ehhh I think I used the concise defaults that `uv init` generated for me, so I guess not?

---

_Comment by @charliermarsh on 2025-02-22 17:21_

If you omit a build backend, we treat the project as "virtual" and don't install it in the first place. So `--no-install-project` wouldn't affect it. (If you use `uv init --lib`, we _will_ include a build backend, so you'd then notice the difference.)

---

_Comment by @charliermarsh on 2025-02-22 17:21_

See: https://docs.astral.sh/uv/concepts/projects/config/#build-systems

---

_Closed by @charliermarsh on 2025-02-22 17:21_

---

_Assigned to @charliermarsh by @charliermarsh on 2025-02-22 17:22_

---

_Comment by @dimaqq on 2025-02-23 00:14_

Thank you, very good to know!

I wonder how to best expose this… I suppose there’s no way to know if the project is an app or a lib at `uv init` time, is there?

Perhaps `ub knit` console output, or better yet pry project.toml generated could have a note about this?

I also wonder if it would help to add the inverse flag, `--install-project`? Somehow having to “shell out” to uv pip install seems clunky.

---
