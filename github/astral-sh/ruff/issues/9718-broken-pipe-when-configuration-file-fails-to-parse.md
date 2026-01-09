---
number: 9718
title: Broken pipe when configuration file fails to parse
type: issue
state: closed
author: kaste
labels: []
assignees: []
created_at: 2024-01-30T20:04:47Z
updated_at: 2024-02-08T17:33:32Z
url: https://github.com/astral-sh/ruff/issues/9718
synced_at: 2026-01-07T13:12:15-06:00
---

# Broken pipe when configuration file fails to parse

---

_Issue opened by @kaste on 2024-01-30 20:04_

Test scenario:

I have an error in my ruff.toml

```
select = ["F", "E", "W", "B", "ERA", "I001"]
line-length = 100

[lint.isort]
line-length = 90
```

and then I do on Windows CMD

```
type plugin.py | ruff check --no-cache -
```

which outputs

```
> type plugin.py | ruff check --no-cache -
ruff failed
  Cause: Failed to parse C:\Users\c-flo\AppData\Roaming\Sublime Text\Packages\TreeSitter-navigate-around\ruff.toml
  Cause: TOML parse error at line 1, column 1
  |
1 | select = ["F", "E", "W", "B", "ERA", "I001"]
  | ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
unknown field `line-length`, expected one of `force-wrap-aliases`, `force-single-line`, `single-line-exclusions`, `combine-as-imports`, `split-on-trailing-comma`, `order-by-type`, `force-sort-within-sections`, `case-sensitive`, `force-to-top`, `known-first-party`, `known-third-party`, `known-local-folder`, `extra-standard-library`, `relative-imports-order`, `required-imports`, `classes`, `constants`, `variables`, `no-lines-before`, `lines-after-imports`, `lines-between-types`, `forced-separate`, `section-order`, `no-sections`, `detect-same-package`, `from-first`, `length-sort`, `length-sort-straight`, `sections`

Ein Prozess hat versucht, zu einer nicht bestehenden Pipe zu schreiben.
```

Note the last line which tells you (in german) that the process tried to write to a closed pipe.  

