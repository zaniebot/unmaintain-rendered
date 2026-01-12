```yaml
number: 8530
title: Difference in multi-line str.format between ruff and black
type: issue
state: closed
author: dwyatte
labels:
  - formatter
assignees: []
created_at: 2023-11-06T22:07:46Z
updated_at: 2023-11-27T04:02:23Z
url: https://github.com/astral-sh/ruff/issues/8530
synced_at: 2026-01-12T15:54:48Z
```

# Difference in multi-line str.format between ruff and black

---

_@dwyatte_

Settings: Default ruff `0.1.3`

`black` formats multi-line strings by adding args to `str.format` on a newline

https://black.vercel.app/?version=stable&state=_Td6WFoAAATm1rRGAgAhARYAAAB0L-Wj4ACkAG9dAD2IimZxl1N_Wj9gaSn83r_MDRh3Sohxk5tq0YJ9PWyG6Cyohdz_PJoc7zUs17RxhRdCnJ7Npmq7PfdCxS68vrTxtZKdTAoQUsbG77GpIhJyuHq4tEKRNQcU7LDmXIq_Rno-hiLpkYbL4dUTP5aaAAAA1H7xhdfRFL8AAYsBpQEAAHaaNfGxxGf7AgAAAAAEWVo=

`ruff` keeps args to a multi-line `str.format` on a single line, presumably until it hits `line-length`

ruff
```
MULTI_LINE_STRING = """
a
b
c
{}
""".format("d")
```

black
```
MULTI_LINE_STRING = """
a
b
c
{}
""".format(
    "d"
)
```

This causes a cost in migrating to ruff in codebases that contain a large number of multi-line strings that get formatted

---

_Comment by @zanieb on 2023-11-06 22:31_

Thanks for the report.

I'm not sure I understand the logic in Black's formatting here.

Separately from Black compatibility, do you prefer that formatting?

Note that Black's preview style formats the same as Ruff so we are unlikely to change this.

---

_Label `formatter` added by @zanieb on 2023-11-06 22:31_

---

_Comment by @dwyatte on 2023-11-07 00:02_

> Note that Black's preview style formats the same as Ruff so we are unlikely to change this

Ah, so if I'm understanding correctly, this *might* get collected into Black's 2024 release anyway ([link for others](https://black.readthedocs.io/en/stable/the_black_code_style/future_style.html#improved-multiline-string-handling)). If it ends up not making the cut, would `ruff` continue to use the current one-line style for the call to `.format`?



---

_Comment by @zanieb on 2023-11-07 15:19_

That's a good question, I'm not sure. Often we would only implement the preview style over the stable style if it was a hard technical challenge. The inconsistency here is pretty weird, I can't think of other cases where a function call would be forced to break over multiple lines like that. It'd likely make it difficult for us to implement.

---

_Comment by @charliermarsh on 2023-11-07 15:51_

(Thanks for filing!) We tend not to fix deviations from Black if they are (1) a result of greater internal consistency in Ruff's formatter, and/or (2) already part of Black's preview style. This one feels like it meets both of those descriptions, so I think I'd categorize it as acceptable.

---

_Comment by @dwyatte on 2023-11-07 16:12_

> (2) already part of Black's preview style

Yep, was unaware of this. I think we can close this as I don't have a strong preference independent of compatibility

---

_Closed by @dwyatte on 2023-11-07 16:12_

---

_Comment by @charliermarsh on 2023-11-07 16:14_

Thanks @dwyatte, appreciate you taking the time to file these.

---

_Comment by @MichaReiser on 2023-11-27 04:02_

The reason for the difference is that Ruff already implements Black's [multiline-string preview style](https://black.readthedocs.io/en/stable/the_black_code_style/future_style.html#improved-multiline-string-handling) because it was more difficult to support the stable style.

---
