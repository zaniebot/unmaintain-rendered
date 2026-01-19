```yaml
number: 22350
title: "[Panic] - Error Encountered"
type: issue
state: closed
author: orrdjoshua
labels:
  - question
assignees: []
created_at: 2026-01-03T03:04:25Z
updated_at: 2026-01-19T08:51:41Z
url: https://github.com/astral-sh/ruff/issues/22350
synced_at: 2026-01-19T09:26:50Z
```

# [Panic] - Error Encountered

---

_@orrdjoshua_

Got this error:

```
error: Ruff crashed. If you could open an issue at:

    https://github.com/astral-sh/ruff/issues/new?title=%5BPanic%5D

...quoting the executed command, along with the relevant file contents and `pyproject.toml` settings, we'd be very appreciative!


thread 'main' panicked at /root/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/rayon-core-1.12.1/src/registry.rs:168:10:
The global thread pool has not been initialized.: ThreadPoolBuildError { kind: IOError(Os { code: 11, kind: WouldBlock, message: "Resource temporarily unavailable" }) }
note: run with `RUST_BACKTRACE=1` environment variable to display a backtrace
```


I did not capture what command exactly was being run, as it was running within an automated coding platform, but it should have been executed from this source, the directory/path for the file to be linted was dynamically determined:

```py   
    if file_type == "python":
        try:
            try:
                cmd = ["ruff", "check", abs_path, "--fix"]
                result = subprocess.run(cmd, capture_output=True, text=True, timeout=20)
                linter_type = "ruff"
            except Exception:
                cmd = ["flake8", abs_path, "--max-line-length=120"]
                result = subprocess.run(cmd, capture_output=True, text=True, timeout=20)
                linter_type = "flake8"
            output = (result.stdout or "") + ("\n" + result.stderr if result.stderr else "")
            success = result.returncode == 0
```

The real curiosity is that really this seemed to have occurred at a deeper level because of some strange unknown issue/crash within Git itself in this repository while we were operating the system.  Not sure what happened, but I was getting some random git errors simultaneously with this (pretty large repo).  

Anyway, just following directions by sending this in.


---

_Comment by @ntBre on 2026-01-04 00:57_

Thanks for the report!

This is interesting. Assuming the response [here](https://github.com/rayon-rs/rayon/issues/579) is still relevant, I think this was caused by the OS failing to create a thread. I found a couple of other potentially related issues, including one from uv:

- https://github.com/rayon-rs/rayon/issues/694
- https://github.com/astral-sh/uv/issues/14462

You could try setting the `RAYON_NUM_THREADS` environment variable to a smaller number than the number of cores, which is the default.

---

_Label `bug` added by @ntBre on 2026-01-04 00:58_

---

_Comment by @ntBre on 2026-01-04 01:10_

Probably not too much help, but I can at least reproduce a similar panic with a command like:

```shell
❯ (ulimit -u 16; ruff check crates/ruff_linter/resources/test/cpython)

error: Ruff crashed. If you could open an issue at:

    https://github.com/astral-sh/ruff/issues/new?title=%5BPanic%5D

...quoting the executed command, along with the relevant file contents and `pyproject.toml` settings, we'd be very appreciative!


thread 'main' (2362097) panicked at /usr/src/debug/rust/rustc-1.92.0-src/library/std/src/thread/scoped.rs:199:46:
failed to spawn thread: Os { code: 11, kind: WouldBlock, message: "Resource temporarily unavailable" }
note: run with `RUST_BACKTRACE=1` environment variable to display a backtrace
```

---

_Label `bug` removed by @MichaReiser on 2026-01-04 17:39_

---

_Label `question` added by @MichaReiser on 2026-01-04 17:39_

---

_Comment by @MichaReiser on 2026-01-04 17:47_

Can you say more about your environment? E.g. did you set a thread or open file limit? Is this error reproducible or only happens occasionally? 

---

_Comment by @orrdjoshua on 2026-01-05 19:26_

@MichaReiser  I'm not a very experienced person, so I'm not sure exactly how to answer, but I can say that:

1. I run the server on WSL2 in windows 11 via WSL plugin of vscode
2. The program is a fastAPI server running async python code
3. receiving the Ruff error didn't crash or hang the server (error was captured in the code branch and saved as output from the "linter attempt" branch in my system)
4. not sure if I hit any "thread" limits but did not hit any open file limits in vscode.  If your talking about async threadpool or coroutine limits, I don't think I hit any of those in the server as I was just running one copy of it locally and not doing anything particularly excessive, also the error continued to occur over time during variety of use.
5. I have not been able to reproduce the error
6. I did not call ruff directly myself in bash but was using subprocess.run within a module for auto-linting files after code patches had been auto-applied from an LLM-provided-response.  The whole system is deeply automated (think "codex" but homebuilt and running locally)

Took me a while to notice that the linter was failing with this error, and restarting the fastAPI server did not resolve the issue, and it happened consistently during the situation (i.e. probably dozens of times over hours of automation).  It never worked again until I restarted IDE (vscode) itself.  I THINK that's what solved it.  Possibly it didn't work again until I actually restarted the computer, but I think restarting vscode worked.

I was getting lots of EAGAIN error pop-ups within vscode for GIT.  These were coming up because of automated commit commands occurring in worktrees or background git tasks from using source control in vscode.

My guess is that there was some kind of issue with the connection between WSL/bash server/Windows environment...  Likely due to leaving the fastAPI server running and the WSL port in vscode connected over probably 24 hours and multiple computer sleep/hibernate cycles...  

but that doesn't really explain the ruff errors seemingly since that would have all been within the same process... strangely none of these errors prevented me from continuing my work for hours (except that I couldn't lint python files from within the system, but vscode linting extensions still worked), so after that got annoying enough I restarted vscode and the issue went away...  In over 6 months of use in the same environment I've never had this issue before.

---

_Comment by @MichaReiser on 2026-01-07 09:34_

This sounds to me like your system run out of file handles or memory, so that the OS failed to spawn new threads. Which also explains why restarting your computer or restarting whatever program is responsible for the high resource usage resolved the issue. There isn't anything we can do here on the Ruff side other than catching the error more gracefully, but that would require upstream changes in rayon.

---

_Closed by @MichaReiser on 2026-01-19 08:51_

---
