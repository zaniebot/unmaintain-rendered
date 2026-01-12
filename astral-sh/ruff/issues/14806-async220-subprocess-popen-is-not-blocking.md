```yaml
number: 14806
title: "ASYNC220: subprocess.Popen() is not blocking"
type: issue
state: closed
author: ybybwdwd
labels:
  - question
assignees: []
created_at: 2024-12-06T06:13:46Z
updated_at: 2025-01-27T01:52:04Z
url: https://github.com/astral-sh/ruff/issues/14806
synced_at: 2026-01-12T15:54:54Z
```

# ASYNC220: subprocess.Popen() is not blocking

---

_@ybybwdwd_

`subprocess.Popen()` starts a child process and immediately returns a Popen instance.

It should not be fixed.

---

_Comment by @MichaReiser on 2024-12-06 08:54_

Ruff's behavior here matches the [upstream rule](https://github.com/python-trio/flake8-async/blob/dd684fb935941b5884172bd2b5df90d561f15a4b/flake8_async/visitors/visitor2xx.py#L212C1-L213C36) (not saying it's correct because of that).

I'm not familiar with asyncio or any other Python async libraries but I suspect that `subprocess.Popen` is disallowed because the method *blocks on the system call to spawn a process*. This is similar to blocking on reading a file in that the call waits for the OS to complete some operation. Could you tell me more why you think that `subprocess.Popen` isn't blocking and e.g `os.popen` is?

Reading the Python documentation I found

> [popen()](https://docs.python.org/3/library/os.html#os.popen) is a simple wrapper around [subprocess.Popen](https://docs.python.org/3/library/subprocess.html#subprocess.Popen). Use [subprocess.Popen](https://docs.python.org/3/library/subprocess.html#subprocess.Popen) or [subprocess.run()](https://docs.python.org/3/library/subprocess.html#subprocess.run) to control options like encodings.





---

_Comment by @ybybwdwd on 2024-12-07 03:18_

> I'm not familiar with asyncio or any other Python async libraries but I suspect that subprocess.Popen is disallowed because the method blocks on the system call to spawn a process. This is similar to blocking on reading a file in that the call waits for the OS to complete some operation. Could you tell me more why you think that subprocess.Popen isn't blocking and e.g os.popen is?

`subprocess.Popen()` behaves differently from `os.popen()` and `subprocess.run()` in that it returns a `Popen` instance immediately after creating a child process without blocking the current task.

It can manage child processes more flexibly, using ` Popen.wait () ` to wait for the child process to end, ` Popen.terminate () ` to stop the child process, ` Popen.poll () ` to check the child process status, etc.

> Reading the Python documentation I found
>> [popen()](https://docs.python.org/3/library/os.html#os.popen) is a simple wrapper around [subprocess.Popen](https://docs.python.org/3/library/subprocess.html#subprocess.Popen). Use [subprocess.Popen](https://docs.python.org/3/library/subprocess.html#subprocess.Popen) or [subprocess.run()](https://docs.python.org/3/library/subprocess.html#subprocess.run) to control options like encodings.

Python documentation may not explicitly state that `subprocess.Popen()` doesn't block, its behavior was tested by me writing the demo

https://docs.python.org/3.13/library/subprocess.html#popen-objects



---

_Comment by @MichaReiser on 2024-12-07 13:13_

Thank you

> Popen instance immediately after creating a child process without blocking the current task.

I think the point here is that *creating a child process* is a blocking call, the same as reading a file is. You're blocked on the OS to get around and create the process for you. That might take a while. But I'm sure @Zac-HD can give you a way more satisfying answer.

---

_Comment by @Zac-HD on 2024-12-08 19:49_

Per [the documentation here](https://flake8-async.readthedocs.io/en/latest/rules.html#blocking-sync-calls-in-async-functions), we recommend instead using a natively async interface ([e.g. `trio.run_process()`](https://trio.readthedocs.io/en/latest/reference-io.html#trio.run_process)).
  
While the initial process creation of `subprocess.Popen` is typically cheap, it sets up a footgun in blocking poll/wait/communicate methods and I've been bitten enough times in a large codebase that we introduced this rule.  I've also found that the Trio APIs (for example) make it considerably easier to write correct programs in general; for example defaulting to `check=True` + keeping `.returncode` synced + `stdin=b""` are also pretty nice to have, along with making it easier to wait _exactly_ until the process is done and no longer in the simple case.

---

_Label `question` added by @charliermarsh on 2025-01-27 01:41_

---

_Closed by @charliermarsh on 2025-01-27 01:52_

---

_Closed by @charliermarsh on 2025-01-27 01:52_

---
