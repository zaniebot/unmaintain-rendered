```yaml
number: 7352
title: Add additional Color Environment Variables+Values
type: issue
state: closed
author: TheRealBecks
labels:
  - question
  - external
assignees: []
created_at: 2024-09-13T08:21:05Z
updated_at: 2024-09-16T14:28:47Z
url: https://github.com/astral-sh/uv/issues/7352
synced_at: 2026-01-10T04:45:10Z
```

# Add additional Color Environment Variables+Values

---

_Issue opened by @TheRealBecks on 2024-09-13 08:21_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->
I'm running `uv` version `0.4.9` on Opensuse Tumbleweed (rolling release).

In #3955 the color environment variables have been improved, but when executing `uv run ansible-playbook ...` I don't get an colored output, but it's working when activating the virtual environment with `sources ...` and exucuting `ansible-playbook ...`.

Until now the `uv run` command never supported colored output on my system. As I can see my system has other environemnt variables set then mentioned in #3955:
```
❯ printenv | grep -i color
COLORTERM=truecolor
COLORFGBG=15;0
TERM=xterm-256color
```

As far as I found out the following variables+values are used for declaring color support:
```
COLORTERM=truecolor
COLORTERM=24bit
```
...additionally to the variables+values you already check 😉

---

_Comment by @TheRealBecks on 2024-09-13 09:41_

