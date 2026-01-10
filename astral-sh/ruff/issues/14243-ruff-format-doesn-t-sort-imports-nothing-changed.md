```yaml
number: 14243
title: "Ruff format doesn't sort imports, nothing changed (E402)"
type: issue
state: closed
author: zakimimit
labels:
  - question
assignees: []
created_at: 2024-11-10T09:48:07Z
updated_at: 2024-11-11T09:50:10Z
url: https://github.com/astral-sh/ruff/issues/14243
synced_at: 2026-01-10T11:09:56Z
```

# Ruff format doesn't sort imports, nothing changed (E402)

---

_Issue opened by @zakimimit on 2024-11-10 09:48_

Hello 

Ruff format doesn't sort imports, nothing changed

I use Vscode Extenstion Ruff v2024.54.0

ruff.toml

```
[lint]
extend-select = ["I","N","F","E","W","R","C90","UP","SIM","E402"]

extend-ignore = ["E501"]

ignore = ["E501","F403","F405"]


```

This does not work 
Module-level import not at top of file
Module-import-not-at-top-of-file (E402)

I try several times, nothing works

![image](https://github.com/user-attachments/assets/9d11c8c8-4708-461c-9409-16dcdd9593c3)

![image](https://github.com/user-attachments/assets/75426107-b94a-4b46-8eff-72bf402762c2)

I hope you can fix it 






---

_Comment by @charliermarsh on 2024-11-10 15:58_

Not all errors are auto-fixable. Can you be more specific about what you're trying to do?

---

_Label `question` added by @charliermarsh on 2024-11-10 15:58_

---

_Comment by @zakimimit on 2024-11-10 17:04_

> Not all errors are auto-fixable. Can you be more specific about what you're trying to do?

For example 

```
from flask_wtf import FlaskForm
from wtforms import (
    SubmitField,
)

class SetPriceForm(FlaskForm):
    submit = SubmitField("Set Prices")


import pyotp

class SeForm(FlaskForm):
    submit = SubmitField("Set ")

    def __init__(self, *args, **kwargs):
        super().__init__(*args, **kwargs)
        if not self.is_submitted():
            self.auth_key.data = pyotp.random_base32()

```


The import pyotp is not fixed 
It should be fixed  and go up with othor import 

Like this 
```

import pyotp

from flask_wtf import FlaskForm
from wtforms import (
    SubmitField,
)
```


---

_Comment by @MichaReiser on 2024-11-11 09:50_

The reason this isn't working as expected is because `ruff` doesn't support the `float-to-top` isort options. We track that feature in https://github.com/astral-sh/ruff/issues/1251

---

_Closed by @MichaReiser on 2024-11-11 09:50_

---
