```yaml
number: 11650
title: Automatically detect dependencies from project imports (similar to pipreqs)
type: issue
state: open
author: Sixtails1
labels:
  - enhancement
  - wish
assignees: []
created_at: 2025-02-20T00:33:07Z
updated_at: 2025-09-10T14:12:43Z
url: https://github.com/astral-sh/uv/issues/11650
synced_at: 2026-01-10T03:23:53Z
```

# Automatically detect dependencies from project imports (similar to pipreqs)

---

_Issue opened by @Sixtails1 on 2025-02-20 00:33_

### Summary

If this functionality isn't already available, it would be nice to have a way to automatically detect the packages a project uses based on the imports, similar to how [pipreqs](https://github.com/bndr/pipreqs) works. I was thinking it could be an option for `uv tree`. Additionally, pipreqs is looking for maintainers, which I thought  presented an opportunity for uv to integrate it.

### Example

_No response_

---

_Label `enhancement` added by @Sixtails1 on 2025-02-20 00:33_

---

_Label `wish` added by @zanieb on 2025-02-20 03:35_

---

_Comment by @zanieb on 2025-02-20 03:35_

It'd be cool, but isn't something we're going to take on in the short-term.

---

_Comment by @shaneikennedy on 2025-02-24 22:02_

Hey @zanieb I'm working on exactly this as a side thing https://github.com/shaneikennedy/uv-detect

I'm happy to work on/port this into uv if astral would want it (already written in rust), I would just need some help confirming the api (i.e uv detect --update?) 

---

_Comment by @zanieb on 2025-02-24 23:11_

Thanks for the offer, but I think if we worked on this we'd probably want to integrate tightly with Ruff's parser and import analysis.

Your project seems cool though! I think it makes a lot of sense to explore third-party before we do it here.

---

_Comment by @Spenhouet on 2025-09-10 14:12_

This is technically related to the opposite operation of detecting unused packages: https://github.com/astral-sh/uv/issues/13904

---
