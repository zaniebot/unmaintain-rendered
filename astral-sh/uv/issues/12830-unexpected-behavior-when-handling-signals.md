```yaml
number: 12830
title: Unexpected behavior when handling signals
type: issue
state: closed
author: bustbr
labels:
  - bug
assignees: []
created_at: 2025-04-11T09:18:09Z
updated_at: 2025-04-23T23:42:59Z
url: https://github.com/astral-sh/uv/issues/12830
synced_at: 2026-01-10T03:41:47Z
```

# Unexpected behavior when handling signals

---

_Issue opened by @bustbr on 2025-04-11 09:18_

### Summary

Hello,

first off, I wasn't sure how to tag this; this is a bug report about unexpected behavior, with a feature request latched on as a solution proposal.

We recently switched our projects to use `uv` and ran into issues where on our servers **Python processes would stay alive in the background** after deploying new versions of our software onto our production servers.
It turned out this was the result of a chain of unexpected behaviors, the last one being how `uv` behaves when using `uv run` and how `uv` handles signals.

A quick **summary** of what I understand is happening:
When using `uv run` `uv` will run Python as a subprocess and stick around until Python quits, **or** until it receives any of a whole bunch of signals: `SIGKILL` (of course), but also `SIGHUP`, `SIGQUIT`, `SIGILL`, `SIGTRAP`, `SIGABRT`, ...
and for all of these `uv` simply quits, without passing the signal on to the underlying Python process, which leaves the Python process orphaned.
When `uv` receives `SIGINT` it will swallow the first signal it receives (#12108), which there might be good reasons for, but is also unexpected.
[All of this has been brought up in issues here before](https://github.com/astral-sh/uv/issues?q=is%3Aissue%20signal).

The best way that I found to avoid any of these issues seems to be to not use `uv run` and instead have `uv` only create the environment and then use the venv yourself, like
```bash
export VIRTUAL_ENV=.venv  # optional, but to be sure we know where the venv will end up
uv sync --frozen
"$VIRTUAL_ENV"/bin/python myscript.py
```

This seems like a complicated solution to me – the opposite of why I came to love `uv`. ;)

As mentioned before, this has been discussed in multiple issues already, and there seems to be no consensus how `uv` should handle signals better.
**Why does `uv` have to keep running** though? If `uv` simply replaced its own process with the Python process (`man 3 exec`), none of this would be an issue.
The [documentation](https://docs.astral.sh/uv/concepts/projects/run/#signal-handling) says
> uv does not cede control of the process to the spawned command in order to provide better error messages on failure.

which I'm not even sure about what that means.

Could we **add a new option** to `uv run` to in fact have `uv` do all its magic, and then quit when it spawns the Python process?
Something like
```bash
uv run --exec --frozen myscript.py
```
Of course a better name than `--exec` could be found, maybe `--no-subprocess`, or `--cede-control`.

### Platform

Darwin 24.4.0 arm64

### Version

uv 0.6.12 (Homebrew 2025-04-02)

### Python version

Python 3.13.1

---

_Label `bug` added by @bustbr on 2025-04-11 09:18_

---

_Comment by @jdtsmith on 2025-04-21 00:45_

I came here to report the same thing and found this related issue.  

I have a tool which sends `SIGUSR2` to `ipython` for a special type of interrupt handling.  uvx looks like a fantastic way to "drop in" on a project and run `ipython` there with all its packages, without adding a dependency.  But as mentioned, sending `SIGUSR2` to the process simply quits `uv` and leaves the child process as a zombie.   

Reproducer:

```
% uvx ipython           
Python 3.13.3 (main, Apr  9 2025, 03:47:57) [Clang 20.1.0 ]
Type 'copyright', 'credits' or 'license' for more information
IPython 9.1.0 -- An enhanced Interactive Python. Type '?' for help.
Tip: Use `F2` or %edit with no arguments to open an empty editor with a temporary file.

In [1]: def handle(*args):
   ...:     print("Handled!")
   ...: 

In [2]: import signal; signal.signal(signal.SIGUSR2, handle)
Out[2]: <Handlers.SIG_DFL: 0>

In [3]: 
[in other terminal window:  % killall -SIGUSR2 uv]
zsh: user-defined signal 2  uvx ipython
# <ipython closes>

% ps auxw | grep ipython  # but is not dead!  Zombie's are real.
__          12881   0.1  0.3 411005824  64928 s001  S     8:34PM   0:00.48 /Users/__/.cache/uv/archive-v0/xWuDZgj8cLGx5N8exlyTU/bin/python /Users/__/.cache/uv/archive-v0/xWuDZgj8cLGx5N8exlyTU/bin/ipython
```


I echo the need for an `--exec` option to `uv run` and `uv tool run` (possibly used as the default).  Once it has installed/located the tool with all its dependencies, is there any reason to keep the `uv` process around, vs. just `exec`ing the child process?  

_All sorts_ of python tools (e.g. coverage tools) use signals for various purposes.

---

_Assigned to @zanieb by @zanieb on 2025-04-21 02:27_

---

_Comment by @zanieb on 2025-04-21 15:37_

> Why does uv have to keep running though? If uv simply replaced its own process with the Python process (man 3 exec), none of this would be an issue.

We need to be able to clean up resources, such as temporary virtual environments. We could consider changing our strategy there, but it's complicated. Discussed previously at https://github.com/astral-sh/uv/issues/3095

---

_Comment by @zanieb on 2025-04-21 15:50_

> We recently switched our projects to use uv and ran into issues where on our servers Python processes would stay alive in the background after deploying new versions of our software onto our production servers.

How were you terminating your processes?

---

_Closed by @zanieb on 2025-04-21 19:47_

---

_Closed by @zanieb on 2025-04-21 19:47_

---

_Comment by @bustbr on 2025-04-22 10:10_

> > We recently switched our projects to use uv and ran into issues where on our servers Python processes would stay alive in the background after deploying new versions of our software onto our production servers.
> 
> How were you terminating your processes?

This is a bit complicated. 😬
We use [Fabric](https://www.fabfile.org/) and [Supervisor](https://supervisord.org/).
Using Fabric we copy the new code to the production machine(s), tell supervisor to stop all processes (which should send `SIGTERM` I think) and shut down completely, then start supervisord again loading the fresh configuration and starting the processes using supervisor. In our case we now have a supervisor config file that contains `uv run ...` as `command`.
So far so good.
But it turns out when Fabric disconnects it'll send `SIGHUP` to the last process it spawned, which in our case is Supervisor. And when Supervisord receives `SIGHUP` it will in turn forward that to all of its managed processes, so in our case `uv`.
... you know the rest.

We also found some work arounds, for example doing something else before we disconnect in Fabric, like `supervisord; :`, to avoid having SIGHUP being sent to supervisor.
Or by configuring `stopasgroup=true` in the supervisor.conf, so that any `SIGTERM` from supervisor would be sent to the whole process group – so not only `uv` but also it's `python` child. Of course that would also include any subprocesses our Python process spawns then. 😕
All of these would only fix our current very specific circumstances though, and I'm afraid the issue could pop back up again very easily.

Thank you for taking care of this! (#13017)

---

_Comment by @zanieb on 2025-04-22 14:12_

> Or by configuring stopasgroup=true in the supervisor.conf, so that any SIGTERM from supervisor would be sent to the whole process group

I think this is the proper configuration, fwiw. If you don't want other children to receive the signal, you can change their PGID with `setpgid`.

But yeah, it's complicated. Hopefully forwarding the signals solves most of these problems.

---

_Comment by @jdtsmith on 2025-04-23 23:42_

Just wanted to mention that signals are now flowing correctly via `uv` v0.6.16 for my application mentioned above.  Thanks very much for the quick work here!

---
