---
number: 3800
title: uv compile now shows annotations for -c and -r paths that causes issues cross platform
type: issue
state: closed
author: inoa-jboliveira
labels:
  - bug
  - windows
assignees: []
created_at: 2024-05-23T19:08:37Z
updated_at: 2024-05-24T18:57:39Z
url: https://github.com/astral-sh/uv/issues/3800
synced_at: 2026-01-10T01:23:31Z
---

# uv compile now shows annotations for -c and -r paths that causes issues cross platform

---

_Issue opened by @inoa-jboliveira on 2024-05-23 19:08_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->
Since uv 0.2.x the pip compile tool adds -r and -c annotations on the "via" part of the code, but uses windows-style paths when running on Windows, not respecting the command line on "-c". 

tested both
uv 0.2.1 (1fc71ea73 2024-05-22)
uv 0.2.2 (e52ae0e2b 2024-05-22)

See diff below
![Screenshot 2024-05-23 154932](https://github.com/astral-sh/uv/assets/148236158/d3ac3456-41b8-45c5-bb0d-d398307a4891)

my command line is (I run from the project root):
`uv pip compile ./requirements/requirements.in -o ./requirements/requirements-win.txt --python-platform=windows`

I run the same command line on Windows and on my Linux CI, I make sure to always use forward slashes so outputs are compatible.

In my "requirements.in" file there is a line `-c dependencies.txt` since the file is in the same directory, I cannot prefix it with "-c requirements/dependencies.txt" so UV gets to chose how to build the path. 

I like having the annotations and wouldn't want to switch to "--no-annotate", but running the same command in 2 different OSs now accuses a diff in the files.

Since UV already respects the forward slashes when I pass "requirements/requirements.in", could it use the same prefix of the file containing the value when prefixing names that are relative?




---

_Comment by @charliermarsh on 2024-05-23 19:14_

Hmm, I think the annotation should match whatever form is used verbatim. What is the exact form used in `requirements.in`?

---

_Comment by @charliermarsh on 2024-05-23 19:16_

Can you include a `requirements.in` file that replicates it along with the command? Thanks!

---

_Comment by @charliermarsh on 2024-05-23 19:17_

Oh sorry I see. You're using a relative path. Hmm...

---

_Label `windows` added by @charliermarsh on 2024-05-23 19:17_

---

_Comment by @inoa-jboliveira on 2024-05-23 19:30_

Simplified version of my setup:

- /project-root/
   - requirements/
      - requirements.in
      - dependencies.txt
      - requirements-win.txt
  - compile.bat


requirements.in: 
```
-c dependencies.txt
bcrypt
bs4
```
dependencies.txt
```
pip>24
```

compile.bat (it contains just the commands so I use it both as a bat and sh file)
```
uv pip compile ./requirements/requirements.in -o ./requirements/requirements-win.txt --python-platform=windows
```

---

_Comment by @inoa-jboliveira on 2024-05-23 19:34_

I would be ok with the -c being verbatim. Althought it woud be less correct i guess


---

_Comment by @inoa-jboliveira on 2024-05-23 19:41_

Funny thing that I just noticed: you already use `./requirements` and join with `\` the `\dependencies.txt`
So you more or less do what I described. Probably getting one extra char from the same string you extract the prefix will solve the issue

---

_Comment by @charliermarsh on 2024-05-23 19:59_

I agree that it would be nice to be consistent here. Will look into it.

---

_Label `bug` added by @charliermarsh on 2024-05-23 19:59_

---

_Comment by @charliermarsh on 2024-05-23 19:59_

Marking as a bug.

---

_Comment by @inoa-jboliveira on 2024-05-23 20:29_

I think this could be solved in the user_display method (or maybe the origin.path()). I am having issues with the rust debugger on Windows right now, but I think at this point it should be possible to acquire the path separator.

https://github.com/astral-sh/uv/blob/3e4365301e36ab084650a6acdbe2b7b1487777c2/crates/distribution-types/src/annotation.rs#L31-L36

https://github.com/astral-sh/uv/blob/bc963d13cb5b1e0ca0783f62848d3ff5d93f3dda/crates/uv-fs/src/path.rs#L52-L63

https://github.com/astral-sh/uv/blob/3e4365301e36ab084650a6acdbe2b7b1487777c2/crates/pep508-rs/src/origin.rs#L16-L21


Here is the original PR published in v0.1.43
- https://github.com/astral-sh/uv/pull/3269

---

_Assigned to @charliermarsh by @charliermarsh on 2024-05-23 20:58_

---

_Referenced in [astral-sh/uv#3804](../../astral-sh/uv/pulls/3804.md) on 2024-05-23 21:20_

---

_Closed by @charliermarsh on 2024-05-24 03:02_

---

_Comment by @inoa-jboliveira on 2024-05-24 18:57_

I just tried release v0.2.3 and it works perfectly. Thank you!

---
