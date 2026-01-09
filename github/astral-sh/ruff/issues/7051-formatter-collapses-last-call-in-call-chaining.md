---
number: 7051
title: Formatter collapses last call in call chaining
type: issue
state: closed
author: cnpryer
labels:
  - documentation
  - formatter
assignees: []
created_at: 2023-09-01T20:07:51Z
updated_at: 2023-11-02T17:01:48Z
url: https://github.com/astral-sh/ruff/issues/7051
synced_at: 2026-01-07T13:12:15-06:00
---

# Formatter collapses last call in call chaining

---

_Issue opened by @cnpryer on 2023-09-01 20:07_

I prefer the change, but reporting since it's a diff in a from-`black` migration.

Line-length: 88

Related: #5343

Black (23.7.0):
```py
df.drop(
    columns=["aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa"]
).drop_duplicates().rename(
    columns={
        "a": "a",
    }
).to_csv(
    path / "aaaaaa.csv", index=False
)
```

Ruff (0.0.287):
```py
df.drop(
    columns=["aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa"]
).drop_duplicates().rename(
    columns={
        "a": "a",
    }
).to_csv(path / "aaaaaa.csv", index=False)
```

Counter example
```python
df.drop(
    columns=["aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa"]
).drop_duplicates(a).rename(
    columns={
        "a": "a",
    }
).to_csv(
    path / "aaaaaa.csv", index=False
).other(a)
```

Playground: https://play.ruff.rs/02424913-15b0-4703-8eb8-6bc5896b00b6

---

_Renamed from "Formatter removes last expanded call in call chaining" to "Formatter un-expands last call in call chaining" by @cnpryer on 2023-09-01 20:35_

---

_Label `formatter` added by @charliermarsh on 2023-09-01 22:59_

---

_Comment by @MichaReiser on 2023-09-02 08:18_

Black uses Ruff's formatting in preview style if the left is a multiline string but it seems they keep the old formatting otherwise [playground](https://black.vercel.app/?version=stable&state=_Td6WFoAAATm1rRGAgAhARYAAAB0L-Wj4AFYAL9dAD2IimZxl1N_WlbvtxhHIP4MzK-CKGFKTx4PoxaHmn8Dmof37FGcjOIX-bz5K3YuD1k254NkYXInS24C_6UBPyDJgSGApL6casEi4Z7wrUCGFKEo7Q-MBvAQ33945CQCKbk7SRrMV9n-OwmV81tNymNFU19Ggiw41EygIA_Q7eokwPqJZmNitGQH3G3zY9DlJaPCkTZZX-atlL7psySvTc8uJ_eolTpD9JOn6d__xVQJ6kixksOyuwNvmmnL6BGBAACOtjZboj1_AAAB2wHZAgAA_DMU3rHEZ_sCAAAAAARZWg==)

To me it's unclear if expanding the last call is intentional to improve readability. I'm inclined to keep Ruff's formatting because it achieves the goal of avoiding unnecessary horizontal lines but it depends on how frequent we run into this difference (although could be hard to fix)

---

_Label `needs-decision` added by @MichaReiser on 2023-09-02 08:18_

---

_Added to milestone `Formatter: Beta` by @MichaReiser on 2023-09-02 08:18_

---

_Comment by @cnpryer on 2023-09-02 12:49_

This was not frequent for me, but tbh I'm curious why. The pattern looks like it could be in some exploratory scripts we have.

I'll dig more next week.

---

_Removed from milestone `Formatter: Beta` by @MichaReiser on 2023-09-27 14:50_

---

_Added to milestone `Formatter: Stable` by @MichaReiser on 2023-09-27 14:50_

---

_Comment by @charliermarsh on 2023-09-27 14:52_

We're going to mark this as a known deviation for now. We'll revisit based on user feedback, but it's not blocking for the beta. (We can close the issue itself for now once this is documented.)

---

_Label `needs-decision` removed by @charliermarsh on 2023-09-27 14:52_

---

_Label `documentation` added by @MichaReiser on 2023-09-27 16:04_

---

_Assigned to @charliermarsh by @charliermarsh on 2023-09-27 18:50_

---

_Referenced in [astral-sh/ruff#7767](../../astral-sh/ruff/pulls/7767.md) on 2023-10-02 21:36_

---

_Closed by @charliermarsh on 2023-10-02 21:46_

---

_Renamed from "Formatter un-expands last call in call chaining" to "Formatter collapses last call in call chaining" by @cnpryer on 2023-11-02 17:01_

---
