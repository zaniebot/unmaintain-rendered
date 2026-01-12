```yaml
number: 5495
title: Ruff rule does not describe multiple rules
type: issue
state: closed
author: rotu
labels:
  - bug
assignees: []
created_at: 2023-07-03T20:52:29Z
updated_at: 2023-07-04T19:23:07Z
url: https://github.com/astral-sh/ruff/issues/5495
synced_at: 2026-01-12T15:54:45Z
```

# Ruff rule does not describe multiple rules

---

_@rotu_

version = 0.0.272

Flake8 supports providing a partial error code like "E4" to select all error codes starting with "E4" (i.e. E401 and E402)

`ruff . --select E4` runs both E401 and E402.
But `ruff rule E4` describes E401 and does not even mention that "E4" also entails E402.

---

_Comment by @rotu on 2023-07-03 20:54_

Behavior has changed since #2812 was reported. Not sure if current behavior is intentional.

---

_Label `bug` added by @charliermarsh on 2023-07-03 23:57_

---

_Comment by @charliermarsh on 2023-07-03 23:58_

My initial preference would be to error on `ruff rule E4`, rather than show multiple rules, since we don't really have a good format for that. At the very least, it's wrong to show the `E402` docs in that case!

---

_Comment by @rotu on 2023-07-04 06:35_

@charliermarsh erroring would be good. Even better would be showing the list of rules it expands to (just the ID and short name is plenty useful!)

---

_Comment by @dhruvmanila on 2023-07-04 13:59_

The behavior got changed in `0.0.247`, earlier we were throwing an error:
```console
❯ pipx run "ruff==0.0.246" rule E4                               
⚠️  ruff is already on your PATH and installed at /Users/dhruv/.local/bin/ruff. Downloading and running anyway.
error: invalid value 'E4' for '<RULE>': unknown rule code

For more information, try '--help'.
```

I think it might be related to the `many-to-one` changes: `git log --grep='^many-to-one' --oneline`

---

_Comment by @charliermarsh on 2023-07-04 18:19_

@dhruvmanila - Are you taking a look at the lack of an error?

---

_Comment by @dhruvmanila on 2023-07-04 18:46_

You can look at it if you want, I just thought to perform a quick bisection search to find out when did the behavior changed.

---

_Comment by @charliermarsh on 2023-07-04 19:00_

I'll take a quick look.

---

_Closed by @charliermarsh on 2023-07-04 19:23_

---
