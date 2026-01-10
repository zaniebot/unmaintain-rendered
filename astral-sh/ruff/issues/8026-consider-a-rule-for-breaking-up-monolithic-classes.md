---
number: 8026
title: Consider a rule for breaking up monolithic classes
type: issue
state: closed
author: NeilGirdhar
labels: []
assignees: []
created_at: 2023-10-17T21:20:59Z
updated_at: 2023-10-17T21:21:18Z
url: https://github.com/astral-sh/ruff/issues/8026
synced_at: 2026-01-10T01:22:47Z
---

# Consider a rule for breaking up monolithic classes

---

_Issue opened by @NeilGirdhar on 2023-10-17 21:20_

I'm actually going to open and then close this.  It's an interesting idea, but probably too much work, and may not be popular.

[This treatise](http://www.gotw.ca/gotw/084.htm) on monolithic classes by OO expert Herb Sutter proposes the general principle that bare functions should be preferred when they can be implemented using the public interface of a class.

Could Ruff incorporate a rule for monolithic classes (more than a given number of member functions) that detects those members that could be implemented using the public interface of the class, and suggest that those members be converted to bare functions?

---

_Closed by @NeilGirdhar on 2023-10-17 21:21_

---
