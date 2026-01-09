---
number: 16163
title: "`uv venv` suspends itself while prompting"
type: issue
state: open
author: roy-work
labels:
  - question
assignees: []
created_at: 2025-10-07T20:56:12Z
updated_at: 2025-12-31T14:30:49Z
url: https://github.com/astral-sh/uv/issues/16163
synced_at: 2026-01-07T13:12:19-06:00
---

# `uv venv` suspends itself while prompting

---

_Issue opened by @roy-work on 2025-10-07 20:56_

### Question

At my org, we have a small wrapper script around `uv`. I'm going to boil that wrapper down to a minimal example here, but just note that the "real world case" is bigger:

```sh
#!/bin/bash

set -eu -o pipefail

function have_dyld_correct() {
	zsh -ic 'python3 -c '\''print("set")'\'
}

python3 -c 'import sys; li = sys.stdin.readline(); print(repr(li))'
PYTHON_VERSION="3.9.19"
HAVE_DYLD_CORRECT="$(have_dyld_correct)"
uv venv --seed --managed-python --python "$PYTHON_VERSION"
```

If you run this, and the venv already exists, `uv venv` will self-suspend into the background, and then fail if resumed:

```
± /
» ./wat.sh
Using CPython 3.9.19
Creating virtual environment with seed packages at: .venv
? A virtual environment already exists at `.venv`. Do you want to replace it? [y/n] › yes

hint: Use the `--clear` flag or set `UV_VENV_CLEAR=1` to skip this promptzsh: suspended (tty output)  ./wat.sh
± /
(last command got signal SIGTTOU)
» fg
[2]  - continued  ./wat.sh
error: Failed to create virtual environment
  Caused by: Interrupted system call (os error 4)
± /
»
```
Note that at the prompt where I type `fg`, the TTY's state is borked. (Notably, I have no cursor.) The terminal remains borked even after `uv` fails.

Either is this a bug in `uv`, or do you know what's going on here?

---

Removing the interactive `zsh` call causes `uv` to behave normally, but it isn't clear to me why those two are interacting the way they are.

<details>
<summary>What is the real wrapper script doing / why <tt>zsh -i</tt>?</summary>

As the function names hit at, we have a function in our wrapper that attempts to check if `DYLD_LIBRARY_PATH` is appropriately set — this helps us ensure devs don't run into issues with Python modules that have dependencies on native code, e.g., on Pango. The reason for invoking `zsh -i` is that, on macOS and macOS only, checking the value of `DYLD_LIBRARY_PATH` is tricky: most of the system utilities will remove it from the environment, and thus many attempts to check its value can result in mistakenly thinking it is unset when it is set. Because _this script_ itself is part of our bootstrapping a dev, it too runs under such a system utility, and so by the time we're executing, `DYLD_LIBRARY_PATH` has already been unset, _even if the user has it set_. The `zsh -i` ensures that the child command gets an environment similar to the user's shell, in which we can check on that env var. (Otherwise, bash is perfectly capable of testing if an env var is set, of course.)

<details>

### Platform

macOS 15.5, `Darwin 24.5.0 arm64`

### Version

`uv 0.8.24 (Homebrew 2025-10-07)`

---

_Label `question` added by @roy-work on 2025-10-07 20:56_

---

_Comment by @konstin on 2025-12-18 13:21_

This isn't uv specific, I can also reproduce this on Ubuntu 24.04 with zsh using a second Python process:

```
#!/bin/bash

set -eu -o pipefail

function have_dyld_correct() {
	zsh -ic 'python3 -c '\''print("set")'\'
}

python3 -c 'import sys; li = sys.stdin.readline(); print(repr(li))' &
PYTHON_VERSION="3.9.19"
HAVE_DYLD_CORRECT="$(have_dyld_correct)"

python3 -c 'import sys; li = input(); print(li)'
```

I get a "zsh: suspended (tty input)  ./uv.sh" with and without the `&` I added above, thought without the `&`, I need to press enter first (might be a linux thing). It seems that the zsh usage generally does some changes to stdin that affect all programs running afterwards trying to use stdin, uv just happens to be the next program in the script.

---

_Comment by @EliteTK on 2025-12-31 14:27_

Regarding the failure on `EINTR`. I somehow recall this was fixed. I can't get that to happen with `0.9.20` at least. That is to say, uv still gets suspended but resuming it doesn't lead to that error. But I haven't tried on a mac.

