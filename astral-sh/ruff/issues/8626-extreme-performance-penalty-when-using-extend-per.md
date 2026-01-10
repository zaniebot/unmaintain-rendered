```yaml
number: 8626
title: "Extreme performance penalty when using `--extend-per-file-ignores` and leading recursive glob patterns"
type: issue
state: open
author: ofek
labels:
  - performance
assignees: []
created_at: 2023-11-12T02:42:56Z
updated_at: 2023-11-17T02:13:25Z
url: https://github.com/astral-sh/ruff/issues/8626
synced_at: 2026-01-10T11:09:50Z
```

# Extreme performance penalty when using `--extend-per-file-ignores` and leading recursive glob patterns

---

_Issue opened by @ofek on 2023-11-12 02:42_

```
git clone https://github.com/pypa/hatch
cd hatch
ruff check --isolated --select INP001 .
```

notice it runs fast and there are many errors, then run

```
ruff check --isolated --select INP001 --extend-per-file-ignores "scripts/*:INP001" .
```

notice it runs fast (I think no slow down) and there are fewer errors, then run

```
ruff check --isolated --select INP001 --extend-per-file-ignores "**/scripts/*:INP001" .
```

notice it takes about two seconds. Interestingly, when configured there is no slowdown:

```toml
[tool.ruff.per-file-ignores]
"**/scripts/*" = ["INP001"]
```

You might think, then, that perhaps the shells are globbing themselves but I've tried in `bash` and `nushell` with the proper single quotes/escaping mechanisms to no avail. Also interesting and inexplicable, the slowdown is not happening on Windows' `cmd` shell.

---

_Comment by @ofek on 2023-11-12 02:52_

I cannot reproduce this in Docker, looks like this is only a bug on Windows

---

_Comment by @ofek on 2023-11-12 03:02_

I suspect the bug is that the double leading asterisks somehow cause traversal to start at the root. Running the following (notice the leading forward slash) on Windows straight up hangs:

```
ruff check --select INP001 --extend-per-file-ignores "/**/scripts/*:INP001" .
```

---

_Comment by @ofek on 2023-11-12 03:05_

Actually, it's possible this bug is not just on Windows but rather the file system in the `python` Docker image was too small to tell. Then again, that doesn't explain the lack of the slowdown in command prompt. Anyway… this is all the information I have

---

_Comment by @charliermarsh on 2023-11-12 04:11_

\cc @BurntSushi who will have better instincts on what the possible set of issues is here.

---

_Comment by @ofek on 2023-11-12 04:26_

Timing:

```
❯ ruff check -v --select INP001 --extend-per-file-ignores "**/scripts/*:INP001" .
...
[2023-11-11][23:23:37][ruff_cli::commands::check][DEBUG] Identified files to lint in: 2.3416198s
[2023-11-11][23:23:37][ruff_cli::diagnostics][DEBUG] Checking: C:\Users\ofek\Desktop\code\hatch\pyproject.toml
[2023-11-11][23:23:37][ruff_cli::diagnostics][DEBUG] Checking: C:\Users\ofek\Desktop\code\hatch\backend\pyproject.toml
[2023-11-11][23:23:37][ruff_cli::diagnostics][DEBUG] Checking: C:\Users\ofek\Desktop\code\hatch\backend\tests\downstream\datadogpy\pyproject.toml
[2023-11-11][23:23:37][ruff_cli::commands::check][DEBUG] Checked 329 files in: 3.5788ms
```

---

_Label `performance` added by @charliermarsh on 2023-11-12 20:30_

---

_Comment by @BurntSushi on 2023-11-13 13:17_

I can't reproduce this on my Linux machine. Nothing really stands out to me as a possible culprit. Some ideas:

* Can you say more precisely in what environment (or environments) you're seeing a slow-down? Are you in a cygwin or MSYS environment for example?
* Failing all else, some printf debugging might help here. I'm still getting familiar with the Ruff code, but printing out all of the files that it's traversing might reveal something.

