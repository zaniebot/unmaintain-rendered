```yaml
number: 17526
title: "Respect `Content-Disposition` for direct URL installations?"
type: issue
state: open
author: woodruffw
labels:
  - needs-decision
assignees: []
created_at: 2026-01-16T16:16:15Z
updated_at: 2026-01-16T17:53:24Z
url: https://github.com/astral-sh/uv/issues/17526
synced_at: 2026-01-16T18:01:07Z
```

# Respect `Content-Disposition` for direct URL installations?

---

_@woodruffw_

This is a breakout from #17426, since we need to obtain clarity/consensus on an approach here.

TL;DR: URLs don't necessarily have a clear filename suffix. When this happens, uv doesn't have a filename to use for the URL's content, which could be either a sdist or a wheel. Consequently, the URL is rejected.

One thing we could do (for a subset of scenarios) is to honor the `Content-Disposition` header, which _can_ include a `filename` field. 

pip appears to do this, and gives it precedence over any inferred filename:

https://github.com/pypa/pip/blob/545eda389c41478e2f99d23212254d757d8c2cef/src/pip/_internal/network/download.py#L105-L117

Some things we need clarity on (these may already have answers, I just haven't collected the information together):

1. How does this interact with other PEP 508 (and non-standard) distribution name/filename specification mechanisms for URLs? 
2. There are other `Content-Distribution` mechanisms for specifying filenames, e.g. `filename*`. We should see if pip handles these, if they do the precedence between them correctly, etc.


---

_Label `needs-decision` added by @woodruffw on 2026-01-16 16:16_

---

_Comment by @notatallshaw on 2026-01-16 17:53_

FYI, I'm following this discussion, if you think this puts pip out of standards compliance I will take a look. Although this is hairy enough I'm not likely going to change the behavior beyond logging a warning when content disposition does not match with the actual filename.

---
