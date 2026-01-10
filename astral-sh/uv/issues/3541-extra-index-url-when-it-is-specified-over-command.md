---
number: 3541
title: Extra index url when it is specified over command line and uv.toml
type: issue
state: closed
author: jmspereira
labels:
  - cli
assignees: []
created_at: 2024-05-13T10:50:17Z
updated_at: 2024-05-21T17:30:33Z
url: https://github.com/astral-sh/uv/issues/3541
synced_at: 2026-01-10T01:23:29Z
---

# Extra index url when it is specified over command line and uv.toml

---

_Issue opened by @jmspereira on 2024-05-13 10:50_

Hey everyone,

I have a setup where I call uv pip install with an extra-index-url specified as a parameter, for instance:

uv pip install <package> --extra-index-url <extra-index-url>

But during CI I want to overwrite the value of that flag, for that, I am using the uv. tom file where I have specified:

[pip]
extra-index-url = [<extra-index-url2>]

So the final setup is the uv.toml file and uv being called as mentioned previously (i.e., having 2 extra index URL).
However, it appears that uv is not looking the one specified in the configuration (contrary to what pip does). Is this supposed to happen? Or am I doing something wrong?

Best regards,
- Jorge


---

_Comment by @charliermarsh on 2024-05-13 13:56_

Are you _also_ providing the `--extra-index-url` on the command-line in CI? So in CI, do you have _both_ the command-line argumnet _and_ the `uv.toml` file present?

---

_Label `question` added by @charliermarsh on 2024-05-13 13:56_

---

_Comment by @jmspereira on 2024-05-13 16:04_

Correct, in CI I have both the command line argument and the uv.toml present

---

_Comment by @charliermarsh on 2024-05-13 16:17_

Ah, ok. Command-line arguments always take precedence over configuration files, so yes, this is the expected behavior. I think you'll need to find a way to avoid passing the command-line argument in CI.

---

_Comment by @jmspereira on 2024-05-13 16:29_

is there any reason to implement that behavior? That is not the behavior of pip when there are configurations specified as global in pip.conf

---

_Comment by @charliermarsh on 2024-05-13 16:32_

It's standard for the command line to override environment variables, and for environment variables to override configuration files. Even pip implements this (at least as-written here): https://pip.pypa.io/en/stable/topics/configuration/#precedence-override-order.

---

_Comment by @jmspereira on 2024-05-13 16:57_

Ok, I looked into it and the difference in behavior is that in the case of pip, for the concrete case of the extra-index-url (since it can take multiple values), the value passed in the command line is merged with the value specified in the config file (i.e., if you have 1 extra index url specified in pip.conf and another in the command line, in the end, the two are used). 

---

_Comment by @charliermarsh on 2024-05-13 17:00_

Oh, you want _both_ index URLs to be respected? I assumed from the original issue that you were trying to _disable_ the index URL from the command-line given `extra-index-url = []`.

---

_Comment by @jmspereira on 2024-05-13 17:02_

Sorry If I was not explicit enough in the original issue, but yes, I would like to have both to be respected.

---

_Comment by @charliermarsh on 2024-05-13 17:24_

I think it's reasonable to merge here when the type is a vector? We at least merge when we discover multiple configuration files. What do you think @zanieb?

---

_Comment by @zanieb on 2024-05-13 17:34_

I think merging seems reasonable, we have `--isolated` to stop reading the configuration file.

---

_Assigned to @charliermarsh by @charliermarsh on 2024-05-15 18:16_

---

_Label `question` removed by @charliermarsh on 2024-05-15 18:16_

---

_Label `cli` added by @charliermarsh on 2024-05-15 18:16_

---

_Referenced in [astral-sh/uv#3618](../../astral-sh/uv/pulls/3618.md) on 2024-05-15 18:59_

---

_Closed by @charliermarsh on 2024-05-20 13:37_

---

_Comment by @charliermarsh on 2024-05-20 21:10_

Out now in [v0.1.45](https://github.com/astral-sh/uv/releases/tag/0.1.45). Let me know if you continue to see issues.

---

_Comment by @jmspereira on 2024-05-21 17:30_

Everything works as expected from my side, thank you!

---

_Referenced in [astral-sh/uv#7924](../../astral-sh/uv/issues/7924.md) on 2025-06-24 20:19_

---
