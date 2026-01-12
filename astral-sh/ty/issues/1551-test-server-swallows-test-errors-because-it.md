```yaml
number: 1551
title: "Test server: Swallows test errors because it throws an exception that there are unawaited responses"
type: issue
state: closed
author: MichaReiser
labels:
  - server
  - testing
assignees: []
created_at: 2025-11-14T11:40:14Z
updated_at: 2025-11-14T18:48:39Z
url: https://github.com/astral-sh/ty/issues/1551
synced_at: 2026-01-12T15:54:25Z
```

# Test server: Swallows test errors because it throws an exception that there are unawaited responses

---

_@MichaReiser_

The `Drop` handler of the test server contains an assertion that all messages were processed. This assertion runs except when a test panicks. 

However, most tests don't panic if an expected message wasn't received, instead, they exit early with an `Err` result. In this case, the assertion in the drop handler still runs which is then very likely to panic because the test didn't get a chance to consume all its expected messages (if it expects two different kind of mesages).


I think we should simply remove the assertion. Tests that want to assert that all messages were consumed can do so by calling the existing method (we can make it public) at the end of the test. 

The only assertion that I think we want to keep is that the server doesn't send any new messages after shutdown.

---

_Label `testing` added by @MichaReiser on 2025-11-14 11:40_

---

_Label `server` added by @AlexWaygood on 2025-11-14 11:49_

---

_Comment by @MichaReiser on 2025-11-14 18:48_

I think what we have in #21451 should be sufficient for most tests. It's still a bit of a footgun but only very few tests use the `try_` variants (and they either panic or ignore the error, so they're not affected by the outlined issue)

---

_Closed by @MichaReiser on 2025-11-14 18:48_

---
