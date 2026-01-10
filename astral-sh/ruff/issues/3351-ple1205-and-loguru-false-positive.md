---
number: 3351
title: "PLE1205 and loguru \"false positive\""
type: issue
state: closed
author: OMEGARAZER
labels:
  - bug
  - type-inference
assignees: []
created_at: 2023-03-05T20:18:55Z
updated_at: 2023-09-01T00:36:47Z
url: https://github.com/astral-sh/ruff/issues/3351
synced_at: 2026-01-10T01:22:42Z
---

# PLE1205 and loguru "false positive"

---

_Issue opened by @OMEGARAZER on 2023-03-05 20:18_

Not sure if this is something that can actually be done, but I figured it might be worth mentioning, in case someone else runs into this as it took me a bit (longer than it should have anyway) to realize that the check seems specific to the base logging module.

It can obviously be set as an ignore but I think it would be great if Ruff could detect in the PLE1205 check that [Loguru](https://github.com/Delgan/loguru) is being used and ignore the lines or check that they are properly formatted for the module that's being used.


```python
from loguru import logger

filename = "test_file.txt"
directory = "/tmp"
logger.info("Downloaded {} to {} successfully", filename, directory)
```
```bash
ruff --version
ruff 0.0.254

ruff test.py --select=PLE
test.py:5:1: PLE1205 Too many arguments for `logging` format string
  |
5 | logger.info("Downloaded {} to {} successfully", filename, directory)
  | ^^^^^^^^^^^ PLE1205
  |

Found 1 error.
```

---

_Comment by @charliermarsh on 2023-03-05 20:22_

Oh interesting. We could ignore `loguru` there. We generally match on variable names like `logger`, etc.

---

_Label `bug` added by @charliermarsh on 2023-03-05 20:23_

---

_Comment by @OMEGARAZER on 2023-03-05 20:27_

Yeah, that's what I figured was happening but not knowing rust well I didn't dig into the inner workings too far to verify.

---

_Label `type-inference` added by @charliermarsh on 2023-03-06 23:34_

---

_Comment by @actionless on 2023-03-11 07:08_

i have same problem, not related to `loguru`, just seems `ruff` not support new-style (`{}`) formatting inside logging func

in pylint itself that could be configured as:

```
[LOGGING]
logging-modules=logging
logging-format-style=new
```

---

_Comment by @charliermarsh on 2023-03-12 05:27_

Mmm yeah, we're likely not supporting that properly. Is that part of the standard library `logging` module, or something else? Feel like I'm being dumb:

```py
>>> logging.info("foo: {}", "foo")
--- Logging error ---
Traceback (most recent call last):
  File "/Users/crmarsh/.local/share/rtx/installs/python/3.10.10/lib/python3.10/logging/__init__.py", line 1100, in emit
    msg = self.format(record)
  File "/Users/crmarsh/.local/share/rtx/installs/python/3.10.10/lib/python3.10/logging/__init__.py", line 943, in format
    return fmt.format(record)
  File "/Users/crmarsh/.local/share/rtx/installs/python/3.10.10/lib/python3.10/logging/__init__.py", line 678, in format
    record.message = record.getMessage()
  File "/Users/crmarsh/.local/share/rtx/installs/python/3.10.10/lib/python3.10/logging/__init__.py", line 368, in getMessage
    msg = msg % self.args
TypeError: not all arguments converted during string formatting
Call stack:
  File "<stdin>", line 1, in <module>
Message: 'foo: {}'
Arguments: ('foo',)
```

---

_Comment by @actionless on 2023-03-12 11:04_

my bad - i've just realized that `logging` is a local module in the project with the same name 🤦
just because i saw it in my pylint config i thought it's part of the default

and handling smth like this (https://stackoverflow.com/a/26003573) would indeed require type inference, i suppose

---

_Comment by @axd1x8a on 2023-03-13 15:27_

It is possible in the standard logging, even if it is [not so easy](https://docs.python.org/3/howto/logging-cookbook.html#use-of-alternative-formatting-styles), for example, I [use this](https://github.com/vkbottle/vkbottle/blob/ef9f5ead0c804a1cfcfae7202779f67dd41204e7/vkbottle/modules.py#L54-L83) to give users a choice between logguru/logging

---

_Comment by @actionless on 2023-03-13 22:29_

actually even after renaming the module i still can reproduce it:

```python
import sys
from typing import Any


class PikaurLogger:
    def __init__(
            self, module_name: str, color: int,
    ) -> None:
        self.module_name = module_name
        self.color = color

    def debug(
        self, msg: "Any", *args: "Any", **kwargs: "Any",
    ) -> None:
        str_message = msg.format(*args, **kwargs) if isinstance(msg, str) else str(msg)
        msg = f"{self.module_name}: {str_message}\n"
        sys.stderr.write(msg)


logger = PikaurLogger(module_name="build", color=1)
logger.debug("Build dir: {}", "foo")
```

```console
$ ./env/bin/ruff test_PLE1205.py
test_PLE1205.py:21:1: PLE1205 Too many arguments for `logging` format string
Found 1 error.
```

---

_Comment by @charliermarsh on 2023-03-13 22:35_

Ah yeah, we match on the name of the logging object (in this case, `logger`).

---

_Comment by @charliermarsh on 2023-07-24 18:10_

I think the _original_ issue here is resolved, Ruff no longer flags imports from modules apart from `logging`.

---

_Closed by @charliermarsh on 2023-07-24 18:10_

---

_Comment by @actionless on 2023-07-24 18:17_

can confirm 👍

---

_Comment by @Pixel-Minions on 2023-09-01 00:02_

Hello,

It seems this is back, 
![image](https://github.com/astral-sh/ruff/assets/1145623/66299f7e-126d-4f21-8491-890c3cb46e41)

Version 0.0.286

---

_Comment by @charliermarsh on 2023-09-01 00:04_

Can you share a complete reproduction? Note that Ruff will only avoid flagging these objects if they’re defined in another file and imported (rather than defined and used in the same file).

---

_Comment by @Pixel-Minions on 2023-09-01 00:06_

```python

import loguru

logger = loguru.logger
var = "Hello, World!"
logger.debug("{}", var)
```
![image](https://github.com/astral-sh/ruff/assets/1145623/bb1f682e-b91a-4a52-a310-63f78423782a)



---

_Comment by @charliermarsh on 2023-09-01 00:08_

Thanks! This is a known limitation of the rule — we can’t trace arbitrary local references. I’d recommend disabling these rules entirely if you’re using a separate logging library like loguru, or importing a logger from a common file, or using loguru.debug directly rather than aliasing.

---

_Comment by @Pixel-Minions on 2023-09-01 00:09_

Thank you for the info! I might disable it for now.

Cheers.

---

_Comment by @charliermarsh on 2023-09-01 00:16_

Out of curiosity: is that roughly the pattern you’re using in your real code? (Assigning loguru.logger to logger.) Or is it more complex in practice?

---

_Comment by @Pixel-Minions on 2023-09-01 00:36_

I have a logger manager object that returns a logger, for some older apps returns a logging logger for new versions a loguru logger, the manager does a lot of the setup depending of the host.

---