I checked further what's going in in the Linux and Unix world and therefore it's always good to check what the [rich](https://github.com/Textualize/rich) team is doing:

They are using these variables+values to check for coloring in their tests:

https://github.com/Textualize/rich/blob/e9f75c9912ed25b9777bc0257853370951220b17/tests/test_console.py#L984-L992

-->
```
"FORCE_COLOR": "1",
"TERM": "xterm-256color",
"COLORTERM": "truecolor",
```

Over here they are parsing the following environment variables:

https://github.com/Textualize/rich/blob/e9f75c9912ed25b9777bc0257853370951220b17/rich/diagnose.py#L17-L29

-->
```
"TERM",
"COLORTERM",
"CLICOLOR",
"NO_COLOR",
```

`CLICOLOR` color is for MAC based systems told me the internet.

So these ones are needed for Linux and MAC:
```
"TERM": "xterm-256color",
"COLORTERM": "truecolor",
"CLICOLOR": "1",

# You already check for these ones?
"FORCE_COLOR": "1",
"NO_COLOR: "1"",
```

They are not checking for `COLORTERM=24bit`, but I found several Google entries for searching `"COLORTERM=24bit"` (with `"` included).

---

_Comment by @charliermarsh on 2024-09-13 13:12_

I think some of those are already checked in `anstream` which we use for color detection: https://github.com/rust-cli/anstyle/blob/c2ee0d95bf619b721da004f69fd69a46c5fc51b8/crates/anstyle-query/src/lib.rs#L55. It looks like `"TERM": "xterm-256color"` should be sufficient to enable color? So not sure why you aren't seeing it.

---

_Label `question` added by @charliermarsh on 2024-09-13 13:12_

---

_Comment by @charliermarsh on 2024-09-13 13:12_

How can I reproduce?

---

_Comment by @TheRealBecks on 2024-09-16 12:05_

I added a [test repository](https://github.com/TheRealBecks/uv-test-coloring) with a README.md.

I found out that Ansible is using the `curses` module that can be tested as follows:
```
import curses

curses.setupterm()
print(curses.tigetnum("colors"))
```

If the terminal supports coloring we will get a positive integer as return value (like `256`), if not we will get a negative number.

When __not__ using a virtual environment both Python 3.11 and 3.12 return `256`. But then using a virtual environment with `uv` or `pipenv` I get the following results:

The output of Python 3.11:
```
$ python test_curses.py 
256
```

...and Python 3.12:
```
python test_curses.py 
Traceback (most recent call last):
  File "/home/mbeckert/Entwicklung/Sonstiges/uv-test-coloring/test_curses.py", line 3, in <module>
    curses.setupterm()
_curses.error: setupterm: could not find terminfo database
```
🙃

So maybe that's a curses bug in Python 3.12?

#####

UPDATE: I found out what's going on here! 🙂 I will write another comment today, but I need to check some documentations.

---

_Comment by @TheRealBecks on 2024-09-16 13:29_

Regarding the coloring: That was a bug in Python `3.12.0`, but has been fixed in `3.12.x`. I wondered why I had Ansible workign with coloring in some cases, but my test cases had no colering at all. So I checked my Python versions:

There seems to be something odd with the Python version as `uv` used `3.12.0` instead of `3.12.5`. So I did some tests with the version defined in `pyproject.toml`. I did not delete the `uv.lock` file for these test cases, but used `uv sync` after I changed the `requires-python` key.

I started with `requires-python = "==3.12"` that lead to `3.12.0`. When changing to `==3.11`:
```
error: No interpreter found for Python ==3.11 in managed installations or system path
```
-> Which is correct, but from a user perspective I would not expect this message as there's `3.11.9` installed on my system. It would be good if the error message gives a clear message that `uv` searched for version `3.11.0` that's not been installed on my system.

Now changing to `>=3.11` still leads to version `3.12.0` as that has been choosen by `uv` in the first run.

So I see some problems from the user side:
* Many users do not know that there is a "zero padding expansion" seen [here](https://packaging.python.org/en/latest/specifications/version-specifiers/#version-matching) when using `==3.11` which leads to version 3.11.0 if it's installed on the system. Not sure what do about it... 😀
* In your [example](https://docs.astral.sh/uv/concepts/projects/#project-metadata) `>=3.12` is used what I did not use for my projects as I don't want Python `3.13.x` to be installed on new systems. So I wrongly choose `==3.12`. Instead I thing it would be best to use `~=3.12` and tell the people that it's the same as using `==3.12.*` and `>=3.12,<3.13`. In that case all users copying your example will get a `3.12` version instead of `3.13` on newer OSes.

# Wrapping up:
* I used the wrong version identifier in the first place which resulted to `3.12.0`
* I did not expect `uv` to be stuck at `3.12.0` even if I changed the identifier to `~=3.12`
* I think it's best to change the identifier in the documentation to `~=3.12` (or `==3.12.*`?) and describing what that means
* And maybe pointing the interested user in a notice field to the [`Version specifier`](https://packaging.python.org/en/latest/specifications/version-specifiers/) documentation?

Does that make sense what I'm saying here? 😀

# Final words
Thanks for your awesome software and support!

---

_Comment by @zanieb on 2024-09-16 13:41_

Created an issue for that specific problem https://github.com/astral-sh/uv/issues/7426

Thanks for investigating!

---

_Comment by @TheRealBecks on 2024-09-16 13:47_

Is there a `uv` command that tells me if `uv` will color my console? Or do you know how I can test this?

My Ansible example isn't working here, because they are using `curses` that checks my terminal environment variable `TERM=xterm-256color` itself (...that environment variable is __not__ check by `uv`, am I right?). I still think it's best to add these environemtn variables when checking for color support:
```
"TERM": "xterm-256color",
"COLORTERM": "truecolor",
"CLICOLOR": "1",

# Maybe this one:
"COLORTERM": "24bit",

# You already check for these ones?
"FORCE_COLOR": "1",
"NO_COLOR: "1"",
```

---

_Comment by @zanieb on 2024-09-16 13:51_

No we don't have a command to check. You might want to open an issue [upstream](https://github.com/rust-cli/anstyle) — we're not really experts on color selection and generally rely on the dependency to do "the right thing".

---

_Label `upstream` added by @zanieb on 2024-09-16 13:52_

---

_Comment by @TheRealBecks on 2024-09-16 14:27_

Done from my side. Thanks for your input! I checked the upstream `anstyle` and their environment variable checking looks sane to me 🙂

I opened #7429 for further discussion about Python versioning and the `uv` documentation

---

_Closed by @TheRealBecks on 2024-09-16 14:27_

---
