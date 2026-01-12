```yaml
number: 11594
title: "Request: prefer `%r`/`!r` marker instead of calling `repr` in string formatting"
type: issue
state: open
author: Avasam
labels: []
assignees: []
created_at: 2024-05-29T05:27:38Z
updated_at: 2024-05-29T05:27:38Z
url: https://github.com/astral-sh/ruff/issues/11594
synced_at: 2026-01-12T15:54:51Z
```

# Request: prefer `%r`/`!r` marker instead of calling `repr` in string formatting

---

_@Avasam_

See this PR for the kind of changes I'd like to see automated: https://github.com/mhammond/pywin32/pull/2272

From:
```py
"%s %s" % (repr(foo), bar)
"{} {}".format(repr(foo), bar)
f"{repr(foo)} {bar}"
```

To:
```py
"%r %s" % (foo, bar)
"{!r} {}".format(foo, bar)
f"{foo!r} {bar}"
```

I don't think this is supported by any linter atm. This should synergize well with UP030-UP032

---
