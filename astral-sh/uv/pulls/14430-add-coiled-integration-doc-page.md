```yaml
number: 14430
title: Add Coiled integration doc page
type: pull_request
state: merged
author: jrbourbeau
labels: []
assignees: []
merged: true
base: main
head: coiled-integration
created_at: 2025-07-02T19:53:08Z
updated_at: 2025-09-12T12:24:33Z
url: https://github.com/astral-sh/uv/pull/14430
synced_at: 2026-01-10T06:36:15Z
```

# Add Coiled integration doc page

---

_Pull request opened by @jrbourbeau on 2025-07-02 19:53_

This PR adds a new integrations doc page for using uv with [Coiled](https://coiled.io/). It's a slightly adapted version of this blog post https://docs.coiled.io/blog/uv-coiled-cloud-scripts.html

Side note: it's been really pleasant using uv and Coiled together recently 

cc @zanieb for visibility 

---

_Assigned to @zanieb by @zanieb on 2025-07-02 19:56_

---

_@zanieb reviewed on 2025-07-09 15:53_

---

_Review comment by @zanieb on `docs/guides/integration/coiled.md`:36 on 2025-07-09 15:53_

```suggestion
dependencies directly in the script using [PEP 723](https://peps.python.org/pep-0723/) inline
```

---

_@zanieb reviewed on 2025-07-09 15:53_

---

_Review comment by @zanieb on `docs/guides/integration/coiled.md`:23 on 2025-07-09 15:53_

```suggestion
```python title="process.py"
import pandas as pd
```

---

_@zanieb reviewed on 2025-07-09 15:54_

---

_Review comment by @zanieb on `docs/guides/integration/coiled.md`:47 on 2025-07-09 15:54_

```suggestion
```python title="process.py" hl_lines="1-8"
# /// script
```

---

_@zanieb reviewed on 2025-07-09 15:55_

---

_Review comment by @zanieb on `docs/guides/integration/coiled.md`:160 on 2025-07-09 15:55_

```suggestion
```bash hl_lines="1"
$ uvx coiled batch run \
```

---

_@zanieb reviewed on 2025-07-09 15:55_

I think I need to do a pass on this to make the language / tone consistent with the rest of the documentation, so feel free to just leave those minor changes.

---

_Comment by @jrbourbeau on 2025-07-09 15:58_

> I think I need to do a pass on this to make the language / tone consistent with the rest of the documentation, so feel free to just leave those minor changes.

Sounds good, thanks for reviewing. Happy to iterate as needed 

---

_@zanieb reviewed on 2025-08-01 13:19_

---

_Review comment by @zanieb on `docs/guides/integration/coiled.md`:57 on 2025-08-01 13:19_

I didn't want to duplicate too much detail on using scripts with inline metadata. I'm hoping we can direct people to the official guide if they want more on that.

---

_@zanieb reviewed on 2025-08-01 13:21_

---

_Review comment by @zanieb on `docs/guides/integration/coiled.md`:142 on 2025-08-01 13:21_

@jrbourbeau Let's talk about this bit and what we can do to clarify this or improve on the experience?

---

_@zanieb reviewed on 2025-08-01 13:22_

---

_Review comment by @zanieb on `docs/guides/integration/coiled.md`:142 on 2025-08-01 13:22_

(I don't really expect you to go change things in Coiled in service of this goal, but I was pretty confused about what was happening and I'm an advanced user, so we need to say more at least)

---

_Comment by @zanieb on 2025-08-01 13:23_

@jrbourbeau I made quite a few edits, please let me know if you have qualms with anything! Thanks for your patience.

---

_@jrbourbeau reviewed on 2025-08-20 21:47_

---

_Review comment by @jrbourbeau on `docs/guides/integration/coiled.md`:142 on 2025-08-20 21:47_

> I also wasn't sure how to get the logs for the job. I eventually found it with
`uvx coiled batch logs 1067394`. 

Yeah, that's exactly where to go for getting logs programatically. The other, less programatic, way is to look at logs in the Coiled UI. We output the link to view the batch job in the Coiled UI in the terminal output from `coiled batch run`

<img width="1552" height="987" alt="Screenshot 2025-08-18 at 2 44 13 PM" src="https://github.com/user-attachments/assets/f193163f-cdc2-471b-8128-22603cc2c03b" />

> if the user experience for running the script locally is that it waits for execution
and shows the output, then we need to address that difference in user experience here

That's a fair point. I went ahead an made a small update here https://github.com/astral-sh/uv/pull/14430/commits/109a0992ddd5d14dd258b01e8768f692cab8b064 that hopefully helps address this. Let me know what you think -- happy to update again 

> It looks like `uvx coiled batch run -- uv run process.py` isn't supported (using the `--` as a
separator), I wanted to use that for a single-line command that still separated the `uv run`
command.

Hmm that's supposed to work and I'm not able to reproduce an issue -- what error are you seeing? 

<img width="1396" height="836" alt="Screenshot 2025-08-18 at 2 35 03 PM" src="https://github.com/user-attachments/assets/9ad73d80-5236-4b41-9268-4b5b80391442" />


---

_Merged by @zanieb on 2025-09-12 12:24_

---

_Closed by @zanieb on 2025-09-12 12:24_

---
