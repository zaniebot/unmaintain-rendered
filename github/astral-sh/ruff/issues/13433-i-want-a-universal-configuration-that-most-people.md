---
number: 13433
title: I want a universal configuration that most people are used to.
type: issue
state: closed
author: lqllife
labels:
  - question
assignees: []
created_at: 2024-09-21T08:21:30Z
updated_at: 2024-11-20T12:02:15Z
url: https://github.com/astral-sh/ruff/issues/13433
synced_at: 2026-01-07T13:12:15-06:00
---

# I want a universal configuration that most people are used to.

---

_Issue opened by @lqllife on 2024-09-21 08:21_

I just started using ruff and never used black before, so I want to ask if there are any standard, commonly used and easy-to-use rules.
I searched for a long time in the issue but couldn't find it, so I'm asking.
Please give me some ready-made rules, preferably with some brief descriptions, thank you.
Here are my pyproject.toml configuration.

```toml
[project]
name = "flask_test"
version = "0.1.0"
description = "falsk test"
readme = "README.md"
requires-python = ">=3.10"
dependencies = [
    "flask~=2.3.2",
    ...
]

# 开发环境依赖
[tool.uv]
dev-dependencies = [
    "pytest >=8.1.1,<9",
    "ruff>=0.6.6",
]

# ruff 自动化校验
[tool.ruff]
target-version = "py310"
exclude = [".venv", "*.pyi"]
src = ["app", "tests"]
line-length = 120
indent-width = 4

[tool.ruff.lint]
select = ["E", "F", "UP", "W"]

[tool.ruff.format]
quote-style = "single"
indent-style = "space"
skip-magic-trailing-comma = false
line-ending = "auto"
```

---

_Comment by @augustelalande on 2024-09-23 00:16_

ruff when unconfigured use [this](https://docs.astral.sh/ruff/configuration/#configuring-ruff) as the default configuration so I would say that is a good starting point.

---

_Comment by @lqllife on 2024-09-23 01:42_

> ruff when unconfigured use [this](https://docs.astral.sh/ruff/configuration/#configuring-ruff) as the default configuration so I would say that is a good starting point.

Thank you. I want to know, what is the more reasonable configuration in actual development?

---

_Comment by @augustelalande on 2024-09-23 04:15_

That's a very loaded question, which people debate all the time. I would say the one specified by black, which I believe is also the ruff default is one of the most widely used.

---

_Comment by @lqllife on 2024-09-23 06:29_

> That's a very loaded question, which people debate all the time. I would say the one specified by black, which I believe is also the ruff default is one of the most widely used.

It's true, could you please share your configuration?

---

_Comment by @AndreuCodina on 2024-09-23 06:34_

I always use this configuration: https://github.com/astral-sh/ruff/issues/11415

---

_Label `question` added by @dhruvmanila on 2024-10-04 04:53_

---

_Comment by @Avasam on 2024-10-11 01:49_

With tools like these, I start with everything (`select = ["ALL"]`) then remove  stuff as I find issues or disagree with their reasoning. I also get to learn a lot by reading their doc as they come up in my code. Then end up with a config looking something like this: https://github.com/BesLogic/shared-configs/blob/main/ruff.toml

https://github.com/astral-sh/ruff/issues/12352 / https://github.com/astral-sh/ruff/discussions/3363#discussioncomment-7266932 would be nice to make it easy to share a config once you settle on something you like.


---

_Closed by @MichaReiser on 2024-11-20 12:02_

---
