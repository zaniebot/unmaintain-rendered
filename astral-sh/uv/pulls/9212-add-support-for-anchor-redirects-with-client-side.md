```yaml
number: 9212
title: Add support for anchor redirects with client-side js
type: pull_request
state: merged
author: zanieb
labels:
  - documentation
assignees: []
merged: true
base: main
head: zb/docs-redirect-anchor
created_at: 2024-11-18T20:29:42Z
updated_at: 2024-11-20T04:32:44Z
url: https://github.com/astral-sh/uv/pull/9212
synced_at: 2026-01-10T12:00:00Z
```

# Add support for anchor redirects with client-side js

---

_Pull request opened by @zanieb on 2024-11-18 20:29_

The redirect plugin doesn't support this, and it's not feasible to do server-side so we need to do the redirect client-side with some javascript.

---

_Label `documentation` added by @zanieb on 2024-11-18 20:29_

---

_@zanieb reviewed on 2024-11-18 20:30_

---

_Review comment by @zanieb on `docs/js/extra.js`:12 on 2024-11-18 20:30_

Prettier strikes!

---

_@dhruvmanila reviewed on 2024-11-19 02:54_

---

_Review comment by @dhruvmanila on `docs/js/extra.js`:86 on 2024-11-19 02:54_

Is this required? Could we just do `window.location.pathname + window.location.hash` ?

---

_@dhruvmanila reviewed on 2024-11-19 02:55_

---

_Review comment by @dhruvmanila on `docs/js/extra.js`:57 on 2024-11-19 02:55_

We'll probably need to add an event listener for this, mostly on [`DOMContentLoaded`](https://developer.mozilla.org/en-US/docs/Web/API/Document/DOMContentLoaded_event).

---

_@dhruvmanila approved on 2024-11-19 02:56_

---

_@zanieb reviewed on 2024-11-19 14:05_

---

_Review comment by @zanieb on `docs/js/extra.js`:86 on 2024-11-19 14:05_

Is the pathname guaranteed to end in a `/`?

---

_@zanieb reviewed on 2024-11-19 14:06_

---

_Review comment by @zanieb on `docs/js/extra.js`:57 on 2024-11-19 14:06_

Why? I don't think you can get to a page that doesn't exist except on initial load — i.e., like only an external link should reference it.

---

_@dhruvmanila reviewed on 2024-11-20 04:28_

---

_Review comment by @dhruvmanila on `docs/js/extra.js`:86 on 2024-11-20 04:28_

I'm not sure, I can't find any documentation about whether this is guaranteed or not but from my experience modern browsers do tend to maintain this consistency even if you might not include a trailing `/` the browser will include it. I think it's fine to keep this in just to be on the safer side.

---

_@dhruvmanila reviewed on 2024-11-20 04:32_

---

_Review comment by @dhruvmanila on `docs/js/extra.js`:57 on 2024-11-20 04:32_

Oh right, because all the internal references have been updated.

---

_Merged by @zanieb on 2024-11-20 04:32_

---

_Closed by @zanieb on 2024-11-20 04:32_

---

_Branch deleted on 2024-11-20 04:32_

---
