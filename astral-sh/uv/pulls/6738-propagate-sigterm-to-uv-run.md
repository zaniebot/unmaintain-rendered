```yaml
number: 6738
title: "Propagate SIGTERM to `uv run`"
type: pull_request
state: closed
author: charliermarsh
labels:
  - bug
assignees: []
base: main
head: charlie/sigterm
created_at: 2024-08-28T02:48:53Z
updated_at: 2024-11-12T02:47:30Z
url: https://github.com/astral-sh/uv/pull/6738
synced_at: 2026-01-10T11:59:59Z
```

# Propagate SIGTERM to `uv run`

---

_Pull request opened by @charliermarsh on 2024-08-28 02:48_

## Summary

See: https://github.com/astral-sh/uv/issues/6724

## Test Plan

(Needs testing.)


---

_@zanieb approved on 2024-08-28 12:39_

Makes sense, should we be forwarding _all_ signals? 

---

_Label `bug` added by @zanieb on 2024-08-28 12:39_

---

_Comment by @charliermarsh on 2024-08-28 12:51_

I don't think this forwards them per se, it just kills the process if we see them. Other signals might have different meanings, right...?

---

_Comment by @zanieb on 2024-08-28 13:07_

Oh.. hm. I'll need to look closer.

---

_Review comment by @zanieb on `crates/uv/src/commands/tool/run.rs`:220 on 2024-08-28 14:08_

I think you need to remove the above `ctrl_c()` handler if we're going to replicate it here, right?

---

_@zanieb reviewed on 2024-08-28 14:08_

---

_Comment by @zanieb on 2024-08-28 14:09_

Reading about this in https://unix.stackexchange.com/questions/176235/fork-and-how-signals-are-delivered-to-processes 

Seems like both the child and parent should receive the signal already? That's why above we say:

> // Ignore signals in the parent process, deferring them to the child. 

And just eat all the SIGINT we receive in the parent process.

---

_Comment by @zanieb on 2024-08-28 14:14_

More interesting discussion in https://github.com/docker/compose/issues/10898#issuecomment-1680334699 and https://github.com/docker/cli/issues/4402#issuecomment-1681236915

> I think that when the CLI receives a signal which is customarily used to ask processes to gracefully terminate, the CLI should oblige; but it's been pointed out that if the CLI were to forward signals to its children (which would be the easiest solution), then plugins would receive two signals in cases where the signal is sent to the entire process group (such as when pressing Ctrl+C in the terminal) - which would break the existing and, as I understand it, desirable behaviour of plugins (shutdown gracefully on first signal, shutdown fast on second).

---

_Comment by @zanieb on 2024-08-28 14:16_

Basically — I think we should be forwarding all signals to the child process but we'll need an exception for SIGINT in an interactive session because it's probably _already_ been sent to the child process by a shell. 😭 

---

_Comment by @overfl0 on 2024-08-30 22:00_

Hi @charliermarsh , I have tested this PR the same way as I stated in https://github.com/astral-sh/uv/issues/6724 and sadly it doesn't seem to actually fix the issue 😬 
The service still takes 10 seconds to stop, for some reason.

Maybe it's not about sigterm after all? I can't really say. All I know is that the service doesn't stop when you Ctrl+C the service started with docker compose

---

_Comment by @charliermarsh on 2024-08-30 22:09_

Thanks @overfl0! I appreciate you trying it out. I can also repro the issue, though hadn't had a chance to test out this fix.

---

_Comment by @mvaled on 2024-09-03 07:31_

@charliermarsh 

I wonder if it wouldn't be better to replace the current process completely (a la [`exec`](https://www.man7.org/linux/man-pages/man3/exec.3.html)).  That would remove the need of meddling the signals.

Disclaimer: I'm not sure the about the overall design of `uv run`.

I see that `rye` used this exec approach (not using `tokio`, though):

```rust
/// Spawns a command exec style.
pub fn exec_spawn(cmd: &mut Command) -> Result<Infallible, Error> {
    // this is technically only necessary on windows
    crate::disable_ctrlc_handler();

    #[cfg(unix)]
    {
        use std::os::unix::process::CommandExt;
        let err = cmd.exec();
        Err(err.into())
    }
    #[cfg(windows)]
    {
        cmd.stdin(Stdio::inherit());
        let status = cmd.status()?;
        std::process::exit(status.code().unwrap())
    }
}
```

What would be the pros/cons of not using Tokio's `Command` and use the stdlib?

---

_Comment by @zanieb on 2024-09-03 17:36_

That's also discussed briefly in #3095 

---

_@bluss reviewed on 2024-09-04 10:40_

---

