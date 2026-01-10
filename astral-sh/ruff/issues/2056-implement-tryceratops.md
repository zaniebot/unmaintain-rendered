---
number: 2056
title: "Implement `tryceratops`"
type: issue
state: closed
author: sbrugman
labels:
  - plugin
assignees: []
created_at: 2023-01-21T11:54:14Z
updated_at: 2023-02-27T17:45:38Z
url: https://github.com/astral-sh/ruff/issues/2056
synced_at: 2026-01-10T01:22:40Z
---

# Implement `tryceratops`

---

_Issue opened by @sbrugman on 2023-01-21 11:54_

- [x] [TC002](https://github.com/guilatrova/tryceratops/blob/main/docs/violations/TC002.md): Create your own exception
- [x] [TC003](https://github.com/guilatrova/tryceratops/blob/main/docs/violations/TC003.md): Avoid specifying long messages outside the exception class
- [x] [TC004](https://github.com/guilatrova/tryceratops/blob/main/docs/violations/TC004.md): Prefer TypeError exception for invalid type
- [x] ~[TC100](https://github.com/guilatrova/tryceratops/blob/main/docs/violations/TC100.md): Check to continue~
- [x] ~[TC101](https://github.com/guilatrova/tryceratops/blob/main/docs/violations/TC101.md): Too many try blocks~
- [x] [TC200](https://github.com/guilatrova/tryceratops/blob/main/docs/violations/TC200.md): Use raise Exception from
- [x] [TC201](https://github.com/guilatrova/tryceratops/blob/main/docs/violations/TC201.md): Simply use raise
- [x] ~[TC202](https://github.com/guilatrova/tryceratops/blob/main/docs/violations/TC202.md): Don't ignore a broad exception without even logging~
- [x] [TC300](https://github.com/guilatrova/tryceratops/blob/main/docs/violations/TC300.md): Consider adding an else block
- [x] [TC301](https://github.com/guilatrova/tryceratops/blob/main/docs/violations/TC301.md): Avoid direct raises in try body
- [x] [TC400](https://github.com/guilatrova/tryceratops/blob/main/docs/violations/TC400.md): Use logging .exception instead of .error
- [x] [TC401](https://github.com/guilatrova/tryceratops/blob/main/docs/violations/TC401.md): Do not log the exception object


---

_Referenced in [astral-sh/ruff#2055](../../astral-sh/ruff/pulls/2055.md) on 2023-01-21 11:54_

---

_Referenced in [astral-sh/ruff#909](../../astral-sh/ruff/pulls/909.md) on 2023-01-21 16:26_

---

_Comment by @alonme on 2023-01-21 17:32_

Taking TC201

---

_Comment by @sbrugman on 2023-01-21 17:37_

For tracking: I'm working on TC004

---

_Referenced in [astral-sh/ruff#2066](../../astral-sh/ruff/pulls/2066.md) on 2023-01-21 18:33_

---

_Comment by @karpa4o4 on 2023-01-21 20:55_

@charliermarsh, i would like to do TC200.

---

_Comment by @charliermarsh on 2023-01-21 22:01_

Awesome -- thanks all! Will review as they come in :)

---

_Referenced in [astral-sh/ruff#2087](../../astral-sh/ruff/pulls/2087.md) on 2023-01-22 14:46_

---

_Label `plugin` added by @charliermarsh on 2023-01-22 22:08_

---

_Referenced in [astral-sh/ruff#2073](../../astral-sh/ruff/pulls/2073.md) on 2023-01-22 22:08_

---

_Comment by @karpa4o4 on 2023-01-23 19:26_

I'll take TC301 next.

---

_Referenced in [astral-sh/ruff#2113](../../astral-sh/ruff/pulls/2113.md) on 2023-01-23 19:55_

---

_Comment by @Flowake on 2023-01-23 20:27_

I'd like to implement TC400, which would be my first rule so it might take a few days. Would that be fast enough ?

---

_Comment by @charliermarsh on 2023-01-23 20:45_

@Flowake - Sure! Take your time. Feel free to open a draft PR and ask questions there if you run into any.

---

_Referenced in [astral-sh/ruff#2115](../../astral-sh/ruff/pulls/2115.md) on 2023-01-23 22:57_

---

_Comment by @karpa4o4 on 2023-01-24 13:41_

I'll take TC002 next.

---

_Referenced in [astral-sh/ruff#2135](../../astral-sh/ruff/pulls/2135.md) on 2023-01-24 16:35_

---

_Comment by @bigluck on 2023-01-26 21:18_

Ciao, I'm testing the new implementation, but I'm getting this error: `TRY300 Consider else block`

<img width="472" alt="Screenshot 2023-01-26 at 22 16 04" src="https://user-images.githubusercontent.com/1511095/214952122-ab8ea72f-51a7-400a-bd00-59f5eebd1427.png">

Isn't it a false positive?
Thanks so much

---

_Comment by @charliermarsh on 2023-01-26 21:21_

Hmm, comparing to the original plugin, I think what's incorrect is the highlighted region. It seems to want you to do this (omitting some intermediary lines since I had to copy from the screenshot :)):

```py
try:
    assert isinstance(value, list) is False
except Exception as exc:
    msg = ""
    raise EnsureStrException(msg)
else:
    return str(value)
```

---

_Referenced in [astral-sh/ruff#2228](../../astral-sh/ruff/pulls/2228.md) on 2023-01-26 21:36_

---

_Comment by @charliermarsh on 2023-01-26 21:36_

(Fixed in #2228.)

---

_Comment by @bigluck on 2023-01-26 21:53_

Superb, thanks!

---

_Comment by @jacopotagliabue on 2023-01-26 22:09_

@charliermarsh me and luca should buy you lunch in BKL - ping me on [linkedin](https://jacopotagliabue.it/) or whatever and I'm happy to buy you lunch. Thanks for all the great work!

---

_Comment by @colin99d on 2023-02-19 13:38_

Taking TC401 and TC202

---

_Referenced in [astral-sh/ruff#3036](../../astral-sh/ruff/pulls/3036.md) on 2023-02-19 17:53_

---

_Referenced in [astral-sh/ruff#3044](../../astral-sh/ruff/pulls/3044.md) on 2023-02-19 23:30_

---

_Referenced in [astral-sh/ruff#3109](../../astral-sh/ruff/pulls/3109.md) on 2023-02-22 03:11_

---

_Referenced in [astral-sh/ruff#3226](../../astral-sh/ruff/pulls/3226.md) on 2023-02-25 19:22_

---

_Closed by @charliermarsh on 2023-02-27 17:45_

---
