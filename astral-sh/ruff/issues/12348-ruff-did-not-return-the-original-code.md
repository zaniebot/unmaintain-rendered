---
number: 12348
title: ruff did not return the original code
type: issue
state: closed
author: ArtemIsmagilov
labels: []
assignees: []
created_at: 2024-07-16T15:46:27Z
updated_at: 2024-07-16T15:58:21Z
url: https://github.com/astral-sh/ruff/issues/12348
synced_at: 2026-01-10T01:22:52Z
---

# ruff did not return the original code

---

_Issue opened by @ArtemIsmagilov on 2024-07-16 15:46_

ruff version 0.5.2
OS ubuntu

I have python code
```python
self.create_empty_buttons("btn_task1", "btn_task2", "btn_task3", "btn_task4", "btn_task5", "btn_task6")
```
After command ruff formatting code and create many nested arguments
going ruff with line-length=80
```bash
ruff ruff format . --line-length=80
>>>self.create_empty_buttons(
            "btn_task1",
            "btn_task2",
            "btn_task3",
            "btn_task3",
            "btn_task3",
            "btn_task3",
        )
```
 I'm trying to remove this nesting that fits in 120 characters, but ruff does not make the code initially horizontal without nesting. Why?
 ```bash
 ruff ruff format . --line-length=120
>>>???self.create_empty_buttons(
            "btn_task1",
            "btn_task2",
            "btn_task3",
            "btn_task3",
            "btn_task3",
            "btn_task3",
        )
 ```

---

_Comment by @ArtemIsmagilov on 2024-07-16 15:49_

It would be expected for me remove nested args for example
```python
>>>self.create_empty_buttons("btn_task1", "btn_task2", "btn_task3", "btn_task4", "btn_task5", "btn_task6")
```

---

_Comment by @ArtemIsmagilov on 2024-07-16 15:51_

I may not know much about whether this is semantically correct and whether this is expected by many developers. I just want to know why exactly this return is ignored, given that it fits in 120 characters.

---

_Comment by @charliermarsh on 2024-07-16 15:51_

But you ran with `--line-length=80`, and the line is over 80 characters.

---

_Comment by @charliermarsh on 2024-07-16 15:52_

Oh, and then you re-ran with `--line-length=120` _on the formatted output_, is that right? By default Ruff will respect trailing commas and won't collapse statements. You can configure that here: https://docs.astral.sh/ruff/settings/#format_skip-magic-trailing-comma

---

_Comment by @charliermarsh on 2024-07-16 15:52_

Or just remove the trailing comma from your example.

---

_Comment by @ArtemIsmagilov on 2024-07-16 15:56_

Hi, @charliermarsh, thank you. Remove `,` in end. ruff returned the lines as expected)
It seems I missed this point because you have it in the documentation. Thanks again)

Result now after remove  `,` in end args
```python
>>>self.create_empty_buttons("btn_task1", "btn_task2", "btn_task3", "btn_task4", "btn_task5", "btn_task6")
```

---

_Closed by @ArtemIsmagilov on 2024-07-16 15:56_

---

_Comment by @charliermarsh on 2024-07-16 15:58_

No problem! Glad to help.

---
