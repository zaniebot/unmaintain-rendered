```yaml
number: 8286
title: Add help page for build failures
type: pull_request
state: merged
author: konstin
labels:
  - documentation
assignees: []
merged: true
base: main
head: konsti/build-failures-faq
created_at: 2024-10-17T10:45:38Z
updated_at: 2024-10-22T11:35:55Z
url: https://github.com/astral-sh/uv/pull/8286
synced_at: 2026-01-10T12:54:06Z
```

# Add help page for build failures

---

_Pull request opened by @konstin on 2024-10-17 10:45_

One of the most common and most diverse are errors users encounter in Python packaging are build failures. They usually show lengthy, sometimes nested stack traces with no clear indication of what went wrong and how to fix it, while blocking the user.

We already catch four common cases with dedicated, actionable help messages. For all other errors, we link to a dedicated help page enumerating typical causes and their solutions.


---

_Label `documentation` added by @konstin on 2024-10-17 10:45_

---

_Review requested from @charliermarsh by @konstin on 2024-10-17 10:45_

---

_Review requested from @zanieb by @konstin on 2024-10-17 10:45_

---

_@zanieb reviewed on 2024-10-17 12:19_

---

_Review comment by @zanieb on `crates/uv/tests/it/build.rs`:892 on 2024-10-17 12:19_

This would be the first time we link out to the documentation. I'm a bit hesitant, given that it's not versioned and stable. I've been intentionally avoiding it so far. I'm not sure what we can do about it right now though.

It seems nice to have a page like this.

As a minor aside, does this belong in the reference section? It's not really a reference document.

---

_@konstin reviewed on 2024-10-17 12:23_

---

_Review comment by @konstin on `crates/uv/tests/it/build.rs`:892 on 2024-10-17 12:23_

It don't have a preference for a section (and for the specific phrasing in the document fwiw), i put it there because it didn't seem to fit anywhere.

I share the concern about the versioned documentation, otoh i wouldn't know where else to share this kind of information, it's too much to show inline.

---

_@zanieb reviewed on 2024-10-17 13:24_

---

_Review comment by @zanieb on `crates/uv/tests/it/build.rs`:892 on 2024-10-17 13:24_

An option is to add a `uv help <topic>` command? We lose rich rendering then (like links) but at least it'll be permanently accessible?

---

_@cthoyt reviewed on 2024-10-17 14:43_

---

_Review comment by @cthoyt on `docs/reference/build_failures.md`:29 on 2024-10-17 14:43_

```suggestion
       `sudo apt install default-libmysqlclient-dev build-essential pkg-config` to install the MySQL
```

---

_@konstin reviewed on 2024-10-17 15:18_

---

_Review comment by @konstin on `crates/uv/tests/it/build.rs`:892 on 2024-10-17 15:18_

The help doc needs to link to other parts of the docs for the suggestions to be actionable, so that would only push the problem of installed uv version vs. docs uv version one page further.

---

_@konstin reviewed on 2024-10-22 09:47_

---

_Review comment by @konstin on `crates/uv/tests/it/build.rs`:892 on 2024-10-22 09:47_

Trimmed this down to only the docs page. We still need to fix our docs, it can't be that we are the only ones who can't be that we are the only ones who can't link our docs.

---

_@zanieb approved on 2024-10-22 11:03_

I can edit the content in this and try to move it to a new appropriate location when I look at the docs in the near future.

---

_Merged by @konstin on 2024-10-22 11:35_

---

_Closed by @konstin on 2024-10-22 11:35_

---

_Branch deleted on 2024-10-22 11:35_

---
