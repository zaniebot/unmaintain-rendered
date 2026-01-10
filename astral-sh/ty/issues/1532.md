```yaml
number: 1532
title: Playground crashes if you name a file like a scheme with special characters
type: issue
state: closed
author: MeGaGiGaGon
labels:
  - bug
  - playground
assignees: []
created_at: 2025-11-12T18:34:44Z
updated_at: 2025-11-13T07:45:38Z
url: https://github.com/astral-sh/ty/issues/1532
synced_at: 2026-01-10T02:06:25Z
```

# Playground crashes if you name a file like a scheme with special characters

---

_Issue opened by @MeGaGiGaGon on 2025-11-12 18:34_

### Summary

As the title says, if you try to name a file something like `!:` the playground will crash with a `Uncaught Error: [UriError]: Scheme contains illegal characters.` This happens for almost any name of the form `<any number of normal + special characters>:`. The only time I found this doesn't crash is if you put the `:` after a path separator `/`

### Version

playground

---

_Label `playground` added by @AlexWaygood on 2025-11-12 18:35_

---

_Label `bug` added by @AlexWaygood on 2025-11-12 18:35_

---

_Comment by @MichaReiser on 2025-11-13 07:45_

Thank you. 

I agree that this is a bug but I'm still going to close this issue because it isn't something I plan on fixing because it isn't a goal to make the playground a production ready IDE and I'd expect some "sensible-use" ;)

I would accept a PR fixing this issue though.



---

_Closed by @MichaReiser on 2025-11-13 07:45_

---
