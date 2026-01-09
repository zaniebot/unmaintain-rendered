---
number: 6818
title: "Formatter: Unstable call arguments comment formatting"
type: issue
state: closed
author: MichaReiser
labels:
  - bug
  - formatter
assignees: []
created_at: 2023-08-23T12:42:53Z
updated_at: 2023-08-25T04:06:58Z
url: https://github.com/astral-sh/ruff/issues/6818
synced_at: 2026-01-07T13:12:15-06:00
---

# Formatter: Unstable call arguments comment formatting

---

_Issue opened by @MichaReiser on 2023-08-23 12:42_

I created this example to test some edge cases with #6817. That PR is handling the edge case correctly but I can't add the test case because the call expression formatting with the following comment itself is unstable

```python
aaa = (
    bbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbb
    # awkward comment
    ()
    .bbbbbbbbbbbbbbbb
)


aaa = bbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbb# awkward comment
().bbbbbbbbbbbbbbbb
```

[Playground](https://play.ruff.rs/31db160f-3bd9-4c5d-939c-69e2819c16e2)

---

_Label `bug` added by @MichaReiser on 2023-08-23 12:42_

---

_Label `formatter` added by @MichaReiser on 2023-08-23 12:42_

---

_Added to milestone `Formatter: Beta` by @MichaReiser on 2023-08-23 12:42_

---

_Comment by @charliermarsh on 2023-08-23 16:52_

@MichaReiser - Want me to look at this, or are you taking as part of #6817?

---

_Comment by @MichaReiser on 2023-08-23 17:03_

> @MichaReiser - Want me to look at this, or are you taking as part of #6817?

I'm not solving it. I used a different test case to work around the issue.

---

_Comment by @charliermarsh on 2023-08-23 17:13_

I'm thinking that I'll just make that a leading comment on `bbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbb`.

---

_Comment by @MichaReiser on 2023-08-23 17:25_

Prettier makes it a leading comment of what comes after [Playground](https://prettier.io/playground/#N4Igxg9gdgLgprEAuEBDdACAvBgFAHSg2IwCNyLKrqba76HHbCSMB6NjVAdwGtvUAJwAmGSAFtxCGCxK4AlLOIA6JhULyQAGhAQADjACW0AM7JQQwRG4AFIQjMpUAGwEBPMztKDUYXnBgAZVQpABlDKDhkADMXEzgvHz8AwL1fCIBzZBhBAFcEkHjxQ2y8gpNM5zgARVyIeBi4goArEwAPQMqauoakWOd4nQBHHrgbKz1HNBMAWki4YQXtEBzUQ2dMgGEISVRkNGdnZYqoDKqAQRgcw1Jc+Bs4QXDIxoGCgAsYcWcAdXfDeAmNJgOCBBwAwwANwBbn2YBMnhAkPyAEkoItYIEwIJDAZzujAjA3FVXoMQHorPEfj49PsKXB4oJIVEdBFGTBxqgMuI9n0mjo0oJGftSKhSHAjgKcbAfoZhDB3sgABwABh0gjgI0MGs53N5-TJMDFsvliqQACYdLl4gAVMWOA0FODicXCRbCUKoU65LlwABiEEEPKumX2qDuEBAAF8o0A). You have to be careful with comment ordering if you make it a leading comment of `bbb`, e.g if `bbb` has some trailing comments too

---

_Comment by @charliermarsh on 2023-08-23 17:30_

I need to understand why we break the latter but not the former:

```python
aaa = (
    bbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbb
    # awkward comment
    ()
    .bbbbbbbbbbbbbbbb
)

aaa = (
    bbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbb
    ()
    # awkward comment
    .bbbbbbbbbbbbbbbb
)
```

---

_Comment by @MichaReiser on 2023-08-23 18:24_

Attribute formatting has special handling for leading comments of the next attribute 

https://github.com/astral-sh/ruff/blob/0cea4975fcfe09716c084c0a18d1b4c4cd9b8f05/crates/ruff_python_formatter/src/expression/expr_attribute.rs#L113-L117

---

_Assigned to @charliermarsh by @charliermarsh on 2023-08-23 21:08_

---

_Referenced in [astral-sh/ruff#6826](../../astral-sh/ruff/pulls/6826.md) on 2023-08-23 21:28_

---

_Closed by @charliermarsh on 2023-08-25 04:06_

---
