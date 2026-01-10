```yaml
number: 750
title: Project specific verification
type: issue
state: closed
author: KaQuMiQ
labels:
  - question
assignees: []
created_at: 2025-07-02T10:44:13Z
updated_at: 2025-07-03T07:41:15Z
url: https://github.com/astral-sh/ty/issues/750
synced_at: 2026-01-10T02:07:36Z
```

# Project specific verification

---

_Issue opened by @KaQuMiQ on 2025-07-02 10:44_

### Question

Hi! 
I am curious if you are running ty against some well typed python projects? 

I am heavily utilizing python typing (to the extreme) in my projects and develop two libraries which are fully, strictly typed [haiway](https://github.com/miquido/haiway) and [draive](https://github.com/miquido/draive). I am using pyright strict typing mode and put types wherever I can including heavy usage of parametric polymorphism (generics) of both functions and classes.

I am really interested in ty since I already use (and love!) ruff and uv. Current version (run through `uvx ty check`) seems to have multiple issues when running on my codebase (both panics and false positives). I am wondering about possibility to either report all of the issues (there is plenty) or having you being interested to run ty against any of those?

### Version

_No response_

---

_Label `question` added by @KaQuMiQ on 2025-07-02 10:44_

---

_Comment by @carljm on 2025-07-02 17:14_

Hi! Thanks for the question.

We run ty in CI on every pull request on ~120 projects via https://github.com/hauntsaninja/mypy_primer/. You could consider submitting a PR to that project to include your projects, and then your projects would become visible in our CI, as well as in the CI of other existing Python type checkers. (Although if we currently panic on your projects, we would exclude them from our CI for now until we fix those issues.)

We have a lot of existing open issues, particularly around generics which your projects use heavily. Chances are good that most of the issues you are seeing (including the panics) are already known to us. So while we definitely welcome reporting any problems that don't seem to be covered by an existing issue, it may take a lot of time for you to search for an existing issue for each problem. So it may be a better use of your time to just give it some time and check back in later.

---

_Comment by @KaQuMiQ on 2025-07-03 07:41_

Sure! Thank you for your response, keep going astral team! 😄 

---

_Closed by @KaQuMiQ on 2025-07-03 07:41_

---
