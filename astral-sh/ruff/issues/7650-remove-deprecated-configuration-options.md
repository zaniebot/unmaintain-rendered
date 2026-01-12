```yaml
number: 7650
title: Remove deprecated configuration options
type: issue
state: closed
author: MichaReiser
labels:
  - configuration
assignees: []
created_at: 2023-09-25T09:25:31Z
updated_at: 2024-06-26T08:15:14Z
url: https://github.com/astral-sh/ruff/issues/7650
synced_at: 2026-01-12T15:54:47Z
```

# Remove deprecated configuration options

---

_@MichaReiser_

Remove configuration options that are marked as deprecated.

---

_Label `configuration` added by @MichaReiser on 2023-09-25 09:25_

---

_Comment by @tibor-reiss on 2024-02-04 19:01_

Looks like a nice place to start contributing to this super tool!

---

_Added to milestone `v0.3.0` by @zanieb on 2024-02-04 21:00_

---

_Removed from milestone `v0.3.0` by @MichaReiser on 2024-02-14 16:27_

---

_Added to milestone `v0.4` by @MichaReiser on 2024-02-14 16:27_

---

_Removed from milestone `v0.4.0` by @dhruvmanila on 2024-04-18 19:48_

---

_Added to milestone `v0.5.0` by @dhruvmanila on 2024-04-18 19:48_

---

_Comment by @MichaReiser on 2024-06-21 12:25_

As of today:
* `show-source`
* `tab-size`
* Top level lint settings 
* `extend-ignore`
* `extend-unfixable`
* `ignore-init-module-imports`


---

_Comment by @MichaReiser on 2024-06-24 07:14_

> `ignore-init-module-imports`

This option has only been deprecated with Ruff 0.4.4 (https://github.com/astral-sh/ruff/pull/11436) and the new behavior is still gated behind preview (we may want to promote it to stable?). I think we should wait with removing the option until after the new F401 behavior has been stabilized



---

_Assigned to @MichaReiser by @MichaReiser on 2024-06-24 08:26_

---

_Comment by @MichaReiser on 2024-06-24 08:31_

There's also the `output-format` `text` that is deprecated and that we probably should remove

---

_Comment by @MichaReiser on 2024-06-26 08:15_

I'll close this and will create specific deprecation tasks to ease tracking of what needs to be deprecated/errored/removed when.

---

_Closed by @MichaReiser on 2024-06-26 08:15_

---