@konstin's example gets a `tty input` (`SIGTTIN`) instead of a `tty output` (`SIGTTOU`). It took me a moment to figure out why we would ever get a `SIGTTOU` for output despite the fact that `TOSTOP` shouldn't be getting set by default, until I found out that terminal ioctls which modify the terminal state can cause this, and we change the terminal configuration as part of setting up to prompt the user. You can replicate the exact scenario with just:

```
bash -c 'zsh -ic /bin/true; stty sane'
```

That being said, if uv didn't call `ioctl(0, TCSETSW, ...)` you would get konstin's case above, which can be simplified down to:

```
bash -c 'zsh -ic /bin/true; read'
```

There's a great post on `SIGTTOU` and `SIGTTIN` here: https://www.linusakesson.net/programming/tty/index.php It's good background reading for this issue.

Now, especially if you've read the above, you might ask: Why are we getting a `SIGTTOU`/`SIGTTIN` in the first place? After all, the process which gets signalled should be in the foreground process group?

Well, if you expand the script and sprinkle some `ps lT`[^psflags] you get:

```bash
ps lT
zsh -ic /bin/true
ps lT
read
```

This outputs:

```
F   UID   PID  PPID PRI  NI    VSZ   RSS WCHAN  STAT TTY        TIME COMMAND
0  1000  2561 31326  20   0  10044  6216 sigsus S    pts/12     0:00 zsh
0  1000  2567  2561  20   0   7152  3564 do_wai S+   pts/12     0:00 /bin/bash ./foo
4  1000  2568  2567  20   0   7456  3700 -      R+   pts/12     0:00 ps lT
0  1000 31326  6468  20   0   8472  5440 do_wai Ss   pts/12     0:00 /bin/bash --posix
F   UID   PID  PPID PRI  NI    VSZ   RSS WCHAN  STAT TTY        TIME COMMAND
0  1000  2561 31326  20   0  10044  6216 sigsus S    pts/12     0:00 zsh
0  1000  2567  2561  20   0   7152  3568 do_wai S    pts/12     0:00 /bin/bash ./foo
4  1000  2572  2567  20   0   7456  3740 -      R    pts/12     0:00 ps lT
0  1000 31326  6468  20   0   8472  5440 do_wai Ss   pts/12     0:00 /bin/bash --posix
zsh: suspended (tty input)  ./foo
```

Specifically the `+` in the STAT column for the first invocation indicates that ps and the bash process itself both belong to the foreground process group.

In the second output, after `zsh -i`, they no longer belong to the foreground process group.

The reason is that when you use `zsh -i`, `zsh` (a child process of bash) will put itself in a new process group (`setpgid(0, 0)`) and then set the current terminal's (`pts/12` in the example above) foreground process group ID to its new group ID. This means that the bash instance running the script will from that point onwards no longer be in the foreground process group[^restore-pgrp].

So, to confirm, this isn't a uv issue. Although you can pass `--clear` to work around it.

If you need to keep using `zsh -i` you can solve this by spawning that instance of zsh in a new terminal using something like `script` or alternatively using python's [pty](https://docs.python.org/3/library/pty.html) module[^ptymod]. That's probably the most "correct" approach.

But an alternative option is just to use python to re-set the terminal's foreground process group:

```bash
zsh -ic /bin/echo
python -c '
import os, sys, signal
signal.pthread_sigmask(signal.SIG_BLOCK, [signal.SIGTTOU])
os.tcsetpgrp(sys.stdin.fileno(), os.getpgid(0))
'
read
```

Depends on your appetite for horrendous incantations :)

[^psflags]:  `ps` is too ancient to have long options which can represent these specific flags.
    * `l`: `Display BSD long format.`
    * `T`: `Select all processes associated with this terminal.`
[^restore-pgrp]: The bash instance running the script won't restore this when `zsh` exits because that's only something it does in interactive mode. Which hints at one solution to this problem: `bash -ic 'zsh -ic /bin/echo; read'` Although this actually will just move your problem up the stack, if you ever run this script itself in some other context where this may matter.
[^ptymod]: e.g.:
    ```
    python -c 'import pty; pty.spawn(["zsh", "-ic", "/usr/bin/echo"])'
    read
    ```
    
    But beware of the fact that doing this will merge stderr and stdout, although I guess you already can't guarantee that something else won't write to stdout in your original version.

---
