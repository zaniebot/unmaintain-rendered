```yaml
number: 581
title: "W605 potential false positive with `\\` in multi-line strings"
type: issue
state: closed
author: AA-Turner
labels:
  - bug
assignees: []
created_at: 2022-11-04T14:03:38Z
updated_at: 2022-11-04T18:30:06Z
url: https://github.com/astral-sh/ruff/issues/581
synced_at: 2026-01-12T15:54:40Z
```

# W605 potential false positive with `\` in multi-line strings

---

_@AA-Turner_

Escaping a newline is explicitly allowed, see the table in Lexical Analysis: https://docs.python.org/3/reference/lexical_analysis.html#index-23

Reproducer:

```python
# bug.py
print("""\
Lorem ipsum dolar sit amet.
Lorem ipsum dolar \
sit amet.
Lorem ipsum dolar \
sit amet.""")
```

Sample output of `flake8` and `ruff`:

```doscon
I:\Development>type bug.py                         
print("""\                 
Lorem ipsum dolar sit amet.
Lorem ipsum dolar \        
sit amet.                  
Lorem ipsum dolar \        
sit amet.""")              

I:\Development>flake8 --isolated --select W bug.py 

I:\Development>ruff --select W bug.py              
Found 3 error(s).
'ug.py:1:10: W605 Invalid escape sequence: '\
'ug.py:3:20: W605 Invalid escape sequence: '\
'ug.py:5:20: W605 Invalid escape sequence: '\
```

I think there is also a bug in the error message here, as the first character is replaced by `'`.

Windows 10, Python 3.10

A

---

_Comment by @charliermarsh on 2022-11-04 14:07_

Thank you! Will fix this today.

---

_Label `bug` added by @charliermarsh on 2022-11-04 14:07_

---

_Assigned to @charliermarsh by @charliermarsh on 2022-11-04 14:07_

---

_Comment by @charliermarsh on 2022-11-04 14:30_

I'm wondering if this is Windows-specific? I'm not able to repro on macOS. Might take me a bit as I'll need to setup a testing env for Windows.


---

_Comment by @charliermarsh on 2022-11-04 14:38_

(I'm going through that process now, I need to be able to test on Windows anyway so this is a good reason to set it up.)

---

_Comment by @AA-Turner on 2022-11-04 14:38_

I can reproduce using Windows Subsystem for Linux, Ubuntu 22.04:

```bash
adam@Gyrostats:~$ cd /mnt/i/Development/
adam@Gyrostats:/mnt/i/Development/$ python3.11 -m venv venv
adam@Gyrostats:/mnt/i/Development/$ . ./venv/bin/activate
(venv) adam@Gyrostats:/mnt/i/Development/$ python -V
Python 3.11.0rc1
(venv) adam@Gyrostats:/mnt/i/Development/$ python -m pip install ruff
Collecting ruff
  Downloading ruff-0.0.99-py3-none-manylinux_2_17_x86_64.manylinux2014_x86_64.whl (3.3 MB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 3.3/3.3 MB 4.4 MB/s eta 0:00:00
Installing collected packages: ruff
Successfully installed ruff-0.0.99
(venv) adam@Gyrostats:/mnt/i/Development/$ ruff --select W bug.py
Found 3 error(s).
'ug.py:1:10: W605 Invalid escape sequence: '\
'ug.py:3:20: W605 Invalid escape sequence: '\
'ug.py:5:20: W605 Invalid escape sequence: '\
```

A

---

_Comment by @AA-Turner on 2022-11-04 14:42_

> I'm wondering if this is Windows-specific?

Got it!

```bash
(venv) adam@Gyrostats:/mnt/i/Development$ file bug.py
bug.py: ASCII text, with CRLF line terminators
(venv) adam@Gyrostats:/mnt/i/Development$ ruff --select W bug.py
Found 3 error(s).
'ug.py:1:10: W605 Invalid escape sequence: '\
'ug.py:3:20: W605 Invalid escape sequence: '\
'ug.py:5:20: W605 Invalid escape sequence: '\
(venv) adam@Gyrostats:/mnt/i/Development$ file good.py
good.py: ASCII text
(venv) adam@Gyrostats:/mnt/i/Development$ ruff --select W good.py
(venv) adam@Gyrostats:/mnt/i/Development$
```

Incorrect parsing of carriage returns, it seems.

A

---

_Comment by @charliermarsh on 2022-11-04 17:38_

I can reproduce on macOS by saving the file with CRLF newlines.

---

_Closed by @charliermarsh on 2022-11-04 18:26_

---

_Comment by @AA-Turner on 2022-11-04 18:29_

Thanks for the quick fix!

A

---

_Comment by @charliermarsh on 2022-11-04 18:30_

For sure! Thanks for the great Issue write-up. I'll cut a new release in a bit (want to include a few more small things).


---
