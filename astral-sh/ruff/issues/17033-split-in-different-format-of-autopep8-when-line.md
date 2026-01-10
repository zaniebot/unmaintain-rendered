```yaml
number: 17033
title: split in different format of autopep8 when line length on column 80
type: issue
state: closed
author: dlintw
labels:
  - formatter
  - needs-info
assignees: []
created_at: 2025-03-28T09:12:21Z
updated_at: 2025-04-04T09:59:25Z
url: https://github.com/astral-sh/ruff/issues/17033
synced_at: 2026-01-10T11:09:58Z
```

# split in different format of autopep8 when line length on column 80

---

_Issue opened by @dlintw on 2025-03-28 09:12_

### Summary

a.py 
```
if true:
    if true:
        if ture:
            if true:
                print(f"Debug:{inspect.currentframe().f_lineno}: {log_message}")
```
the result of `autopep8 -i a.py` and `ruff format .` are different, autopep8 can use the column 80 to put last char.

* autopep8 version: 2.3.2(pycodestyle:2.12.1)

### Version

ruff 0.11.2

---

_Label `formatter` added by @MichaReiser on 2025-03-28 15:16_

---

_Label `needs-info` added by @MichaReiser on 2025-03-28 15:16_

---

_Comment by @MichaReiser on 2025-03-28 15:16_

Can you share more details of what behavior you'd expect and what formatter settings you use?

---

_Comment by @dlintw on 2025-04-03 03:05_

expect the same result as autopep8, in ruff, it treat

```
print(f"Debug:{inspect.currentframe().f_lineno}: {log_message}")
```

as 

```
print(
    f"Debug:{inspect.currentframe().f_lineno}: {log_message}" )
```


---

_Comment by @MichaReiser on 2025-04-03 06:53_

It does get formatted to

```py
if true:
    if true:
        if ture:
            if true:
                print(
                    f"Debug:{inspect.currentframe().f_lineno}: {log_message}"
                )

``` 

If you change the line length to 79 (your example exactly fits into the 80 limit)

https://play.ruff.rs/83e21204-c02a-44c3-9249-822842dadb6c

---

_Comment by @dlintw on 2025-04-04 09:59_

I've tried again on my another machine. it works. I think it's my fault,  my ruff.toml is

```
line-length = 80
```

---

_Closed by @dlintw on 2025-04-04 09:59_

---