_Review comment by @bluss on `crates/uv/src/commands/tool/run.rs`:234 on 2024-09-04 10:40_

Wouldn't it be important to preserve the signal kind, so still a SIGINT to the child?

---

_Comment by @pkucmus on 2024-09-13 07:55_

I was pointed here from [another issue](https://github.com/astral-sh/uv/issues/7343) with something that might be related. So just to leave it here, did you consider how a Uvicorn (or other webservers) deployed application might be used with uv? In the context of signal control of the server that is. [Sending SIGHUP, SIGTTIN, SIGTTOU to your container to scale Uvicorn](https://www.uvicorn.org/deployment/#built-in) to the container from it's orchestration layer like Docker Swarm has meaning as only the PID 1 process in the container will receive the signal, in the case of `uv run` uv is running as PID 1 and would need to propagate at least as few signals to it's subprocess webserver for it to for example gracefully shut down.

Here's some context: https://www.kaggle.com/code/residentmario/best-practices-for-propagating-signals-on-docker

Anyway I don't think it's something I can help with but wanted to point it out - maybe it's a separate issue, don't know how you guys would rather track that but I think it's an important thing to consider. Thanks.

---

_Comment by @pkucmus on 2024-09-13 07:57_

Maybe documenting this consideration would be a good idea?

---

_Comment by @cr-klarna on 2024-10-31 08:52_

Hello all, wondering what's missing here for this to be folded into the next release? I see some checks are failing but also that it was approved. If this ensures some propagation it would be super helpful for my team to have as right now we see the same behavior in the original reference issue re: dockerized processes and we depend on being able to properly gracefully shutdown rather than have a forced kill after the sigterm wait period. 

---

_Comment by @zanieb on 2024-10-31 13:24_

This is missing a design and subsequent changes to address my comments at https://github.com/astral-sh/uv/pull/6738#issuecomment-2315451358

---

_Comment by @overfl0 on 2024-10-31 13:46_

@zanieb  I think what's more important is that this PR doesn't actually fix the issue, see my comment below yours: https://github.com/astral-sh/uv/pull/6738#issuecomment-2322406099
No idea why it doesn't work, but the fact is that when I built and tested this PR, I still experienced the old behaviour.

---

_Comment by @bluss on 2024-11-01 11:09_

> Basically — I think we should be forwarding all signals to the child process but we'll need an exception for SIGINT in an interactive session because it's probably _already_ been sent to the child process by a shell. 😭

jupyter-client also sends signals to the process group, so I'm worried about that one too if running the ipykernel using `uv run`. I'm available to test any proposed changes for that scenario though. jupyter-client tells ipykernel (on non-windows) about the user desire to stop current computation using SIGINT by default.

---

_Comment by @cr-klarna on 2024-11-08 08:20_

> @zanieb I think what's more important is that this PR doesn't actually fix the issue, see my comment below yours: [#6738 (comment)](https://github.com/astral-sh/uv/pull/6738#issuecomment-2322406099) No idea why it doesn't work, but the fact is that when I built and tested this PR, I still experienced the old behaviour.

took another look at this and I also confirmed that when sending a signal it was not passed to the underlying process as expected. I now see what @zanieb mentioned with some interactive shells passing signals directly when done from an interactive context but I'm still not convinced that's a big deal really. at this point I'd rather receive double the signals than the current behavior

---

_Comment by @cr-klarna on 2024-11-08 09:36_

A few things after exploring where things are going wrong. Keep in mind I do not know rust and am only looking at it because this tool is written in it; may need some help to confirm my findings.

1. Regarding why some of us see signals not being passed
    - this is because the change was applied only to the `tool/run.rs` file and is also needed in the `project/run.rs` file as well. 
2. Why we still see issues with process behavior when signals get handled
    - this seems to be because afaict this tokio library does not care about proper signal handling and only exposes a kill API method which does exactly that. Not great, but I see some workarounds around the rust community
3. when point 2 is addressed this code still assumes too much on handling only one signal ever then exiting
    - and would need to change to have some sort of looping to readily handle more than one signal when initial signals do not result in child termination 

I am slow in rust as mentioned but I have been looking at this with earnest, though I recognize someone that actually knows this language will be much faster to take them if indeed these are the right things to address

---

_Comment by @shaneikennedy on 2024-11-08 11:46_

@charliermarsh @zanieb sorry for the dup PR but didn't want to push to your branch (and not sure I could anyways) I added a few commits and I believe I have a fix ready here https://github.com/astral-sh/uv/pull/8933, though I might need some guidance incase CI fails

---

_Closed by @zanieb on 2024-11-12 02:47_

---
