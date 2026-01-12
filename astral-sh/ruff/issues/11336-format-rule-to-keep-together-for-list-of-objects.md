```yaml
number: 11336
title: "Format rule  to keep together }, { for list of objects"
type: issue
state: closed
author: darowny
labels:
  - question
assignees: []
created_at: 2024-05-08T12:00:23Z
updated_at: 2024-05-08T12:19:19Z
url: https://github.com/astral-sh/ruff/issues/11336
synced_at: 2026-01-12T15:54:51Z
```

# Format rule  to keep together }, { for list of objects

---

_@darowny_

Is there a setting or a rule that will keep this style of formatting ? I mean keeping `}, {` together and then hugging `}]` at the end?
```
trans = [{
    'first': Stuff,
    'second': Other,
    'third': Blah,
    'fourth': Sth
}, {
    'first': Stuff,
    'second': Other,
    'third': Blah,
    'fourth': Sth
}, {
    'first': Stuff,
    'second': Other,
    'third': Blah,
    'fourth': Sth
}]
```

I tried 0.3.4 and 0.4.3 ruff for this, with enabled preview, shouldn't it keep hugging braces and brackets ? 
[Related discussion I think](https://github.com/astral-sh/ruff/issues/8279)

---

_Renamed from "Format rule for keeping " to "Format rule  to keep together }, { for list of objects" by @darowny on 2024-05-08 12:00_

---

_Label `question` added by @MichaReiser on 2024-05-08 12:14_

---

_Comment by @MichaReiser on 2024-05-08 12:19_

Hi @darowny 

No, there's no such formatter setting. 

The hugging preview style only applies if the array (or dict or set) is the only argument in a function call or the only content of a prenthesized expression

```python
trans = call([
    {
        "first": Stuff,
        "second": Other,
        "third": Blah,
        "third": Blah,
    }
])


trans = a + ([
    {
        "first": Stuff,
        "second": Other,
        "third": Blah,
        "third": Blah,
    }
])

```

---

_Closed by @MichaReiser on 2024-05-08 12:19_

---
