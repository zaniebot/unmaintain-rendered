```yaml
number: 6852
title: "uv fails to compile proper `--universal` txt file with `openai-whisper`"
type: issue
state: closed
author: stasbel
labels:
  - question
assignees: []
created_at: 2024-08-30T03:27:42Z
updated_at: 2024-09-04T02:49:21Z
url: https://github.com/astral-sh/uv/issues/6852
synced_at: 2026-01-12T15:59:08Z
```

# uv fails to compile proper `--universal` txt file with `openai-whisper`

---

_@stasbel_

**platform: macos**
**version: 0.4.0**

openai-whisper has following dependency:
```
triton>=2.0.0,<3;platform_machine=="x86_64" and sys_platform=="linux" or sys_platform=="linux2"
```
however, running `uv pip compile` with `--universal` on macos does't preserve platform tags
what you can see in the output is plain:
```
triton==2.0.0
```
this results in inability to install packages on macos


---

_Comment by @charliermarsh on 2024-08-30 12:47_

Do you mind including the full command you ran to reproduce?

---

_Label `question` added by @charliermarsh on 2024-08-30 12:47_

---

_Comment by @stasbel on 2024-08-30 19:48_

sure

**reqs.in**
```
openai-whisper==20231117
```

**command**
```
uv pip compile --no-strip-extras --universal reqs.in -o reqs.txt
```

---

_Comment by @stasbel on 2024-08-30 19:51_

if I insert platform tags to .txt file manually after, everything works fine :)

---

_Comment by @charliermarsh on 2024-09-04 02:45_

I think this is an issue with `openai-whisper`. See: https://github.com/openai/whisper/blob/ba3f3cd54b0e5b8ce1ab3de13e32122d0d5f98ab/setup.py#L15, which does:

```python
requirements = []
if sys.platform.startswith("linux") and platform.machine() == "x86_64":
    requirements.append("triton>=2.0.0,<3")
```

They should apply those constraints as environment markers, they shouldn't depend on the host machine... I can file a PR.

---

_Comment by @charliermarsh on 2024-09-04 02:47_

Actually confused because that variable isn't even used.

---

_Comment by @charliermarsh on 2024-09-04 02:49_

Ah ok, unfortunately this change is unreleased: https://github.com/openai/whisper/pull/1887. You'll have to use:

```
openai-whisper @ git+https://github.com/openai/whisper.git@ba3f3cd54b0e5b8ce1ab3de13e32122d0d5f98ab
```

In order to access it.

---

_Closed by @charliermarsh on 2024-09-04 02:49_

---
