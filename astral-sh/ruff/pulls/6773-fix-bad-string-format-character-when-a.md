```yaml
number: 6773
title: "Fix `bad-string-format-character` when a placeholder occurs within a format spec part"
type: pull_request
state: closed
author: zanieb
labels:
  - bug
assignees: []
base: main
head: fix/format-spec-placeholder
created_at: 2023-08-22T15:15:20Z
updated_at: 2023-08-24T19:41:10Z
url: https://github.com/astral-sh/ruff/pull/6773
synced_at: 2026-01-12T15:55:22Z
```

# Fix `bad-string-format-character` when a placeholder occurs within a format spec part

---

_@zanieb_

Closes #6767 

The presented false positive `"{0:.{prec}g}".format(1.23, prec=15)` occurs because we treated the entire format specification of `.{prec}g` as the format type (which is typically at the end of a format specification). The nested replacement was not detected because a leading `{` was not seen. Instead, `.` remained the leading character — it's the leading character for a precision clause but was not consumed since the replacement was not a valid number

One way to resolve this is to update parsing of every clause in a format specification to be replacement aware e.g. precision parsing would accept a replacement as well as a number. I determined that this is too complicated and provides little value for our current rules around format specifications.

Instead, after we've parsed most of the format specification parts, we will check for any remaining replacements (i.e. the presence of `{` anywhere in the remaining string) and consume them and any intermediate characters. This matches our existing intention of basically ignoring any replacements while retaining as much of the normal format specification linting as we can.

---

_Label `bug` added by @zanieb on 2023-08-22 15:17_

---

_@zanieb reviewed on 2023-08-22 15:24_

---

_Review comment by @zanieb on `crates/ruff_python_literal/src/format.rs`:339 on 2023-08-22 15:24_

The following implementation is a little clunky. I'm guessing there's a more idiomatic way to do this?

---

_Comment by @github-actions[bot] on 2023-08-22 15:37_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.01      3.4±0.01ms    11.9 MB/sec    1.00      3.4±0.02ms    12.0 MB/sec
formatter/numpy/ctypeslib.py               1.00    709.0±2.82µs    23.5 MB/sec    1.00    712.1±2.61µs    23.4 MB/sec
formatter/numpy/globals.py                 1.00     74.0±0.34µs    39.9 MB/sec    1.00     74.2±0.37µs    39.8 MB/sec
formatter/pydantic/types.py                1.00  1384.4±15.60µs    18.4 MB/sec    1.02  1411.7±21.69µs    18.1 MB/sec
linter/all-rules/large/dataset.py          1.00     10.2±0.06ms     4.0 MB/sec    1.00     10.2±0.06ms     4.0 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      2.8±0.05ms     6.0 MB/sec    1.00      2.8±0.04ms     6.1 MB/sec
linter/all-rules/numpy/globals.py          1.01    391.1±0.65µs     7.5 MB/sec    1.00    387.4±0.87µs     7.6 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.3±0.04ms     4.8 MB/sec    1.00      5.3±0.01ms     4.8 MB/sec
linter/default-rules/large/dataset.py      1.00      5.4±0.01ms     7.5 MB/sec    1.01      5.4±0.01ms     7.5 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1211.5±7.77µs    13.7 MB/sec    1.00   1208.9±1.38µs    13.8 MB/sec
linter/default-rules/numpy/globals.py      1.01    139.3±3.56µs    21.2 MB/sec    1.00    138.5±0.57µs    21.3 MB/sec
linter/default-rules/pydantic/types.py     1.00      2.5±0.02ms    10.3 MB/sec    1.00      2.5±0.01ms    10.3 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      3.7±0.05ms    11.1 MB/sec    1.03      3.8±0.19ms    10.8 MB/sec
formatter/numpy/ctypeslib.py               1.00   734.6±19.75µs    22.7 MB/sec    1.00   734.6±11.14µs    22.7 MB/sec
formatter/numpy/globals.py                 1.00     75.7±1.11µs    39.0 MB/sec    1.01     76.4±3.34µs    38.6 MB/sec
formatter/pydantic/types.py                1.00  1523.7±39.36µs    16.7 MB/sec    1.00  1521.9±23.39µs    16.8 MB/sec
linter/all-rules/large/dataset.py          1.00     13.1±0.64ms     3.1 MB/sec    1.02     13.4±0.37ms     3.0 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.7±0.25ms     4.5 MB/sec    1.00      3.7±0.13ms     4.5 MB/sec
linter/all-rules/numpy/globals.py          1.02   398.5±36.73µs     7.4 MB/sec    1.00   389.5±11.80µs     7.6 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.9±0.27ms     3.7 MB/sec    1.03      7.1±0.29ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.02      7.4±0.18ms     5.5 MB/sec    1.00      7.2±0.19ms     5.6 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.02  1530.8±45.17µs    10.9 MB/sec    1.00  1508.1±34.97µs    11.0 MB/sec
linter/default-rules/numpy/globals.py      1.00    152.2±2.96µs    19.4 MB/sec    1.04    157.9±4.91µs    18.7 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.3±0.15ms     7.6 MB/sec    1.00      3.3±0.11ms     7.7 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_@charliermarsh reviewed on 2023-08-23 03:28_

