---
number: 4618
title: "False-positive ERA001 with `# nuitka-project:` configuration"
type: issue
state: closed
author: bersbersbers
labels: []
assignees: []
created_at: 2023-05-24T07:13:33Z
updated_at: 2023-05-24T20:31:51Z
url: https://github.com/astral-sh/ruff/issues/4618
synced_at: 2026-01-07T13:12:14-06:00
---

# False-positive ERA001 with `# nuitka-project:` configuration

---

_Issue opened by @bersbersbers on 2023-05-24 07:13_

https://github.com/Nuitka/Nuitka allows in-file configurations:

```python
# nuitka-project: --onefile
# nuitka-project: --onefile-windows-splash-screen-image={MAIN_DIRECTORY}/Splash-Screen.png

# Whatever this is obviously
print("Delaying startup by 10s...")
import time, tempfile, os
time.sleep(10)

# Use this code to signal the splash screen removal.
if "NUITKA_ONEFILE_PARENT" in os.environ:
   splash_filename = os.path.join(
      tempfile.gettempdir(),
      "onefile_%d_splash_feedback.tmp" % int(os.environ["NUITKA_ONEFILE_PARENT"]),
   )

   if os.path.exists(splash_filename):
      os.unlink(splash_filename)

print("Done... splash should be gone.")
...

# Rest of your program goes here.
```

I find lines starting with `# nuitka-project: ` should not be flagged as ERA001 (and not be removed by `--fix`).

(If there was an option to disable ERA001 for certain keywords, users could configure that themselves.)

---

_Comment by @bersbersbers on 2023-05-24 07:17_

Alternatively, being able to disable ERA001 for this block would also be an option. I know how to suppress it for a single line or the whole file, but not a block.

---

_Comment by @JonathanPlasse on 2023-05-24 08:33_

You can use [task-tags](https://beta.ruff.rs/docs/settings/#task-tags)
```toml
task-tags = ["nuitka-project"]
```

---

_Comment by @bersbersbers on 2023-05-24 11:33_

> You can use [task-tags](https://beta.ruff.rs/docs/settings/#task-tags)

This is great! It would be even greater if that was documented at https://beta.ruff.rs/docs/rules/commented-out-code/ and https://beta.ruff.rs/docs/rules/line-too-long/, then I would have found it without opening this issue :)

---

_Comment by @JonathanPlasse on 2023-05-24 12:36_

Would you be open to making a pull-request for this?

---

_Referenced in [astral-sh/ruff#4644](../../astral-sh/ruff/pulls/4644.md) on 2023-05-24 20:11_

---

_Comment by @bersbersbers on 2023-05-24 20:11_

@JonathanPlasse sure, see #4644.

---

_Closed by @charliermarsh on 2023-05-24 20:31_

---

_Comment by @charliermarsh on 2023-05-24 20:31_

Thank you!

---
