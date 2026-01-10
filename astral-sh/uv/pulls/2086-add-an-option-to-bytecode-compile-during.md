```yaml
number: 2086
title: Add an option to bytecode compile during installation
type: pull_request
state: merged
author: konstin
labels:
  - enhancement
  - compatibility
assignees: []
merged: true
base: main
head: konsti/compile-option
created_at: 2024-02-29T16:33:07Z
updated_at: 2024-03-05T09:48:49Z
url: https://github.com/astral-sh/uv/pull/2086
synced_at: 2026-01-10T14:54:43Z
```

# Add an option to bytecode compile during installation

---

_Pull request opened by @konstin on 2024-02-29 16:33_

Add a `--compile` option to `pip install` and `pip sync`.

I chose to implement this as a separate pass over the entire venv. If we wanted to compile during installation, we'd have to make sure that writing is exclusive, to avoid concurrent processes writing broken `.pyc` files. Additionally, this ensures that the entire site-packages are bytecode compiled, even if there are packages that aren't from this `uv` invocation. The disadvantage is that we do not update RECORD and rely on this comment from [PEP 491](https://peps.python.org/pep-0491/):

> Uninstallers should be smart enough to remove .pyc even if it is not mentioned in RECORD.

If this is a problem we can change it to run during installation and write RECORD entries.

Internally, this is implemented as an async work-stealing subprocess worker pool. The producer is a directory traversal over site-packages, sending each `.py` file to a bounded async FIFO queue/channel. Each worker has a long-running python process. It pops the queue to get a single path (or exists if the channel is closed), then sends it to stdin, waits until it's informed that the compilation is done through a line on stdout, and repeat. This is fast, e.g. installing `jupyter plotly` on Python 3.12 it processes 15876 files in 319ms with 32 threads (vs. 3.8s with a single core). The python processes internally calls `compileall.compile_file`, the same as pip.

Like pip, we ignore and silence all compilation errors (https://github.com/astral-sh/uv/issues/1559). There is a 10s timeout to handle the case when the workers got stuck. For the reviewers, please check if i missed any spots where we could deadlock, this is the hardest part of this PR.

I've added `uv-dev compile <dir>` and `uv-dev clear-compile <dir>` commands, mainly for my own benchmarking. I don't want to expose them in `uv`, they almost certainly not the correct workflow and we don't want to support them.

Fixes #1788
Closes #1559
Closes #1928

---

_Label `enhancement` added by @konstin on 2024-02-29 16:33_

---

_Label `compatibility` added by @konstin on 2024-02-29 16:33_

---

_Comment by @notatallshaw on 2024-02-29 19:30_

> The disadvantage is that we do not update RECORD and rely on this comment from [PEP 491](https://peps.python.org/pep-0491/):
> 
> > Uninstallers should be smart enough to remove .pyc even if it is not mentioned in RECORD.
> 
> If this is a problem we can change it to run during installation and write RECORD entries.

I would validate that pip successfully uninstalls the pyc files, because unless I am misunderstanding this issue it probably doesn't: https://github.com/pypa/pip/issues/11835

---

_Comment by @konstin on 2024-03-01 10:19_

I ran

```
cargo run venv --seed; cargo run pip install --compile tqdm
.\.venv\Scripts\pip uninstall -y tqdm
```

and the `tqdm` folder was fully removed.

---

_Review comment by @BurntSushi on `crates/uv/src/commands/pip_sync.rs`:355 on 2024-03-01 13:54_

Would it make sense to put this into a function instead of duplicating it for `pip install` and `pip sync`? It _looks_ like they are identical?

---

_Review comment by @BurntSushi on `crates/uv/tests/pip_sync.rs`:81 on 2024-03-01 13:55_

I think this can be `pypy` with https://github.com/astral-sh/uv/pull/2094 merged.

---

_Review comment by @BurntSushi on `crates/uv/tests/pip_sync.rs`:91 on 2024-03-01 13:56_

This overall looks pretty similar to code that @charliermarsh's has in https://github.com/astral-sh/uv/pull/2102. I wonder if we can reuse the logic from that PR once it's merged?

(I have the same comment here as I did there: are we sure conditional compilation is the right way to determine the directory here?)

---

_Review comment by @BurntSushi on `crates/uv/src/main.rs`:799 on 2024-03-01 14:33_

If I've already installed packages to a virtualenv and I forgot to add the `--compile` switch, is there an intended workflow for "compile what's in the virtualenv"? I guess maybe it's just `uv pip sync --compile`? If so, perhaps adding a mention of that workflow to the docs would be good.

---

_Review comment by @BurntSushi on `crates/uv-installer/src/compile.rs`:52 on 2024-03-01 14:55_

I might just use `1` here if `available_parallelism` fails.

---

_Review comment by @BurntSushi on `crates/uv-installer/src/compile.rs`:108 on 2024-03-01 15:06_

Usually, a channel send timing out should be regarded as a bug. That is, it should never happen. But I think I see why you're doing it here. From what I can tell, the subprocesses themselves might get stuck, and thus if all the workers get stuck, there is nobody to drain from the channel and thus the bounded send blocks when the buffer in the channel becomes full.

I'll suggest some ideas below for how to deal with this.

(It may be the case that we still want to put a timeout here so that we can be extra defensive. I have mixed feelings on that.)

---

_Review comment by @BurntSushi on `crates/uv-installer/src/compile.rs`:54 on 2024-03-01 15:07_

I wouldn't even bother with `* 10`.

You're probably find with a rendezvous channel (a capacity of `0`). Rendezvous channels are especially nice because they manifest logic errors in the structure of your concurrency very quickly. (Although I do think there are occasions where using a capacity of `1` can lead to simplifications that you couldn't otherwise do with a capacity of `0`.)

It's like how most times, we just use `vec![]` because you usually don't benefit from starting with some non-zero initial capacity.

---

_Review comment by @BurntSushi on `crates/uv-installer/src/compile.rs`:159 on 2024-03-01 15:16_

I think this means that anything written to stderr by the interpreter will get fed through to `uv`'s stderr right? If so, was that intentional? I don't think it's what I would expect. The messages could be quite confusing to users.

What I'd suggest is reading stderr in a separate Tokio task. That's I think the pretty standard solution for getting around the "so many things went wrong that the stderr pipe filled up and now the subprocess is blocked on writing to stderr and can't make progress" failure mode.

---

_Review comment by @BurntSushi on `crates/uv-installer/src/compile.rs`:177 on 2024-03-01 15:21_

I guess technically we'd want to use NUL terminated paths here, since a path can have a `\n` in it. (I'd also be okay with having an explicit failure mode for paths that contain a `\n` because they are kind of crazy.)

---

_Review comment by @BurntSushi on `crates/uv-installer/src/compile.rs`:212 on 2024-03-01 15:23_

If we have a timeout on reading from stdin, should we also have a timeout on writing to stdout?

---

_Review comment by @BurntSushi on `crates/uv-installer/src/compile.rs`:284 on 2024-03-01 15:28_

So looking at this code holistically, I feel like we might be able to simplify the timeout/cancellation logic here.

The way I would go about this is to think about the workers themselves controlling how long they're willing to wait for the subprocess to complete its task. You pick a timeout at that granularity. Then right before to start communicating with the subprocess, spawk a task that will kill the subprocess if it hasn't completed after N seconds. You should be able to implement this with `tokio::select!`.

If you do that, then I think you should be able to get rid of the timeouts almost everywhere else. It absolves of you needing to worry about writing to stdin taking too long, because the worker will kill the subprocess if it's truly blocked. When the subprocess gets killed, your code that is blocked on stdin should become unblocked immediately and everything should gracefully terminate.

---

_@BurntSushi reviewed on 2024-03-01 15:28_

Nice, this is awesome! I gave some suggestions on the concurrency structure as I _think_ it should be possible to simplify things.

---

_Review comment by @konstin on `crates/uv/src/main.rs`:799 on 2024-03-01 15:34_

I don't really consider this a use case; If you forgot, the first import will just take maybe a bit longer, nothing that justifies running an addional command. What i gather from the linked issues, our users will be people who create images where the startup time matters, who would use `--compile` for the last uv command in the image creation script.

---

_@konstin reviewed on 2024-03-01 15:34_

---

_@konstin reviewed on 2024-03-01 18:46_

---

_Review comment by @konstin on `crates/uv-installer/src/compile.rs`:54 on 2024-03-01 18:46_

A larger queue is significantly faster. I tried size 1, the number of workers, the number of workers by 10 and size 1000.

```
hyperfine --warmup 1 --prepare "target/profiling/uv-dev clear-compile .venv" "target/profiling/uv-dev1 compile .venv" "target/profiling/uv-devworkers compile .venv" "target/profiling/uv-devworkers10 compile .venv" "target/profiling/uv-dev1000 compile .venv"
Benchmark 1: target/profiling/uv-dev1 compile .venv
  Time (mean ± σ):     359.8 ms ±  11.5 ms    [User: 6757.8 ms, System: 1849.4 ms]
  Range (min … max):   346.8 ms … 380.1 ms    10 runs
 
Benchmark 2: target/profiling/uv-devworkers compile .venv
  Time (mean ± σ):     352.6 ms ±   8.1 ms    [User: 6856.0 ms, System: 1709.1 ms]
  Range (min … max):   337.5 ms … 368.1 ms    10 runs
 
Benchmark 3: target/profiling/uv-devworkers10 compile .venv
  Time (mean ± σ):     331.4 ms ±   5.9 ms    [User: 6723.6 ms, System: 1761.8 ms]
  Range (min … max):   323.9 ms … 340.1 ms    10 runs
 
Benchmark 4: target/profiling/uv-dev1000 compile .venv
  Time (mean ± σ):     331.0 ms ±   6.0 ms    [User: 6735.2 ms, System: 1738.5 ms]
  Range (min … max):   323.7 ms … 339.6 ms    10 runs
 
Summary
  target/profiling/uv-dev1000 compile .venv ran
    1.00 ± 0.03 times faster than target/profiling/uv-devworkers10 compile .venv
    1.07 ± 0.03 times faster than target/profiling/uv-devworkers compile .venv
    1.09 ± 0.04 times faster than target/profiling/uv-dev1 compile .venv
```

---

_@konstin reviewed on 2024-03-01 18:47_

---

_Review comment by @konstin on `crates/uv-installer/src/compile.rs`:159 on 2024-03-01 18:47_

Anything written to stderr is a failure mode, is there a good way to say: When there's anything written to stderr at this checkout point, exit the worker with stderr as error message?

---

_@konstin reviewed on 2024-03-01 19:00_

---

_Review comment by @konstin on `crates/uv-installer/src/compile.rs`:177 on 2024-03-01 19:00_

Good catch! Python seems to have no builtin handling for reading to null, i'll just skip newline paths. They couldn't have come from sane wheels anyway since you can't put them into RECORD files.

---

_@konstin reviewed on 2024-03-01 19:00_

---

_Review comment by @konstin on `crates/uv-installer/src/compile.rs`:212 on 2024-03-01 19:00_

I remove both.

---

_@konstin reviewed on 2024-03-01 19:22_

---

_Review comment by @konstin on `crates/uv-installer/src/compile.rs`:284 on 2024-03-01 19:22_

I've simplified the timeouts in a different way: Only the caller has a timeout for workers (or the channel) getting stuck, one when sending (due to backpressure) and one when waiting for the workers to join. This way it doesn't matter whether the python process is broken, there's a bug in the worker implementation or the tokio threadpool or the channel broke due to a panic, each case trips that timeout.

Placing it at the send might be an unusual positioning, but i think it's the best place to catch stuck workers.

---

_@konstin reviewed on 2024-03-01 19:27_

---

_Review comment by @konstin on `crates/uv-installer/src/compile.rs`:284 on 2024-03-01 19:27_

Note that either case is almost certainly a bug, either a real bug in uv or a quirk python interpreter we need to special case.

---

_@charliermarsh reviewed on 2024-03-03 20:10_

---

_Review comment by @charliermarsh on `crates/uv/tests/pip_sync.rs`:91 on 2024-03-03 20:10_

Ok with this since it's just for tests.

---

_@charliermarsh reviewed on 2024-03-03 20:10_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/pip_install.rs`:687 on 2024-03-03 20:10_

Let's move this above the change reporting, maybe? So that this summary comes at the end?

---

_@charliermarsh reviewed on 2024-03-03 20:10_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/mod.rs`:94 on 2024-03-03 20:10_

Nit: let's just call this `compile`? Feels more consistent with other commands. Can you add rustdoc too?

---

_@charliermarsh reviewed on 2024-03-03 20:12_

---

_Review comment by @charliermarsh on `crates/uv-installer/src/compile.rs`:47 on 2024-03-03 20:12_

Nit: `this` and `too easily`

---

_@charliermarsh reviewed on 2024-03-03 20:13_

---

_Review comment by @charliermarsh on `crates/uv-installer/src/compile.rs`:61 on 2024-03-03 20:13_

Our installer always removes `__pycache__`, so this should be ok, right?

---

_@charliermarsh approved on 2024-03-03 20:14_

I'm cool with the overall approach, but I defer to Andrew to review and approve the pipeline in `compile.rs`.

---

_Comment by @charliermarsh on 2024-03-03 23:09_

On second thought, I’m wondering if this should just be a stand-alone command. Since it runs over the whole env, there’s no reason it needs to be coupled to install / sync. 

---

_Comment by @konstin on 2024-03-04 10:21_

We can expose it as `uv compileall` (we can copy the dev command over), my only counterargument is not extending our external interface more. I'd like to keep the `--compile` flag either way to keep the install operation a single command.

Fwiw there's also `python -m compileall`, it does the same thing just slower:

```
$ hyperfine --prepare "uv venv && uv pip install jupyter plotly" "python -m compileall .venv/lib/python3.12/sit
e-packages/"
Benchmark 1: python -m compileall .venv/lib/python3.12/site-packages/
  Time (mean ± σ):      5.407 s ±  1.026 s    [User: 4.354 s, System: 0.427 s]
  Range (min … max):    4.668 s …  7.699 s    10 runs
 ```

---

_@konstin reviewed on 2024-03-04 10:34_

---

_Review comment by @konstin on `crates/uv-installer/src/compile.rs`:61 on 2024-03-04 10:34_

Yes, i tested with django and it got removed without residues.

---

_@BurntSushi reviewed on 2024-03-04 15:17_

---

_Review comment by @BurntSushi on `crates/uv-installer/src/compile.rs`:54 on 2024-03-04 15:17_

That is quite interesting. I stand corrected. It might be good to add a comment about it here (no need to include the full benchmark results though).

To me I think this implies that the directory traversal isn't feeding file paths fast enough into the workers, right? And I think that would be exacerbated with a high core count. It means there are likely idle workers in the low capacity case where as those workers get work more quickly in the high capacity case. Very interesting.

---

_@BurntSushi reviewed on 2024-03-04 15:19_

---

_Review comment by @BurntSushi on `crates/uv-installer/src/compile.rs`:159 on 2024-03-04 15:19_

I would do that in a task dedicated to reading stderr. If that task reads anything, then you should be able to devise a mechanism to communicate that back to the main worker thread, have it kill the sub-process and return whatever was on stderr.

I'd only do this though if you're sure that _anything_ written to stderr should cause the entire enterprise to fail. e.g., What if stderr just contains a warning or something? (But maybe that can't happen here. IDK.)

---

_@zanieb reviewed on 2024-03-04 15:39_

---

_Review comment by @zanieb on `crates/uv-installer/src/compile.rs`:68 on 2024-03-04 15:39_

Can you scope this temporary directory to somewhere in the cache?

See https://github.com/astral-sh/uv/pull/1628

---

_@zanieb reviewed on 2024-03-04 15:43_

---

_Review comment by @zanieb on `crates/uv-installer/src/compile.rs`:177 on 2024-03-04 15:43_

You could also just flush on the Python end when you call `print`?

---

_@BurntSushi approved on 2024-03-04 15:52_

I'm fine with this landing as-is, but I'm still overall skeptical of how the channel timeouts are being used here.

My understanding is that if compilation takes longer than the channel timeout _overall_ (~10s), then the channel timeout could trip and thus cause the entire enterprise to fail even if it would have otherwise succeeded. Whether or not that's a real problem in practice is hard to say, but it feels non-ideal to be placing bounds on things that we don't really control. That is, the channel timeout is conflating the possibility of bugs (in both our code and perhaps in how the Python interpreter behaves) with the amount of time it takes for the user's resources to bytecode-compile a set of Python files.

I do think the better approach here is to deal with problems with byte-code compiling at the level of the subprocess. And specifically, only use timeouts when something you _know_ could legitimately hang (like, say, killing a subprocess).

With that said, without user reports, it's hard to know whether the timeouts will be an issue in practice.

---

_Review comment by @konstin on `crates/uv-installer/src/compile.rs`:177 on 2024-03-04 17:13_

Yes, that would also work

---

_@konstin reviewed on 2024-03-04 17:13_

---

_Comment by @konstin on 2024-03-04 17:44_

> I do think the better approach here is to deal with problems with byte-code compiling at the level of the subprocess. And specifically, only use timeouts when something you know could legitimately hang (like, say, killing a subprocess).

The problem i'm mainly thinking about is when the python process is in some regard broken, e.g. image `python` is replaced by a bash script that does `> /dev/null` (this isn't the case, but i've seen `python`s that were not pythons or behaved strangely for some other reason).  I'm less worried about OS operation such as killing a subprocess.

The second problem i'm thinking about is e.g. a panic in one of the threads that makes tokio behave oddly or a concurrency bug on our end, even i don't have good insight about the failure modes here.

I feel this is a trade-off we have to make: We can put no timeout and expect the user to kill the process (or the CI to timeout) vs. we specify and unreasonably high limit after which we abort (similar to the timeout we put on http requests). If we want to put this in control of the user, i would remove the timeouts.

---

_Review comment by @konstin on `crates/uv-installer/src/compile.rs`:159 on 2024-03-04 18:11_

I've added handling to stderr that will
* do nothing if there was no stderr,
* debug print if there was stderr but exit status passed,
* show the stderr in the error chain if there was stderr print and an error.

---

_@konstin reviewed on 2024-03-04 18:11_

---

_Comment by @BurntSushi on 2024-03-04 18:19_

> The problem i'm mainly thinking about is when the python process is in some regard broken, e.g. image `python` is replaced by a bash script that does `> /dev/null` (this isn't the case, but i've seen `python`s that were not pythons or behaved strangely for some other reason). I'm less worried about OS operation such as killing a subprocess.

I think for cases like this, I'd be happier with a large timeout at a more granular level. e.g., a ~60 second timeout within the worker. So after 60 seconds of no output, the worker would kill the subprocess and report an error to the user. The 60 second wait isn't great, but the user will at least eventually get some kind of error message instead of a truly indefinite hang.

If this were a common failure mode then this wouldn't be great. But if a Python process is as badly broken as `> /dev/null`, I think I'm more-or-less okay with having to wait longer before saying something to the user.

And since the timeout is applied more granularly (at the level of a single subprocess), you could feasibly decrease it.

> The second problem i'm thinking about is e.g. a panic in one of the threads that makes tokio behave oddly or a concurrency bug on our end, even i don't have good insight about the failure modes here.

I don't have as much experience with panics in Tokio, but I think it will catch them and the task will quit and you'll get a `JoinError`. But I defer to you on this one. You probably have more practical hands-on experience than I do here.

> I feel this is a trade-off we have to make: We can put no timeout and expect the user to kill the process (or the CI to timeout) vs. we specify and unreasonably high limit after which we abort (similar to the timeout we put on http requests). If we want to put this in control of the user, i would remove the timeouts.

Yeah to be clear, I think it's fine to have timeouts _somewhere_. I'm mostly more uneasy with having them at a higher level. But it could all be fine. I just don't know.

---

_@BurntSushi reviewed on 2024-03-04 18:23_

---

_Review comment by @BurntSushi on `crates/uv-installer/src/compile.rs`:159 on 2024-03-04 18:23_

Nice, LGTM!

---

_@charliermarsh reviewed on 2024-03-04 21:39_

---

_Review comment by @charliermarsh on `crates/uv-installer/src/compile.rs`:68 on 2024-03-04 21:39_

Yeah this definitely needs to be scoped.

---

_Comment by @charliermarsh on 2024-03-05 03:06_

This LGTM, though I'm also curious if we can move the timeout down a level?

---

_@charliermarsh reviewed on 2024-03-05 03:30_

---

_Review comment by @charliermarsh on `crates/uv-installer/src/compile.rs`:68 on 2024-03-05 03:30_

Done

---

_Comment by @charliermarsh on 2024-03-05 03:32_

I moved the temporary directory into the cache just to be safe. I'll merge this to keep it moving.

---

_Merged by @charliermarsh on 2024-03-05 03:35_

---

_Closed by @charliermarsh on 2024-03-05 03:35_

---

_Branch deleted on 2024-03-05 03:35_

---

_Comment by @konstin on 2024-03-05 09:48_

> I moved the temporary directory into the cache just to be safe.

What's the problem we're avoiding this way?

---