This is a reduced testcase of course.  I call this within an IDE (Sublime Text, I'm the author of the linting framework SublimeLinter over there) basically like so

```
            try:
                out = proc.communicate(code_b)

            except BrokenPipeError as err:
                print("BrokenPipeError", err)
```
 which prints

```
BrokenPipeError [Errno 32] Broken pipe
```

At this point stdout and stderr are closed so I don't get the actual error message/printing you do on the CLI.  It just aborts here and I can only present the generic "Broken pipe" error.  

Hope this helps.



---

_Comment by @kaste on 2024-01-30 20:06_

(Also the actual error message is not very indicative as it points to the first line of the conf file which is not the problem, but that's another story)

---

_Comment by @zanieb on 2024-01-30 20:40_

Hm I'm not sure what's up with that error message pointing to the wrong line, we can consider that separately.

Here, I presume the broken pipe error is probably coming from the process that is piping content _to_ Ruff, not from Ruff itself. I cannot reproduce this with `head`

e.g.
```bash
#!/usr/bin/env bash

set -o pipefail

head -c 10M < /dev/urandom | ruff check --no-cache -
echo "Finished with exit code $?"
```

```
❯ bash example.sh
ruff failed
  Cause: Failed to parse /Users/mz/eng/src/astral-sh/ruff/pyproject.toml
  Cause: TOML parse error at line 59, column 1
   |
59 | [tool.ruff]
   | ^^^^^^^^^^^
unknown field `err`

Finished with exit code 2
```

---

_Comment by @zanieb on 2024-01-30 20:43_

Hm I also cannot reproduce this in Python

```python
import subprocess

stdin = b"x" * 1000000

process = subprocess.Popen(["ruff", "check", "--no-cache", "-"], stdin=subprocess.PIPE, stdout=subprocess.PIPE, stderr=subprocess.PIPE)

try:
    out = process.communicate(stdin)
except BrokenPipeError as err:
    print(err)
else:
    print(out)

process.wait()
```

```
❯ python example.py
(b'', b'ruff failed\n  Cause: Failed to parse /Users/mz/eng/src/astral-sh/ruff/pyproject.toml\n  Cause: TOML parse error at line 59, column 1\n   |\n59 | [tool.ruff]\n   | ^^^^^^^^^^^\nunknown field `err`\n\n')
```

Do you have a reproducible example?

---

_Referenced in [astral-sh/ruff#9719](../../astral-sh/ruff/issues/9719.md) on 2024-01-30 20:47_

---

_Comment by @kaste on 2024-01-30 22:02_

That's indeed surprisingly random.  It seems to depend on the contents of the linted file.  I've set up https://github.com/kaste/ruff-repro  Note that plugin.py results in a Broken Pipe and the very similar plugin_2.py not.  I tried deleting more random lines, the file contents are already fantasy code though without syntax errors, but cannot deduce any pattern here.  Maybe you can reproduce with these files, note however that I'm on Windows.

```
> ruff --version
ruff 0.1.15
```

>  I presume the broken pipe error is probably coming from the process that is piping content to Ruff

I reproduce this on the windows command line but *also* within Sublime Text editor. 

---

_Comment by @kaste on 2024-01-30 22:22_

Stupid me, if the file is larger than 4096 it fails.  The standard buffer size on Windows.  🙄  

---

_Comment by @kaste on 2024-02-01 10:00_

I see that you both starred 👀 at my previous message but that's ambiguous.  Do you have an understanding of the bug?  I could then delete the "repro-repo" as it should be clear how to reproduce this.

---

_Comment by @zanieb on 2024-02-08 03:47_

Could you please provide a reproduction? It's still not clear to me that the broken pipe error is being thrown by Ruff. Generally what happens with broken pipes is

1. Some process is sending data into a pipe
2. Another process (Ruff in this case) is reading data from the pipe
3. The later process encounters an error (invalid configuration in this case) and exits
4. The first process continues sending data to the pipe but the second process has stopped reading it or closed it
5. The pipe is full or closed and is "broken"
6. The first process throws an error

I think it is best practice (and I believe this is commonly implemented) for the first process to _not_ throw the broken pipe error. I don't think Ruff can do anything if the scenario is as-described above.

---

_Comment by @kaste on 2024-02-08 08:47_

> Could you please provide a reproduction?

But I have the reproduction, you just tried on the wrong system and it is not clear that you now tried it on Windows.  So the file that causes the error is "plugin.py" from the repo I posted.  But that is not important as the file just needs to be larger than 4096, which happens to be the buffer size on Windows.  

With this file I do 

```
> type plugin.py | ruff
```

and get a broken pipe error on Windows, in german that reads "Ein Prozess hat versucht, zu einer nicht bestehenden Pipe zu schreiben." on the command line.  (That's the standard cmd shell, I can try power shell later.)

and I do 

```
$ cat plugin.py | ruff
```

in bash and everything works as expected.  (Although I just used the subsystem within Windows for the test.  We don't expect it to break here so that should be enough.)

So that's the reproduction I think.  I also explained it from a different perspective as I'm the author of the linting framework for the SublimeText editor.  There we have dozens of linters that accept the input from stdin and of course they also throw and fail from time to time but don't "break the pipe".  (E.g. you can do `type plugin.py | eslint --stdin`, say with a broken eslint configuration, and you will not see "Ein Prozess hat versucht, zu einer nicht bestehenden Pipe zu schreiben." on the command line and SublimeText can show the complete stderr to the end-user if that matters.)  That's the _common behavior_ I think.  I don't think and remember that there is a common _implementation_ as you suggest as that's a low-level system thing.  

AFAIR I had to solve and tackle this problem years ago as the number 4096 immediately shrugged me but I forgot and don't have a deeper knowledge of Windows right now. 

TL;DR  Have you tried on *Windows*?



---

_Comment by @kaste on 2024-02-08 17:26_

That's a shitty issue here I made.  It is easily resproducable by me but only when
I'm using zombie tools.  Perhaps that's because I'm old too.   🤷 

`cmd.exe` + `type` is of course a real thing in Windows but an ancient one.  Power 
shell's `Get-content` behaves well; and recent versions of python too. To compare 
it with `eslint` is just silly because that's just too slow for any race condition
to appear.

I now backported a sane `Popen._communicate` for my (also ancient) linting framework.
That's 20 lines of code.  

I really was mislead because `type <file> | ruff` was the easy reproduction.  
Actually a combination I never use in practice but only to reproduce or show something.
And it *was* a python bug, just fixed June 2016 😁. 






---

_Comment by @zanieb on 2024-02-08 17:33_

Sorry I didn't try on Windows I don't have easy access to a machine 😬 

I recently ran into a similar problem piping `git` output into `ripgrep` where the broken pipe error was coming from `git` so I had a hunch this was a problem outside of Ruff. Thanks for taking the time to investigate it — I really appreciate it! I'm glad it's working for you now :)


---

_Closed by @zanieb on 2024-02-08 17:33_

---
