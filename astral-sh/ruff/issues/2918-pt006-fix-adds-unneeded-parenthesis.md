---
number: 2918
title: PT006 fix adds unneeded parenthesis
type: issue
state: closed
author: frenck
labels:
  - bug
assignees: []
created_at: 2023-02-15T11:53:54Z
updated_at: 2023-04-14T01:28:32Z
url: https://github.com/astral-sh/ruff/issues/2918
synced_at: 2026-01-10T01:22:41Z
---

# PT006 fix adds unneeded parenthesis

---

_Issue opened by @frenck on 2023-02-15 11:53_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->

PT006 seems to add additional parenthesis in case the string is already wrapped in a parenthesis to split it into multiple lines.

![image](https://user-images.githubusercontent.com/195327/219019029-fc0a9da7-14e5-41b9-8580-987d0dc0cd90.png)

```py
@pytest.mark.parametrize(
    (
        "create_backup_error, create_backup_calls, "
        "update_addon_error, update_addon_calls, "
        "error_message"
    ),
	[...]
)
```

Results in:

```py
@pytest.mark.parametrize(
    (
        (
            "create_backup_error",
            "create_backup_calls",
            "update_addon_error",
            "update_addon_calls",
            "error_message",
        )
    ),
    [...]
)
```

Expected:

```py
@pytest.mark.parametrize(
    (
        "create_backup_error",
        "create_backup_calls",
        "update_addon_error",
        "update_addon_calls",
        "error_message",
    ),
    [...]
)
```


---

_Referenced in [home-assistant/core#88165](../../home-assistant/core/pulls/88165.md) on 2023-02-15 11:55_

---

_Label `bug` added by @charliermarsh on 2023-02-15 13:18_

---

_Comment by @charliermarsh on 2023-02-15 13:18_

Thanks :)

---

_Comment by @charliermarsh on 2023-02-17 22:13_

Ohh interesting, because the expression is itself parenthesized already. Hmm...

---

_Comment by @dhruvmanila on 2023-04-07 11:03_

So, I was looking at the recent commit (https://github.com/charliermarsh/ruff/commit/abaf0a198d9d2cebefd5c029bb271be1a3498f20) and thought of a similar approach in solving this problem.

The indexer will collect all the ranges for a pair of parenthesis (`(` to `)`). Then, while trying to generate a fix for the above case, we'll check how many ranges does the string expression range belongs to. If the value is 2, we won't add the parenthesis otherwise we will.

I'm not sure if you've any other solution in mind so just thought to share this.

---

_Comment by @dhruvmanila on 2023-04-13 05:12_

Actually, I've a better solution in mind, will try to fix it by tonight.

---

_Referenced in [astral-sh/ruff#3955](../../astral-sh/ruff/pulls/3955.md) on 2023-04-13 07:20_

---

_Closed by @charliermarsh on 2023-04-13 17:56_

---

_Comment by @frenck on 2023-04-13 18:00_

Thanks, @dhruvmanila 👍 

../Frenck

---

_Comment by @charliermarsh on 2023-04-13 18:41_

Another all-star contribution from @dhruvmanila :)

---

_Comment by @dhruvmanila on 2023-04-14 01:28_

> Another all-star contribution from @dhruvmanila :)

Thanks for your kind words! Always happy to help wherever I can 😄 

---
