```yaml
number: 10384
title: Progress bar rendering through indicatif is slow
type: issue
state: open
author: konstin
labels:
  - performance
assignees: []
created_at: 2025-01-08T09:08:27Z
updated_at: 2025-01-09T10:15:52Z
url: https://github.com/astral-sh/uv/issues/10384
synced_at: 2026-01-12T16:00:12Z
```

# Progress bar rendering through indicatif is slow

---

_@konstin_

Currently, the uv resolver thread spends a considerable amount of time in `on_progress` -> `indicatif::style::ProgressStyle::format_state`. We should limit rendering progress bar updates to at most 1/60s, ideally directly in indicatif.

Profile for `samply record --rate 20000 target/profiling/uv pip compile scripts/requirements/airflow.in`:

![Image](https://github.com/user-attachments/assets/7baae942-1369-4ac4-98b7-c94b0cc2cb7e)


---

_Label `performance` added by @konstin on 2025-01-08 09:08_

---

_Comment by @jaheba on 2025-01-08 10:48_

To understand the issue: Currently, there is a call to `set_message` for each resolved package. If there are a lot of packages, we could end up calling `set_message` more than 60 times a second, slowing down the process. There is no benefit to display each resolved package, since the user can't process that information that quickly.


From what I understand, `indicatif` already is limited to [20 updates a second by default](https://github.com/console-rs/indicatif/blob/6417492caa546a91ca6f69881d33141ddaf192c2/src/draw_target.rs#L41-L43):

```rust
    /// Draw to a buffered stderr terminal at a max of 20 times a second.
    ///
    /// This is the default draw target for progress bars.  For more
    /// information see [`ProgressDrawTarget::term`].
    pub fn stderr() -> Self {
        Self::term(Term::buffered_stderr(), 20)
    }
```

However, it appears that the rate limiter only applies to progress bar ticks and not changing the text message. A call to `.set_message()` will always end in a call to `.draw()`.

This would mean that `ResolverReporter` would need to implement a rate limiter itself.

---

_Comment by @jaheba on 2025-01-08 13:26_

Actually, I think I was wrong.

There is a call to `rate_limiter.allow(now)` here: https://github.com/console-rs/indicatif/blob/main/src/draw_target.rs#L171

However, attempting to change the rate here did not show different profiles:

https://github.com/astral-sh/uv/blob/68adadf806ffca48d1542a89766d642ef38aee9f/crates/uv/src/printer.rs#L20

I've changed this to:

```rust
Self::Default => ProgressDrawTarget::stderr_with_hz(1),
```

and

```rust
Self::Default => ProgressDrawTarget::stderr_with_hz(1000),
```

@konstin Can you help me how to debug the issue? The profiles running your above command have quite some variance.

---

_Comment by @konstin on 2025-01-08 13:40_

I haven't looked into it in depth so i can't really help with the code.

Usually, i try to optimize by e.g. counting how often the function was called myself or by inserting `dbg!`s. For making the profiles more stable, you can try setting a different cpu govenor, on linux `taskset` to pin the process to performance cores and adjusting the samply sampling rate.

---

_Comment by @jaheba on 2025-01-08 17:00_

I've added a simple counter to invocations of `set_message`. There are 7384 invocations (packages that are reported), of which 24 are allowed to be sent to stderr. With a default of 20Hz that sounds about right.

If I set the frequency to the highest value (255), I get ~70 allowed events, however lower values also end up around 24 events.

In addition, there is a burst mechanism in indicatif, which I haven't looked at yet.

I think it's fair to say that there is not a lot of potential here by optimising the `stderr_with_hz` setting.

---

_Comment by @konstin on 2025-01-08 17:02_

It looks like most of the time is spent in rendering the message, it may be possible to skip or delay the rendering until we actually (this is only a guess from the profile though)

---

_Comment by @jaheba on 2025-01-09 10:15_

I've run some benchmarks using hyperfine on the file above:

|               | 0.5.15   | 0.5.16   | local    | local (noop) |
|---------------|----------|----------|----------|--------------|
| --show-output | 318.6 ms | 159.3 ms | 161.7 ms | 148.6 ms     |
| /dev/null     | 310.3 ms | 149.6 ms | 153.8 ms | 147.7 ms     |

What's most noticable is that there is a big jump from 0.5.15 to 0.5.16, and the latest release is just twice as fast.

The local-noop version does nothing on `on_progress` for the progress bar, so that should give us some indication what the overhead is. From this, it appears that there is about a 6ms overhead of using indicatif, even though the output is disabled. There is roughly similar penalty to actually render the output. (At least on my machine)

I've tried setting different values `stderr_with_hz`, and there is just no difference in the runtime. Higher refresh rates feel much snappier though.

I've also implemented a simple custom rate-limiter, that returns early in `on_progress`. It takes just shy of 150 ms using 50Hz, and ~151 using 100Hz. I guess we could shave off most of the overhead of indicatif.

---
