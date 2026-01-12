```yaml
number: 912
title: "`--add-noqa` alternates between two different rule codes"
type: issue
state: closed
author: hofrob
labels:
  - bug
assignees: []
created_at: 2022-11-26T13:08:01Z
updated_at: 2022-11-27T11:07:32Z
url: https://github.com/astral-sh/ruff/issues/912
synced_at: 2026-01-12T15:54:40Z
```

# `--add-noqa` alternates between two different rule codes

---

_@hofrob_

I just discovered this tool and am very happy with it. While implementing it into our CI I discovered that the `--add-noqa` has buggy behavior when it discovers multiple failures on one line.

# Problematic code

```py
def foo():
    return "use some weird characters in a very long line. for example `’`, nbsp characters or a character looking similar to brackets `）`"
```

# Command

```bash
ruff --select E --select RUF --add-noqa foo.py
```

This will trigger `RUF001` and `E501` but the `--add-noqa` flag will alternately add `# noqa: RUF001` and `# noqa: E501` on each run. In other situations it correctly adds `# noqa: E501, RUF001` once.

Nothing too serious though. `ruff` is a lovely addition to the python eco system. Thanks a lot!

---

_Label `bug` added by @charliermarsh on 2022-11-26 14:17_

---

_Comment by @charliermarsh on 2022-11-26 15:02_

Thank you! Will take a look at it.

---

_Comment by @charliermarsh on 2022-11-26 15:13_

Am I right that you have `--line-length 140` set in your config too?

---

_Comment by @hofrob on 2022-11-26 15:20_

`--line-length 88` in this case. But I can remove it and still reproduce it.

Now that I tried it again: the first time it works correctly (adding both rules). When you remove one, it will start alternating. I guess it's because I was working through the rules/rule sets one by one.

---

_Comment by @charliermarsh on 2022-11-26 15:24_

Yeah, the issue is that it's not preserving the existing codes. So if `noqa: E501` is present, it'll run the linter, _ignore_ `E501`, flag `RUF001`, and then set `noqa: RUF001` (removing `E501`).

---

_Comment by @charliermarsh on 2022-11-26 15:25_

Question: do you have codes listed in `noqa` that aren't supported by Ruff? Like `noqa: E203` or whatever? (Codes that exist in Flake8, but not Ruff.)

The initial intent was to remove those codes here too, to set `noqa` to the _exact_ list of Ruff violations... but not sure if that's too disruptive.


---

_Comment by @charliermarsh on 2022-11-26 15:37_

(Another example is that if you have `# noqa` (the blank catch-all), Ruff's `--add-noqa` will replace that with the precise `# noqa` needed, like `# noqa: E501`. That could also cause issues for other compatibility with other tools, but illustrates the intent of the command.)

---

_Closed by @charliermarsh on 2022-11-26 19:42_

---

_Comment by @hofrob on 2022-11-27 11:07_

> Question: do you have codes listed in noqa that aren't supported by Ruff? Like noqa: E203 or whatever? (Codes that exist in Flake8, but not Ruff.)

Nope.

> #913 

Replied, fixed, merged and released in less than 12h. Wow, that was fast. Thanks a lot!

---
