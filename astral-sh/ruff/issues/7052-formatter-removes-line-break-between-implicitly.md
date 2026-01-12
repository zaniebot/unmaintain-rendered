```yaml
number: 7052
title: Formatter removes line break between implicitly concatenated strings
type: issue
state: closed
author: cnpryer
labels:
  - documentation
  - formatter
assignees: []
created_at: 2023-09-01T20:19:25Z
updated_at: 2023-09-29T17:27:33Z
url: https://github.com/astral-sh/ruff/issues/7052
synced_at: 2026-01-12T15:54:46Z
```

# Formatter removes line break between implicitly concatenated strings

---

_@cnpryer_

Couldn't find an *exact* issue, but I'm almost certain this has been reported already. 

Maybe related to #6936
Also see #5893

Line-length: 88

Black (23.7.0):
```py
print(
    "aaaaaaa {:.2f}% "
    "aaaaaaaaaaaaaaaa".format(
        (
            ((df_aaaaaaaaaaa["fak"].isna()) | (df_aaaaaaaaaaa["fak"] == 0)).sum()
            / len(df_aaaaaaaaaaa)
        )
        * 100
    )
)
```

Ruff (0.0.287):
```py
print(
    "aaaaaaa {:.2f}% " "aaaaaaaaaaaaaaaa".format(
        (
            ((df_aaaaaaaaaaa["fak"].isna()) | (df_aaaaaaaaaaa["fak"] == 0)).sum()
            / len(df_aaaaaaaaaaa)
        )
        * 100
    )
)
```

Playground: https://play.ruff.rs/126e4315-3c75-4268-b43a-12586c5da779


---

_Renamed from "Formatter removes line break between string and string with format method" to "Formatter removes line break between implicitly concatenated strings with format method" by @cnpryer on 2023-09-01 20:21_

---

_Renamed from "Formatter removes line break between implicitly concatenated strings with format method" to "Formatter removes line break between implicitly concatenated strings" by @cnpryer on 2023-09-01 20:35_

---

_Label `formatter` added by @charliermarsh on 2023-09-01 22:59_

---

_Assigned to @MichaReiser by @MichaReiser on 2023-09-02 08:13_

---

_Comment by @MichaReiser on 2023-09-02 08:15_

Black applies special formatting for strings inside binary expressions. We have a "cheap" implementation that works for the most simple cases but it falls short in more complicated settings. I hope to have a fix for this by next week.

---

_Added to milestone `Formatter: Beta` by @MichaReiser on 2023-09-02 08:23_

---

_Unassigned @MichaReiser by @MichaReiser on 2023-09-27 13:30_

---

_Comment by @charliermarsh on 2023-09-27 14:57_

We're marking this as an intentional deviation, since the resulting code doesn't exceed line width and reduces vertical spacing. (If you make the strings longer, we _do_ split and format identically to Black.)

Can be closed once documented.

---

_Label `documentation` added by @MichaReiser on 2023-09-27 16:04_

---

_Assigned to @charliermarsh by @charliermarsh on 2023-09-27 18:44_

---

_Closed by @charliermarsh on 2023-09-29 17:27_

---
