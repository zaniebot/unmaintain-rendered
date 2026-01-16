```yaml
number: 2531
title: "Autocompletion for Polars/Pandas Data Frame Column names, similar to how Pycharm & Data Lab IDE's provides"
type: issue
state: open
author: PGupta-Git
labels:
  - server
  - wish
  - completions
assignees: []
created_at: 2026-01-16T10:27:10Z
updated_at: 2026-01-16T21:43:11Z
url: https://github.com/astral-sh/ty/issues/2531
synced_at: 2026-01-16T22:14:35Z
```

# Autocompletion for Polars/Pandas Data Frame Column names, similar to how Pycharm & Data Lab IDE's provides

---

_@PGupta-Git_

### Question

I am looking for a solution to the following. I am not sure if it already exists or if I need to turn on a setting: Python Autocompletion for Polars/Pandas Data Frame Column names, similar to how PyCharm & Data Lab IDEs provide autocompletion for column names

Can't seem to trigger it. Happy to share more details if it helps. As you can see, the dataframe is loaded in memory.

<img width="2140" height="1481" alt="Image" src="https://github.com/user-attachments/assets/60a2b3db-c777-45ae-8c1e-e1049b639a04" />

It only works when using square brackets and trying to access it as dict

<img width="2140" height="1481" alt="Image" src="https://github.com/user-attachments/assets/822d1d23-174a-45ec-873a-23bacc7739cb" />

### Version

2026.4.0

---

_Label `question` added by @PGupta-Git on 2026-01-16 10:27_

---

_Renamed from "I am looking for a solution to the following. I am not sure if it already exists or if I need to turn on a setting: Python Autocompletion for Polars/Pandas Data Frame Column names, similar to how Pycharm & Data Lab IDE's provides" to "Autocompletion for Polars/Pandas Data Frame Column names, similar to how Pycharm & Data Lab IDE's provides" by @AlexWaygood on 2026-01-16 10:32_

---

_Label `server` added by @AlexWaygood on 2026-01-16 10:32_

---

_Label `completions` added by @AlexWaygood on 2026-01-16 10:32_

---

_Comment by @MichaReiser on 2026-01-16 11:16_

That's fancy. So PyCharm goes and reads the CSV file to extract the column names. 

---

_Label `question` removed by @MichaReiser on 2026-01-16 11:16_

---

_Label `wish` added by @MichaReiser on 2026-01-16 11:16_

---

_Comment by @PGupta-Git on 2026-01-16 11:27_

I think PyCharm extracts the column names from the in-memory dataframe. Please check this https://github.com/posit-dev/positron/issues/5436

---

_Comment by @dangotbanned on 2026-01-16 21:39_

> That's fancy. So PyCharm goes and reads the CSV file to extract the column names.

@MichaReiser I'm not a PyCharm user, but I would've expected they'd be getting the names from [`_ipython_key_completions_`](https://github.com/pola-rs/polars/blob/b894f05bc0ad454e06a78b369c6b0f72cd832075/py-polars/src/polars/dataframe/frame.py#L1603-L1604)

VSCode makes use of it, IIRC only in notebooks, but the behavior extends to anything class that defines the hook + `dict`

---