---

_Comment by @ofek on 2023-11-13 17:28_

I am on Windows 11 Pro with no emulation and no antivirus software running. I am more than happy to test any binary you can send me a link to!

---

_Comment by @BurntSushi on 2023-11-13 17:33_

Which shell are you using where you observed the problem? Are you in PowerShell?

---

_Comment by @ofek on 2023-11-13 17:38_

Affected: nushell, powershell, Git Bash, xonsh
Unaffected: cmd

---

_Comment by @ofek on 2023-11-14 18:20_

Update: a Windows expert from work inspected traces from my machine and figured out the issue. It spends a significant amount of time in `.ruff_cache`.

![image](https://github.com/astral-sh/ruff/assets/9677399/165ce33e-c308-4456-b20e-5149842fde60)

The `--no-cache` flag had no effect, only wiping that directory of >30k files worked. We both don't know why this issue did not present for me in command prompt but if this is accurate then somehow caching is not working when running in command prompt.

Do either of you know of a situation in the code where the flag would trigger cache reads whereas file-based config does not? I think that should be explored since there seems to be a real issue.

Edit: or more likely, perhaps there is logic that differs based on the process shell name and globbing somehow triggers different logic in command prompt

---

_Comment by @charliermarsh on 2023-11-14 18:22_

Is it that it's _indexing_ and traversing the cache to find Python files, instead of ignoring it? Or it's actually reading the cache contents?

---

_Comment by @ofek on 2023-11-14 18:26_

We don't know.

---

_Comment by @charliermarsh on 2023-11-14 18:30_

Fair enough :joy:

---

_Comment by @ofek on 2023-11-14 18:33_

Actually I discovered something else looking at the traces myself:

![image](https://github.com/astral-sh/ruff/assets/9677399/ca89d883-8c60-4858-9a05-1110efc18a15)

The beginning `**/` completely destroys what should be ignored and even `.git` is being entered. This is the fundamental issue.

---

_Comment by @ofek on 2023-11-14 18:38_

So to recap, this is definitely a bug but there are two caveats:

1. when configured via the file rather than command line this does not happen
2. on command line this does not happen in cmd/command prompt which indicates that some conditional logic is happening based on the process name

---

_Comment by @BurntSushi on 2023-11-14 18:57_

> on command line this does not happen in cmd/command prompt which indicates that some conditional logic is happening based on the process name

It could also be that `cmd.exe` is mangling the glob somehow such that it isn't applied.

---

_Comment by @ofek on 2023-11-16 23:54_

Have you been able to find out where in the code this is happening? I want to provide default ignores for the new `hatch fmt` command but I don't want to release until this is fixed because one of the patterns uses a leading `**/`.

---

_Comment by @BurntSushi on 2023-11-17 01:11_

I'm not personally investigating this, no. I can't reproduce the problem on my end unfortunately. I'm somewhat new to Ruff myself and don't understand its filtering logic yet, but I would wonder whether the leading `**/` is correct here. To me, that is explicitly telling Ruff to descend into every directory. But my mental model of what ought to happen here isn't completely clear.

---

_Comment by @charliermarsh on 2023-11-17 01:13_

I can spend a little time investing this tonight.

---

_Comment by @charliermarsh on 2023-11-17 01:20_

Separately, are you just using this for `scripts`, with `INP001` specifically?

---

_Comment by @charliermarsh on 2023-11-17 01:20_

Or are there more of these patterns?

---

_Comment by @ofek on 2023-11-17 02:04_

Thanks! Another one is `**/tests/**/*`

---

_Comment by @charliermarsh on 2023-11-17 02:06_

I'm wondering if we should just allow `per-file-ignores` to function like `excludes`, whereby we match individual basenames against the entire path (so the stars could just be omitted).

---

_Comment by @ofek on 2023-11-17 02:13_

Up to you. That would solve my particular use case but I don't think it's a great idea to reduce functionality just to fix a bug.

---
