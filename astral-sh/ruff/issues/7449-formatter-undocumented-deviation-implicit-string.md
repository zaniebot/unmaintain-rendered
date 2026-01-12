```yaml
number: 7449
title: "Formatter undocumented deviation: implicit string concatenation"
type: issue
state: closed
author: m-richards
labels:
  - formatter
assignees: []
created_at: 2023-09-17T06:01:54Z
updated_at: 2023-09-27T15:59:14Z
url: https://github.com/astral-sh/ruff/issues/7449
synced_at: 2026-01-12T15:54:47Z
```

# Formatter undocumented deviation: implicit string concatenation

---

_@m-richards_

Just sharing another deviation, this time to do with implicit string concatenation (all with line-length=88):
```python
# black / existing formatting
class TestIO:
    def test(self):
        new_rows, orig_rows = 1, 1

        assert new_rows == orig_rows * 2, (
            "There should be {target} rows,"
            "found: {current}".format(target=orig_rows * 2, current=new_rows),
        )
```
Then with ruff this becomes
```python
class TestIO:
    def test(self):
        new_rows, orig_rows = 1, 1

        assert new_rows == orig_rows * 2, (
            "There should be {target} rows," "found: {current}".format(
                target=orig_rows * 2, current=new_rows
            ),
        )
```

It's not significantly different, the snippet ends up one line longer. However the thing that seemed most surprising in the ruff output was that the two strings weren't concatenated. To illustrate what I mean, if I run `black --preview` on the ruff output I get
```python
class TestIO:
    def test(self):
        new_rows, orig_rows = 1, 1

        assert new_rows == orig_rows * 2, (
            "There should be {target} rows,found: {current}".format(
                target=orig_rows * 2, current=new_rows
            ),
        )
```
I appreciate this must have extra complexity to be able to do, but it seems like it should be possible when both strings are the same regular/ raw / f string variant.

---

_Label `formatter` added by @MichaReiser on 2023-09-17 13:12_

---

_Label `needs-decision` added by @MichaReiser on 2023-09-17 13:12_

---

_Added to milestone `Formatter: Beta` by @MichaReiser on 2023-09-17 13:12_

---

_Comment by @MichaReiser on 2023-09-17 13:16_

Thanks for reporting. We plan to implement Black's preview style once we shipped the Beta.

Regarding the deviation. I believe it is related to https://github.com/astral-sh/ruff/issues/7052 We already implement custom handling for implicit strings but it is limited to binary and compare expressions [PR](https://github.com/astral-sh/ruff/pull/7145). We may also need custom handling for call expressions where the string part group also includes the group of the right side (to get left to right, instead of right to left expansion)

A more minified example (to ensure it's unrelated to `assert`):

```python
# black / existing formatting
class TestIO:
    def test(self):
        new_rows, orig_rows = 1, 1

        (
            "There should be {target} rows,"
            "found: {current}".format(target=orig_rows * 2, current=new_rows),
        )
```

---

_Comment by @m-richards on 2023-09-18 10:19_

Thanks for the quick response! This does look like it's the same or very similar to #7052, so I'd be happy to close this.

---

_Comment by @charliermarsh on 2023-09-27 15:59_

(Closing as it does appear to be the same as #7052. Thank you!)

---

_Closed by @charliermarsh on 2023-09-27 15:59_

---

_Label `needs-decision` removed by @charliermarsh on 2023-09-27 15:59_

---
