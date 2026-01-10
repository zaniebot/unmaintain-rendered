```yaml
number: 10714
title: uv called through hatch or tox-uv writes debug to stderr
type: issue
state: closed
author: JohnStokes228
labels:
  - question
assignees: []
created_at: 2025-01-17T15:51:59Z
updated_at: 2025-02-01T19:37:30Z
url: https://github.com/astral-sh/uv/issues/10714
synced_at: 2026-01-10T03:50:31Z
```

# uv called through hatch or tox-uv writes debug to stderr

---

_Issue opened by @JohnStokes228 on 2025-01-17 15:51_

links to [tox-uv](https://github.com/tox-dev/tox-uv/issues/156) and [hatch](https://github.com/pypa/hatch/issues/1888) issues for posterity, same issue comes when running either tool, tox-uv bounced me here.

I am running [tox-uv / hatch] in an azure devops yaml pipeline via bash script. the tools themselves seem to run as expected end to end, however after completion of a successful test run, a number of debug statements are written to stderr, causing the pipeline task to fail despite an exit code of 0.
The specific statements themselves are also quite odd. whilst several are just entire debug messages like '##[error]Resolved 47 packages in 1.72s', it also includes the names of some packages, and at times even just the packages version number (i.e. '##[error] + anyio==4.8.0', '##[error]==2.2.1' or even '##[error] '. 

# for context:
- we run on linux self hosted agents, using ubuntu:20.04, and install uv via pip in eitehr a python 3.9 or python 3.10 environment (both produce the same outputs, due to our reliance on azure machine learning we cannot go beyond python 3.10 currently)
- we use a fresh agent with no other installs on to install first uv, then the required packages for testing (i.e. tox, tox-uv, pytest, ...)
- we dont see the same error when running uv commands manually in bash scripts, only when using these tools which wrap calls to uv.
- we can write the entire output to dev/null and **still** hit these error messages, suggesting they may not even be being written to stderr?
- these errors specifcally appear after the stage completes, i.e. if i were to run `tox` or `hatch test` and then follow it with echo statements, the error trace would appear after the subsequent echoes. 
- changing the rust log level to for instance trace doesnt fix the issue any further than sending to stdout / stderr does

# example code:
```
 pip install uv
uv pip install hatch pytest pytest-cov
RUST_LOG=trace
hatch test --all --cov --cov-report=xml --cov-report=term --randomize --junitxml=junit/test-results.xml
```

with output:

```
##[debug]Exit code 0 received from tool '/bin/bash'
##[debug]STDIO streams have closed for tool '/bin/bash'
##[error]Bash wrote one or more lines to the standard error stream.
##[debug]Processed: ##vso[task.issue type=error;source=TaskInternal;correlationId=c170ade4-6893-4a7e-8644-4632f00ef966;]Bash wrote one or more lines to the standard error stream.
##[error]
[notice] A new release of pip is available: 23.0.1 -> 24.3.1
[notice] To update, run: pip install --upgrade pip

##[debug]Processed: ##vso[task.issue type=error;source=CustomerScript;correlationId=c170ade4-6893-4a7e-8644-4632f00ef966;]
[notice] A new release of pip is available: 23.0.1 -> 24.3.1
[notice] To update, run: pip install --upgrade pip

##[error]Using Python 3.9.21 environment at: /azp/_work/_tool/Python/3.9.21/x64

##[debug]Processed: ##vso[task.issue type=error;source=CustomerScript;correlationId=c170ade4-6893-4a7e-8644-4632f00ef966;]Using Python 3.9.21 environment at: /azp/_work/_tool/Python/3.9.21/x64

##[error]Resolved 47 packages in 1.43s

##[debug]Processed: ##vso[task.issue type=error;source=CustomerScript;correlationId=c170ade4-6893-4a7e-8644-4632f00ef966;]Resolved 47 packages in 1.43s

##[error]Prepared 28 packages in 695ms

##[debug]Processed: ##vso[task.issue type=error;source=CustomerScript;correlationId=c170ade4-6893-4a7e-8644-4632f00ef966;]Prepared 28 packages in 695ms

##[error]Installed 46 packages in 39ms
...

##[error]
 + pluggy==1.5.0
 + ptyprocess==0.7.0
 + pycparser==2.22
 + pygments==2.19.1
 + rich==13.9.4
 + secretstorage==3.3.3
 + shellingham==1.5.4
 + sniffio==1.3.1
 + tomli
##[debug]Processed: ##vso[task.issue type=error;source=CustomerScript;correlationId=c170ade4-6893-4a7e-8644-4632f00ef966;]
 + pluggy==1.5.0
 + ptyprocess==0.7.0
 + pycparser==2.22
 + pygments==2.19.1
 + rich==13.9.4
 + secretstorage==3.3.3
 + shellingham==1.5.4
 + sniffio==1.3.1
 + tomli
##[error]==2.2.1
 + tomli-w==1.2.0
 + tomlkit==0.13.2
 + trove-classifiers==2025.1.10.15
 + typing-extensions==4.12.2
 + trove-classifiers==2025.1.10.15
 + typing-extensions==4.12.2
 +
##[error] userpath==1.9.2
 + virtualenv==20.28.1
 + zipp==3.21.0
 + zstandard==
##[debug]Processed: ##vso[task.issue type=error;source=CustomerScript;correlationId=c170ade4-6893-4a7e-8644-4632f00ef966;] userpath==1.9.2
 + virtualenv==20.28.1
 + zipp==3.21.0
 + zstandard==
##[error]0.23.0

##[debug]Processed: ##vso[task.issue type=error;source=CustomerScript;correlationId=c170ade4-6893-4a7e-8644-4632f00ef966;]0.23.0

##[error]Creating environment: hatch-test.py3.10

##[debug]Processed: ##vso[task.issue type=error;source=CustomerScript;correlationId=c170ade4-6893-4a7e-8644-4632f00ef966;]Creating environment: hatch-test.py3.10

##[error]Installing project in development mode

##[debug]Processed: ##vso[task.issue type=error;source=CustomerScript;correlationId=c170ade4-6893-4a7e-8644-4632f00ef966;]Installing project in development mode

##[error]Checking dependencies

##[debug]Processed: ##vso[task.issue type=error;source=CustomerScript;correlationId=c170ade4-6893-4a7e-8644-4632f00ef966;]Checking dependencies

##[error]Syncing dependencies

##[debug]Processed: ##vso[task.issue type=error;source=CustomerScript;correlationId=c170ade4-6893-4a7e-8644-4632f00ef966;]Syncing dependencies

##[error]Creating environment: hatch-test.py3.9

##[debug]Processed: ##vso[task.issue type=error;source=CustomerScript;correlationId=c170ade4-6893-4a7e-8644-4632f00ef966;]Creating environment: hatch-test.py3.9

##[error]Installing project in development mode

##[debug]Processed: ##vso[task.issue type=error;source=CustomerScript;correlationId=c170ade4-6893-4a7e-8644-4632f00ef966;]Installing project in development mode

##[error]Checking dependencies

##[debug]Processed: ##vso[task.issue type=error;source=CustomerScript;correlationId=c170ade4-6893-4a7e-8644-4632f00ef966;]Checking dependencies

##[error]Syncing dependencies

##[debug]Processed: ##vso[task.issue type=error;source=CustomerScript;correlationId=c170ade4-6893-4a7e-8644-4632f00ef966;]Syncing dependencies

##[debug]task result: Failed
##[debug]Processed: ##vso[task.complete result=Failed;done=true;]
```


---

_Comment by @zanieb on 2025-01-17 15:59_

Perhaps you're looking to ensure uv is invoked with the `--quiet` option? I'm not sure I follow what else could be done here.

---

_Label `question` added by @zanieb on 2025-01-17 15:59_

---

_Comment by @JohnStokes228 on 2025-01-17 16:22_

hmmm potentially? although im not convinced this is the issue - it appears to me the logs are specifically being written somewhere other than stdout / stderr, and are being interpreted as errors when they arent. its possible running in quiet may resolve this (is there a means other than setting the rust log to 'trace' as described in the docs to achieve this when uv is run by a third party tool?) but im not sure it deals with the core issue?

thanks very much for getting back to me so quickly though, really appreciate it man!

---

_Comment by @zanieb on 2025-01-17 16:37_

We don't write logs anywhere but stdout / stderr. It sounds like the tool is just reporting everything in the stderr stream as `##[error]`?

```
❯ uv venv
Using CPython 3.12.6
Creating virtual environment at: .venv
Activate with: source .venv/bin/activate
❯ uv pip install anyio 2>stderr.txt 1>stdout.txt
❯ cat stderr.txt
Resolved 4 packages in 118ms
Installed 4 packages in 6ms
 + anyio==4.8.0
 + idna==3.10
 + sniffio==1.3.1
 + typing-extensions==4.12.2
❯ cat stdout.txt
```

---

_Comment by @zanieb on 2025-01-17 18:57_

It's proper for us to write our debug logs to stderr — that's the unix convention.

---

_Closed by @zanieb on 2025-02-01 19:37_

---
