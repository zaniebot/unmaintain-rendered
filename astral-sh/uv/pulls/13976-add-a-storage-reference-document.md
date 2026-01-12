```yaml
number: 13976
title: "Add a \"storage\" reference document"
type: pull_request
state: open
author: zanieb
labels:
  - documentation
assignees: []
base: main
head: zb/storage
created_at: 2025-06-11T20:42:04Z
updated_at: 2025-08-27T15:04:27Z
url: https://github.com/astral-sh/uv/pull/13976
synced_at: 2026-01-12T16:10:57Z
```

# Add a "storage" reference document

---

_@zanieb_

We're getting questions about this. It seems best to consolidate this information?

---

_Label `documentation` added by @zanieb on 2025-06-11 20:42_

---

_Review comment by @konstin on `docs/reference/storage.md`:9 on 2025-06-11 20:44_

We should mention that we XDG on Unix specifically here, otherwise it's unclear what platform conventions we mean.

---

_Review comment by @konstin on `docs/reference/storage.md`:15 on 2025-06-11 20:46_

We should be using either `$HOME` or `~` throughout the document.

---

_Review comment by @konstin on `docs/reference/storage.md`:44 on 2025-06-11 20:50_

This section is duplicated with the "Cache directory" in cache.md

---

_Review comment by @konstin on `docs/reference/storage.md`:48 on 2025-06-11 20:51_

Can we add a note that the cache needs to be on the same filesystem as the venv for best performance?

---

_Review comment by @konstin on `docs/reference/storage.md`:91 on 2025-06-11 20:52_

In "Tools directory" in tools.md, we call this directory "application state directory" instead.

---

_@konstin reviewed on 2025-06-11 20:52_

---

_@zanieb reviewed on 2025-06-11 21:19_

---

_Review comment by @zanieb on `docs/reference/storage.md`:48 on 2025-06-11 21:19_

That seems like a `cache.md` thing, but yes.

---

_@zanieb reviewed on 2025-06-11 21:19_

---

_Review comment by @zanieb on `docs/reference/storage.md`:91 on 2025-06-11 21:19_

Thanks! I didn't look at `cache.md` or `tools.md` at all yet, so I'll see what I can do to make the relationship clearer between these documents and use consistent language.

---

_@zanieb reviewed on 2025-06-11 21:20_

---

_Review comment by @zanieb on `docs/reference/storage.md`:9 on 2025-06-11 21:20_

I do think this becomes clear in the next section, but I think I can add something.

---

_@konstin reviewed on 2025-06-11 21:21_

---

_Review comment by @konstin on `docs/reference/storage.md`:48 on 2025-06-11 21:21_

When reviewing I've struggled with which information goes here vs. in one of the detailed (concept) pages.

---

_@zanieb reviewed on 2025-06-11 21:32_

---

_Review comment by @zanieb on `docs/reference/storage.md`:48 on 2025-06-11 21:32_

Yeah I think this page is a bit weird and will be duplicative in some ways. I can spend more time on it though, I put this up very quickly.

---

_@samypr100 reviewed on 2025-08-27 14:40_

---

_Review comment by @samypr100 on `docs/reference/storage.md`:9 on 2025-08-27 14:40_

It may be worth crosslinking or updating to the XDG section in `reference/environment.md` to this document
* https://docs.astral.sh/uv/reference/environment/#xdg_bin_home
* https://docs.astral.sh/uv/reference/environment/#xdg_cache_home
* https://docs.astral.sh/uv/reference/environment/#xdg_config_dirs
* https://docs.astral.sh/uv/reference/environment/#xdg_config_home
* https://docs.astral.sh/uv/reference/environment/#xdg_data_home

---

_@zanieb reviewed on 2025-08-27 15:04_

---

_Review comment by @zanieb on `docs/reference/storage.md`:9 on 2025-08-27 15:04_

I don't think we have the infrastructure to properly cross-link from the environment variable reference yet.

---