So I'm playing around with this a bit, and I'm starting to wonder if attempting to parse these format strings (when nested replacements are present) is sound.

Consider `FormatSpec::parse("{a}30")`. In this case, `30` could _either_ be the width _or_ the precision depending on whether `a` is `>` or `.`. So assigning it to the width (as we do now) is incorrect.

Similarly, for `FormatSpec::parse("{a}s")`, this could _either_ resolve to `!s` or `:s` depending on the value of `a`, and so treating `s` as either the format type _or_ the conversion would be wrong.

In general, I just don't know that we can soundly analyze these when the contents are dynamic, which leads me to think we should use a different representation for them and avoid raising errors for them... What do you think?

---

_Comment by @MichaReiser on 2023-08-23 06:34_

@charliermarsh are you saying that the format string gets evaluated at runtime? Like, `a` can contain a partial format spec and the rest is a static string? 

---

_Comment by @charliermarsh on 2023-08-23 13:27_

@MichaReiser - Yes that's correct.

---

_Comment by @zanieb on 2023-08-23 13:45_

>  I just don't know that we can soundly analyze these when the contents are dynamic, which leads me to think we should use a different representation for them and avoid raising errors for them

I do think that we have a possibility of false diagnostics with the current approach, but it seems better than giving up entirely. I don't think replacements are used to replace large parts of the format specification in practice and are typically used for values rather than clause identifiers.

I could be convinced to just go to my initial approach which was an entirely different data structure. If we're not going to bother writing any diagnostics though we might as well just add a `FormatSpecError` type and just give up when we see a replacement.

---

_Comment by @tdulcet on 2023-08-24 09:07_

(I am the original reporter of this bug in: https://github.com/astral-sh/ruff/issues/6442#issuecomment-1686440347.)



> Similarly, for `FormatSpec::parse("{a}s")`, this could _either_ resolve to `!s` or `:s` depending on the value of `a`, and so treating `s` as either the format type _or_ the conversion would be wrong.

Maybe I am missing something here, but it seems the colon or exclamation mark must be included statically in the format string, as otherwise it produces a ValueError:
```
>>> "{{a}s}".format("test", a=":")
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
ValueError: Single '}' encountered in format string
>>> "{{a}s}".format("test", a="!")
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
ValueError: Single '}' encountered in format string
```
Adding the implicit zero does not make it work either:
```
>>> "{0{a}s}".format("test", a=":")
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
ValueError: unexpected '{' in field name
>>> "{0{a}s}".format("test", a="!")
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
ValueError: unexpected '{' in field name
```

---

_Comment by @charliermarsh on 2023-08-24 13:29_

@tdulcet - Sorry, you're right, it seems that it doesn't behave that way in that case. The first example does hold though (of width vs. precision), in my retesting.

---

_Closed by @zanieb on 2023-08-24 19:41_

---
