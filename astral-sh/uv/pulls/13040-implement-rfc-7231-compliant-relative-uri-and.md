```yaml
number: 13040
title: Implement RFC 7231 compliant relative URI and fragment handling in redirects
type: pull_request
state: closed
author: zanieb
labels:
  - bug
assignees: []
draft: true
base: zb/debug-redirect
head: zb/redirect-rfc
created_at: 2025-04-22T03:26:32Z
updated_at: 2025-04-24T13:01:40Z
url: https://github.com/astral-sh/uv/pull/13040
synced_at: 2026-01-12T16:10:31Z
```

# Implement RFC 7231 compliant relative URI and fragment handling in redirects

---

_@zanieb_

https://github.com/astral-sh/uv/issues/13037 prompted me to start speculating on possible failure paths, and I found some cases we're not implementing the specification here?

A couple things

- I'm not sure if I'm joining to the base URL correctly. The RFC points to another
   
    > The field value consists of a single URI-reference.  When it has the
   form of a relative reference ([[RFC3986], Section 4.2](https://www.rfc-editor.org/rfc/rfc3986#section-4.2)), the final
   value is computed by resolving it against the effective request URI
   ([[RFC3986], Section 5](https://www.rfc-editor.org/rfc/rfc3986#section-5)).

- We need test coverage here

---

_Label `bug` added by @zanieb on 2025-04-22 03:26_

---

_Comment by @zanieb on 2025-04-22 03:54_

Extends #13038 requires restoring #13041 

---

_Comment by @jtfmumm on 2025-04-24 13:01_

Closing in favor of #13050 

---

_Closed by @jtfmumm on 2025-04-24 13:01_

---
