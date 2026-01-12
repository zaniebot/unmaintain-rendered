```yaml
number: 14462
title: "Threadpool initialization fails during basic builds on HPC cluster unless low `concurrent-builds` limit set"
type: issue
state: open
author: eah13
labels:
  - needs-mre
assignees: []
created_at: 2025-07-04T19:40:33Z
updated_at: 2025-07-07T16:25:02Z
url: https://github.com/astral-sh/uv/issues/14462
synced_at: 2026-01-12T16:01:49Z
```

# Threadpool initialization fails during basic builds on HPC cluster unless low `concurrent-builds` limit set

---

_@eah13_

### Summary

I hope this might be an easy fix towards the broader HPC support issues mentioned in #7642 

I'm testing out uv for use on the [Lonestar6](https://docs.tacc.utexas.edu/hpc/lonestar6/) HPC cluster at TACC. I got through some basic hello world installs quick enough to want to abandon pyenv and poetry forever, but also ran into this early on:

~~~bash
login1.ls6:~/smtest $ uvx ruff 
░░░░░░░░░░░░░░░░░░░░ [0/1] Installing wheels...                                                                                                                                                              memory allocation of 1520 bytes failed
memory allocation of 1520 bytes failed

thread 'main2' panicked at crates/uv-configuration/src/threading.rs:68:10:
failed to initialize global rayon pool: ThreadPoolBuildError { kind: IOError(Os { code: 11, kind: WouldBlock, message: "Resource temporarily unavailable" }) }
note: run with `RUST_BACKTRACE=1` environment variable to display a backtrace
Aborted (core dumped)
~~~

This is a MWE drawn from the docs, but it happened tool that needed a build. (I picked `snakemake` as a workflow-relevant test install)

I was able to fix this by limiting the number of concurrent builds. I added these lines to my user's `uv.toml` (would also have worked in the relevant `pyproject.toml`, command line args, etc.) and was able to move on:

~~~
#~/.config/uv.toml
concurrent-builds = 4
~~~

_Edit: if you're coming here with the same issue it seems this is not a solid fix. The root cause seems to be resource scheduling, which is why setting this and other config variables _help_ but don't guarantee a fix_

I can confirm that it fails again in the same way with `concurrent-builds =64` (half of the cores on the node). I want to be a good citizen on this system so even though I don't think this would get in anyone's way I didn't try anything higher than that, just in case. 

I'm not exactly sure why simple builds like this would fail without that config line. This was on a login node intended for compilation. There's some throttling to prevent people from forgetting to submit a compute job to the clusters, but the throttling limits are massive compared to the load here. FWIW my user doesn't seem very throttled, or if so it's not at the OS level `ulimit` can see:
<details><summary>Details</summary>

~~~bash
login1.ls6:~/smtest $ ulimit -a
core file size          (blocks, -c) 0
data seg size           (kbytes, -d) unlimited
scheduling priority             (-e) 0
file size               (blocks, -f) unlimited
pending signals                 (-i) 1028386
max locked memory       (kbytes, -l) unlimited
max memory size         (kbytes, -m) unlimited
open files                      (-n) 16384
pipe size            (512 bytes, -p) 8
POSIX message queues     (bytes, -q) 819200
real-time priority              (-r) 0
stack size              (kbytes, -s) unlimited
cpu time               (seconds, -t) unlimited
max user processes              (-u) 300
virtual memory          (kbytes, -v) 8388608
file locks                      (-x) unlimited
~~~

</details> 

Free speculation worth what you paid for it: Seems like [the relevant code](https://github.com/astral-sh/uv/blob/eaf517efd85f3fcaafe290ffc36615f4be8e04f9/crates/uv-configuration/src/threading.rs#L63-L69) could catch that `ThreadPoolBuildError`, infer that resource throttling might be getting in the way (or whatever the correct diagnosis is), and perhaps try again with smaller resource limits? 

tl;dr: uv threadpool initialization failed on multi tenant HPC systems with resource throttling. Seems easy to catch and retry with different limits or, at the very least, display an error message mentioning the configurations that addressed this issue.



### Platform

Rocky 8.4, 2x AMD EPYC 7763 64-Core 

### Version

uv 0.7.19

### Python version

Python 3.11.13

---

_Label `bug` added by @eah13 on 2025-07-04 19:40_

---

_Label `bug` removed by @konstin on 2025-07-07 12:38_

---

_Label `needs-mre` added by @konstin on 2025-07-07 12:38_

---

_Comment by @konstin on 2025-07-07 12:42_

> memory allocation of 1520 bytes failed

Are you memory constrained on the node?

We're using only standard rust tooling for parallelism (tokio, rayon, `std::thread`), so I'm surprised that the spawning the thread pool fails. I'm also surprised that `uvx ruff` is affected by concurrent builds, there are binary wheels for ruff so we shouldn't build at all, and the rayon thread pool is for installation, not for building.

---

_Comment by @eah13 on 2025-07-07 14:14_

> Are you memory constrained on the node?

No; check the `ulimit` output in the folded details above (fixed the formatting). And, let me tell you, when I've gotten it to run it's compiling super fast, in parallel. A thing of beauty.

Best I can tell, there's something about the 'one thread per CPU' heuristic that's baked into Rayon and common in Rust that's not playing well with these kinds of multi-tenant environments.

To me, a clue that there might be assumptions that don't hold baked into how the threadpool is built is this line:
~~~
{ code: 11, kind: WouldBlock, message: "Resource temporarily unavailable" }
~~~

"Temporarily" suggests the system's saying "wait a minute til I have those CPUs you asked for" and Rayon is currently interpreting that as the resource being unavailable and dying. The fix might be as simple as waiting a tick to have full access to the CPUs that it's requesting.

I don't know what the specific resource scheduling setup is, but I have some evidence this is a broader problem.
I've done a bit of looking and have seen a bunch of HPC folks (using other systems with similar architectures) reporting `ThreadPoolBuildError`s on a variety of projects, including many that are using uv. 
A few folks have reported similar errors on commercial hosting providers, so it's not just HPC. 

Is support for the kind of  resource scheduling or whatever is happening on systems like this one in scope for this project? 
If so, I'll reach out to our tech staff to get some more details on what kinds of behind-the-scenes resource scheduling type things might be fooling Rayon. 

Even though there's a potential upstream fix on this, I think that project will be justifiably conservative. Some more intelligent error handling to check for currently unexpected resource scheduling schemes would make uv more robust overall and way more likely to work out of the box on multi-tenant systems.

---

_Comment by @konstin on 2025-07-07 16:25_

> Is support for the kind of resource scheduling or whatever is happening on systems like this one in scope for this project?

We would like to support an as large as possible set of environments, including HPC environments. The main hindrance is that we don't have access to those systems, so we can't reproduce those problems or validate any patches for them.

> I don't know what the specific resource scheduling setup is, but I have some evidence this is a broader problem.
> I've done a bit of looking and have seen a bunch of HPC folks (using other systems with similar architectures) reporting `ThreadPoolBuildError`s on a variety of projects, including many that are using uv.
> A few folks have reported similar errors on commercial hosting providers, so it's not just HPC.

It would be great to fix this in rayon itself, the upstream error seems to be https://github.com/rayon-rs/rayon/issues/694.

---
