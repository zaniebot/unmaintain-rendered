---
number: 11252
title: Consider emitting a warning for the use of continuation characters (except in strings)
type: issue
state: open
author: NeilGirdhar
labels:
  - rule
assignees: []
created_at: 2024-05-02T19:49:48Z
updated_at: 2024-05-03T02:10:36Z
url: https://github.com/astral-sh/ruff/issues/11252
synced_at: 2026-01-07T13:12:15-06:00
---

# Consider emitting a warning for the use of continuation characters (except in strings)

---

_Issue opened by @NeilGirdhar on 2024-05-02 19:49_

The continuation character used outside of strings are no longer necessary as of Python 3.9 thanks to the new PEG parser.  This means that we can always use parentheses.  And parentheses have the advantage of not interfering with comments, and for some people are easier to read.

Also, doing a grep on Python source suggests that many Python programmers are preferring parenthesis continuations—except in strings.

Would it make sense to warn on them?

---

_Label `rule` added by @charliermarsh on 2024-05-03 00:39_

---

_Comment by @charliermarsh on 2024-05-03 00:40_

For completeness, can you give an example of the pattern you want to lint against, and the suggested alternative?

---

_Comment by @NeilGirdhar on 2024-05-03 01:55_

Sure, from the CPython source:
```python
        if self.size_read == self.chunksize and \
           self.align and \
           (self.chunksize & 1):
            dummy = self.file.read(1)
```
I prefer:
```python
        if (self.size_read == self.chunksize and self.align
                and (self.chunksize & 1)):
            dummy = self.file.read(1)
```
Of course, it's a matter of opinion, but apparently [according a core developor](https://discuss.python.org/t/python-should-allow-comments-after-continuation-line-bar/52416/29):
> Guido and other core developers are not fans of using backslash to negate line-ends. Not only do fence pairs (parentheses, brackets, and braces) negate all contained line-ends, but the use of grouping parentheses has been expanded to eliminate most/all needs for line-end backslashes. For instance, 3.10 added the use of grouping parentheses to write [with statements ](https://docs.python.org/3/reference/compound_stmts.html#the-with-statement)over multiple lines without using backslash. AFAIK, this was the last required use of backslash, line-end in normal code.

---

_Comment by @charliermarsh on 2024-05-03 02:10_

Not a 'decision' but as a heads up I'm generally hesitant to add more rules that are made redundant with the formatter.

---
