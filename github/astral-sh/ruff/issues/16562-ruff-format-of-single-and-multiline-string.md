---
number: 16562
title: ruff format of single and multiline string different than in black
type: issue
state: closed
author: matejsp
labels:
  - question
  - formatter
assignees: []
created_at: 2025-03-08T05:42:42Z
updated_at: 2025-03-08T17:48:15Z
url: https://github.com/astral-sh/ruff/issues/16562
synced_at: 2026-01-07T13:12:16-06:00
---

# ruff format of single and multiline string different than in black

---

_Issue opened by @matejsp on 2025-03-08 05:42_

### Summary

When formatting our code I noticed two differences that I couldn't find in docs that they should be different from black.

CASE 1:
Formating of multiline string with format statement
```
        sql = """
            SELECT * FROM transaction WHERE
            AND transaction.id >= %(id_min)s
            AND transaction.id <= %(id_max)s
        """.format(
            id_min=1, id_max=5
        )
```

vs
ruff
```
        sql = """
            SELECT * FROM transaction WHERE
            AND transaction.id >= %(id_min)s
            AND transaction.id <= %(id_max)s
        """.format(id_min=1, id_max=5)
```

CASE 2:
However for single line it is the exact oposite

```
    assert (
        'Skipping command for transferring between accounts (transfer_config={}).'
        ' Switch "enable_transfers" is not active.'.format(transfer_config)
        in caplog.text
    )
```

black
```
    assert (
        'Skipping command for transferring between accounts (transfer_config={}).'
        ' Switch "enable_transfers" is not active.'.format(transfer_config) in caplog.text
    )
```



### Version

0.9.7

---

_Comment by @MichaReiser on 2025-03-08 17:48_

Hi @matejsp Thanks for opening this issue. Both these cases are known and intentional deviations:

Case 1 is a known deviation and is documented [here](https://docs.astral.sh/ruff/formatter/black/#call-chain-calls-break-differently-in-some-cases)


Case 2 is a known deviation and is documented [here](https://docs.astral.sh/ruff/formatter/black/#implicit-string-concatenations-in-attribute-accesses)




---

_Closed by @MichaReiser on 2025-03-08 17:48_

---

_Label `question` added by @MichaReiser on 2025-03-08 17:48_

---

_Label `formatter` added by @MichaReiser on 2025-03-08 17:48_

---
