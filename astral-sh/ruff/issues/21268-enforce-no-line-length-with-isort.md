```yaml
number: 21268
title: Enforce no line length with isort?
type: issue
state: open
author: jeremander
labels:
  - wish
assignees: []
created_at: 2025-11-04T02:30:43Z
updated_at: 2025-11-06T01:02:59Z
url: https://github.com/astral-sh/ruff/issues/21268
synced_at: 2026-01-12T15:54:57Z
```

# Enforce no line length with isort?

---

_@jeremander_

### Question

My preferred settings have been to enforce no line length, e.g.

```toml
lint.select = ["E", "F"]

lint.ignore = ["E501"]
```

Ignoring `E501` prevents line length warnings.  However, I also want to use `isort` via I001, but if an import statement exceeds the default line length of 88, I get this error: `I001 [*] Import block is un-sorted or un-formatted`.  This error is pretty vague since it mentions nothing about line length, but when I use `--fix` it breaks the line into numerous ones, which is undesirable.

In the past I've been able to get around this by setting `line-length = 10000`, but as of `ruff` 0.14 it seems that a maximum of 320 is enforced.  So now it seems impossible to permit unlimited line length while using I001.

Is there another workaround?  Or perhaps there should be a new `line-length` configuration specifically for `isort`, analogous to `pycodestyle.max-line-length`?


### Version

_No response_

---

_Label `question` added by @jeremander on 2025-11-04 02:30_

---

_Comment by @Avasam on 2025-11-05 04:59_

Related old request about having separate line length for isort: 
- https://github.com/astral-sh/ruff/issues/3206

(personally I'd use it to simply ignore line length on imports, which I do in JS/TS)

---

_Comment by @MichaReiser on 2025-11-06 01:02_

I'd be more open to adding an option to disable the line length wrapping for imports instead of allowing arbitrarily large values. However, we'd have to make this change in both the linter and formatter. 

---

_Label `question` removed by @MichaReiser on 2025-11-06 01:02_

---

_Label `wish` added by @MichaReiser on 2025-11-06 01:02_

---
