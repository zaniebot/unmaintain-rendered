```yaml
number: 10342
title: "\"ALL\" should ignore deprecated rules"
type: issue
state: closed
author: philippgl
labels:
  - good first issue
  - rule-selection
assignees: []
created_at: 2024-03-11T15:30:30Z
updated_at: 2024-06-26T09:02:36Z
url: https://github.com/astral-sh/ruff/issues/10342
synced_at: 2026-01-10T11:09:52Z
```

# "ALL" should ignore deprecated rules

---

_Issue opened by @philippgl on 2024-03-11 15:30_

It would be nice, if `--select=ALL` would ignore already deprecated rules.

I think, situations, where you select "ALL", but do not want to ignore already deprecated rules are rare (but this is just a gut feeling).

see also #9778 

---

_Comment by @zanieb on 2024-03-11 15:54_

I don't think we can have deprecation be a breaking change i.e. a rule turns off when it was previously on. It's an interesting idea though.

I think it _would_ make a lot of sense to deselect these in preview mode (if we don't already? I think we do?).


---

_Label `configuration` added by @zanieb on 2024-03-11 16:41_

---

_Label `linter` added by @zanieb on 2024-03-11 16:41_

---

_Label `configuration` removed by @zanieb on 2024-03-11 16:41_

---

_Label `linter` removed by @zanieb on 2024-03-11 16:41_

---

_Label `rule-selection` added by @zanieb on 2024-03-11 16:41_

---

_Comment by @philippgl on 2024-03-11 16:42_

It is turned off in preview mode. 
But I guess, that "ALL" can break already by enabling new rules, so breaking is expected, if using "ALL"

From https://docs.astral.sh/ruff/linter/#rule-selection:
> Use ALL with discretion. Enabling ALL will implicitly enable new rules whenever you upgrade.

---

_Comment by @zanieb on 2024-03-11 16:45_

Hm that's fair... I do prefer this to #9778 so okay let's do it :)

---

_Label `good first issue` added by @zanieb on 2024-03-11 16:45_

---

_Assigned to @zanieb by @zanieb on 2024-03-11 16:47_

---

_Comment by @zanieb on 2024-03-11 16:48_

I'm going to assign myself but if anyone is interested in grabbing this feel free and I'll just be the reviewer.

---

_Comment by @WindowGenerator on 2024-03-11 20:05_

@zanieb Hi!
Can I take it?

---

_Comment by @zanieb on 2024-03-11 21:27_

@WindowGenerator go for it!

---

_Assigned to @WindowGenerator by @zanieb on 2024-03-11 21:27_

---

_Comment by @philippgl on 2024-03-12 12:20_

Wow, thanks. This is just my second issue here and I have to say, that you are as fast as ruff...

---

_Comment by @marc-benz on 2024-05-17 15:47_

Could it also make sense, to add a configuration option in the pyproject.toml that automatically ignores all deprecated rules?
Or is this already possible.

---

_Comment by @zanieb on 2024-05-17 18:42_

@marc-benz That was discussed a bit in https://github.com/astral-sh/ruff/issues/9778 I guess? I'm pretty hesitant to add that though. They'd also be ignored by switching`preview` on in your config.

---

_Added to milestone `v0.5.0` by @MichaReiser on 2024-06-26 09:02_

---

_Closed by @MichaReiser on 2024-06-26 09:02_

---
