---
number: 11780
title: New dependency r-shquote appears unmaintained
type: issue
state: closed
author: musicinmybrain
labels:
  - dependencies
assignees: []
created_at: 2025-02-25T18:37:19Z
updated_at: 2025-02-26T21:47:25Z
url: https://github.com/astral-sh/uv/issues/11780
synced_at: 2026-01-10T01:25:10Z
---

# New dependency r-shquote appears unmaintained

---

_Issue opened by @musicinmybrain on 2025-02-25 18:37_

### Summary

The [`r-shquote` crate](https://crates.io/crates/r-shquote), a new dependency added in `uv` 0.6.3 via 3e04fdb8ae7be7e99b2bb683454ed580c5da6d44, is unmaintained: https://github.com/r-util/r-shquote has been archived since 2020-04-16, and the last release was 2018-10-30.

I don’t have an immediate suggestion for a maintained alternative, but I still thought it was worth raising this as a potential issue.

### Platform

N/A

### Version

0.6.3

### Python version

N/A

---

_Label `bug` added by @musicinmybrain on 2025-02-25 18:37_

---

_Comment by @nkitsaini on 2025-02-25 19:02_

Luckily POSIX shell parsing is stable, so the project being unmaintained is not as big of an issue compared to other libraries as we don't need any further updates.

Also the implementation is small enough that we can easily re-implement it if required.

---

_Assigned to @charliermarsh by @zanieb on 2025-02-25 19:45_

---

_Comment by @charliermarsh on 2025-02-25 19:47_

Yeah I considered just vendoring it, it's very small (a single file, a few hundred lines), but didn't see much upside.

---

_Label `bug` removed by @charliermarsh on 2025-02-25 19:47_

---

_Label `dependencies` added by @charliermarsh on 2025-02-25 19:47_

---

_Comment by @samypr100 on 2025-02-26 03:42_

Agreed with @nkitsaini, cases like this is more defined as "complete" rather than "unmaintained" 

---

_Comment by @musicinmybrain on 2025-02-26 12:27_

Thanks for the input, everyone! I’m going to close this, since everyone seems to be relatively satisfied with the *status quo* for the time being.

---

_Closed by @musicinmybrain on 2025-02-26 12:27_

---

_Comment by @musicinmybrain on 2025-02-26 13:32_

> Agreed with [@nkitsaini](https://github.com/nkitsaini), cases like this is more defined as "complete" rather than "unmaintained"

I would quibble with this slightly: a “complete” crate might still have had no releases in six years, but the source repository wouldn’t be archived, and there would be somewhere to report any bugs that did materialize. Still, I understand the sentiment.

---

_Comment by @nkitsaini on 2025-02-26 14:38_


> the source repository wouldn’t be archived

Yes, it would definitely be worrying if this was a more complex implementation (like html parsing let's say) as there might always be unidentified bugs and it would be non-trivial effort to maintain it ourselves. 

But in this case implementation is very small, so if/when we encounter any bug we can just vendor the code and maintain it. So the only reason I think it is not an issue here is because the implementation is "complete" _and_ it is very small. If any of these two conditions were false, it would not be safe to have this as a dependency.

This is subjective and views are my own.

---

_Comment by @charliermarsh on 2025-02-26 21:38_

I decided to vendor this anyway. We only need a single function, and given that it's archived, it'd be concerning if it actually _was_ updated. I could go either way on it, but it seemed easy to do here. See: https://github.com/astral-sh/uv/pull/11812.

---

_Comment by @nkitsaini on 2025-02-26 21:47_

> I would quibble with this slightly:

It was worth it 😄.

---
