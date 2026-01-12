```yaml
number: 2656
title: "successful  `uv pip install x` operation outputs stream is  `stderr` not `stdout`"
type: issue
state: closed
author: dmatos2012
labels:
  - question
assignees: []
created_at: 2024-03-25T19:13:05Z
updated_at: 2024-03-25T21:08:42Z
url: https://github.com/astral-sh/uv/issues/2656
synced_at: 2026-01-12T15:58:39Z
```

# successful  `uv pip install x` operation outputs stream is  `stderr` not `stdout`

---

_@dmatos2012_

Hello everyone, 

It appears  that `uv pip install <package_name>` prints its output to `stderr` rather than `stdout` as it would be expected in a successful operation

MRE 
```
❯ uv pip install numpy >/dev/null 
Resolved 1 package in 7ms
Installed 1 package in 11ms
 + numpy==1.26.4

❯ uv pip install numpy 2>/dev/null

```
But something like `uv --version` goes as expected 
```
❯ uv --version >/dev/null 


❯ uv --version 2>/dev/null        
uv 0.1.24

```

I am not sure if this is intended, but thought it would be useful to report. Thanks for your time and for the awesome package ! :) 

```
Platform:
Description:	Ubuntu 20.04.6 LTS
Release:	20.04

> uv --version
`0.1.24`
```


<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->


---

_Comment by @zanieb on 2024-03-25 19:48_

Hi! You're seeing log output go to stderr, which is consistent with command line tool standards. We only send "data" that would be worth piping to other tools to stdout.

---

_Label `question` added by @zanieb on 2024-03-25 19:48_

---

_Comment by @dmatos2012 on 2024-03-25 20:50_

Ah ok, thanks for the response. I guess I could check the exit code instead. Thanks!

---

_Comment by @charliermarsh on 2024-03-25 21:08_

👍 Definitely recommend relying on the exit code over the output.

---

_Closed by @charliermarsh on 2024-03-25 21:08_

---
