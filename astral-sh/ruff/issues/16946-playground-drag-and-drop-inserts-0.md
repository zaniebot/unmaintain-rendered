```yaml
number: 16946
title: "[playground] Drag-and-drop inserts `$0`"
type: issue
state: closed
author: InSyncWithFoo
labels:
  - playground
assignees: []
created_at: 2025-03-24T11:23:39Z
updated_at: 2025-03-24T17:09:48Z
url: https://github.com/astral-sh/ruff/issues/16946
synced_at: 2026-01-12T15:54:55Z
```

# [playground] Drag-and-drop inserts `$0`

---

_@InSyncWithFoo_

Steps to reproduce:

1. Open [the playground](https://playknot.ruff.rs/) in a new (browser) tab/window. Leave the editor empty.

2. Select everything in the following code block:

    ```python
    foo = 'bar'
    ```

3. Drag it and drop onto the editor.

4. The editor now has:

    ```python
    foo = 'bar'$0
    ```

---

_Comment by @MichaReiser on 2025-03-24 13:27_

We don't have any custom drag and drop handling and, indeed, this reproduces in [monaco's playground](https://microsoft.github.io/monaco-editor/playground.html?source=v0.52.2#XQAAAAJrAQAAAAAAAABBqQkHQ5NjdMjwa-jY7SIQ9S7DNlzs5W-mwj0fe1ZCDRFc9ws9XQE0SJE1jc2VKxhaLFIw9vEWSxW3yscwzcqiW_1uhs0-3YGTBOsVJ4S4FwfCDQb5-2uUHXTVAefbVSd9IzaZ_vv9VR9KugsKimARHSWlrTPOQHROITHxmAetfeRrm3VHdSe7_Etv6WpgzlH4vE8EOEEwk8H8IFxSmzgqwKQpjOsq0VT9OkSPNBU_af_MQo3128-5xBdX2FQesVrxFyKhiY0ZA2W4TXnVmqpqPvxJJYtioxwHrgSXJT1rfncR_EOq3oQYHWE8IrRYRxLCq4t0QMQ4Mc9YkRWUPC9fZZh_9sUkoANFS4ZDEEcw4LaTFlV-wj4SzR5se0Tu-zSLhxOA_ulRVw)

See https://github.com/microsoft/monaco-editor/issues/4386 I'll close this in favor of the upstream issue

---

_Closed by @MichaReiser on 2025-03-24 13:27_

---

_Label `playground` added by @AlexWaygood on 2025-03-24 17:09_

---
