```yaml
number: 12044
title: Script to convert Pipefile to pyproject.toml with uv tool sections
type: issue
state: closed
author: andrader
labels:
  - question
assignees: []
created_at: 2025-03-07T13:55:09Z
updated_at: 2025-03-09T17:44:22Z
url: https://github.com/astral-sh/uv/issues/12044
synced_at: 2026-01-12T16:00:53Z
```

# Script to convert Pipefile to pyproject.toml with uv tool sections

---

_@andrader_

### Question

Hi there! 

I just want to share a script ([convert_pipfile_to_uv.py](https://gist.github.com/andrader/a020beb04f4366eff14aec12fb26d75a)) to convert Pipefile to pyproject.toml with uv tool sections etc. It is based on this [script](https://gist.github.com/arnaldojvg/9b845a5595cd39b3f18d09abf591df8b) with some minor changes.

I don't know if uv has some native feature to do that or if there is another better alternative please let me know.

Do you guys think uv should have a similar feature for that in the future? Just to show the importance that: at my company with have a huge amount of applications using pipenv and a native uv feature would be safer and straightforward to use.

### Platform

_No response_

### Version

_No response_

---

_Label `question` added by @andrader on 2025-03-07 13:55_

---

_Comment by @andrader on 2025-03-09 17:44_

Found [migrate-to-uv](https://github.com/mkniewallner/migrate-to-uv):

Just run `uvx migrate-to-uv`.

---

_Closed by @andrader on 2025-03-09 17:44_

---
