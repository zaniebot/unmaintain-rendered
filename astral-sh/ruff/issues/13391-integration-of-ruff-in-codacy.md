```yaml
number: 13391
title: Integration of Ruff in Codacy
type: issue
state: closed
author: valeriupredoi
labels:
  - wish
assignees: []
created_at: 2024-09-18T12:14:24Z
updated_at: 2025-08-07T16:11:26Z
url: https://github.com/astral-sh/ruff/issues/13391
synced_at: 2026-01-10T11:09:55Z
```

# Integration of Ruff in Codacy

---

_Issue opened by @valeriupredoi on 2024-09-18 12:14_

Hi folks, apologies if perhaps this would have been better off a Discussion object, but I reckon if code changes get involved it's better to have an Issue to quote.

We at [ESMValTool](https://github.com/ESMValGroup/ESMValTool) are looking at moving from Prospector to Ruff, but we are missing one integration, namely Ruff in Codacy. I have started a thread with Codacy asking them if they are willing to create the bridge (see https://github.com/codacy/codacy-engine-scala-seed/issues/59 ) but by the looks of it, they have a self-serve system whereby one builds their own integration package (a Docker container, in essence). My colleague @bouweandela and myself are rather keen to move away from Prospector rather sooner than later, so we'd be willing to get all the bits in place so we do that, so:

- would you be willing to create a `codacy-ruff` package? See example the [codacy-prospector](https://github.com/codacy/codacy-prospector) one - looks pretty straightforward to me
- alternatively, would you be OK with us building and deploying such a package?

Very many thanks in advance, and many thanks for creating this tool! :beer: 

---

_Comment by @MichaReiser on 2024-09-19 07:45_

Previous discussion: https://github.com/astral-sh/ruff/discussions/10458

---

_Label `needs-decision` added by @MichaReiser on 2024-09-19 07:46_

---

_Label `wish` added by @MichaReiser on 2024-09-19 07:47_

---

_Comment by @MichaReiser on 2024-09-19 07:48_

Let me check in with the team on our preference regarding first-party/third-party. 

---

_Comment by @valeriupredoi on 2024-09-19 08:18_

@MichaReiser great, many thanks, mate :beer: 

---

_Comment by @MichaReiser on 2024-09-19 16:25_

> alternatively, would you be OK with us building and deploying such a package?

That would be a great outcome! We don't have the relevant codacy knowledge and are very busy with improving ruff's semantic model. You can ping me if you have ruff specific questions. Unfortunately, I don't know anything about codacy. 

---

_Label `needs-decision` removed by @MichaReiser on 2024-09-19 16:25_

---

_Comment by @valeriupredoi on 2024-09-20 12:10_

@MichaReiser cheers for the heads up, so your team is OK with us doing that, or you still need to run it by them? - I'll wait, there's no rush at our end :beer: 

---

_Comment by @MichaReiser on 2024-09-20 12:18_

We would welcome a third-party codacy extension.

---

_Comment by @valeriupredoi on 2024-09-20 12:49_

> We would welcome a third-party codacy extension.

Great! Cheers, mate :beer: I'll discuss with me colleagues, and if we agree on us building one, then I'll get cracking with it :+1: 

---

_Comment by @MichaReiser on 2025-08-07 16:06_

This seems to now be available in codacy https://blog.codacy.com/ruff-a-powerful-new-python-linter-in-codacy

---

_Closed by @MichaReiser on 2025-08-07 16:06_

---

_Comment by @valeriupredoi on 2025-08-07 16:11_

Indeed, we popped it in there, apologies for forgetting to close this one, cheers 🍻 

---
