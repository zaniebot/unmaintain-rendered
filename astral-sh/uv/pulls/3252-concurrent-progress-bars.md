```yaml
number: 3252
title: Concurrent progress bars
type: pull_request
state: merged
author: ibraheemdev
labels:
  - tracing
  - cli
assignees: []
merged: true
base: main
head: concurrent-progress-bar
created_at: 2024-04-24T20:11:54Z
updated_at: 2024-05-28T20:18:34Z
url: https://github.com/astral-sh/uv/pull/3252
synced_at: 2026-01-10T14:32:20Z
```

# Concurrent progress bars

---

_Pull request opened by @ibraheemdev on 2024-04-24 20:11_

## Summary

Implements concurrent progress bars. Resolves https://github.com/astral-sh/uv/issues/1209.

## Test Plan

https://github.com/astral-sh/uv/assets/34988408/b21bdfbb-8817-4873-a65c-16c9e8c7c460

---

_Marked ready for review by @ibraheemdev on 2024-04-24 22:18_

---

_Comment by @T-256 on 2024-04-25 09:17_

could it be sorted (the largest at top)?

---

_Comment by @ibraheemdev on 2024-04-25 17:11_

@T-256 I put the largest at the bottom which looks more natural, in my opinion.


https://github.com/astral-sh/uv/assets/34988408/52915706-5349-48b4-82e4-a72f8b1c9846

---

_Review requested from @charliermarsh by @ibraheemdev on 2024-04-25 17:12_

---

_Label `tracing` added by @zanieb on 2024-04-25 23:38_

---

_Label `cli` added by @zanieb on 2024-04-25 23:38_

---

_Comment by @charliermarsh on 2024-04-26 00:20_

The code generally looks good (well done) but need to find a bit of time to play with this myself to see if I have any feedback on the UX.

---

_Comment by @ibraheemdev on 2024-04-30 18:20_

Just tested this on a large requirements file, I think any downloads <250kb shouldn't be displayed. The output changes too fast for it to be useful.

---

_Comment by @codspeed-hq[bot] on 2024-04-30 18:20_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/uv/branches/ibraheemdev:concurrent-progress-bar)

### Merging #3252 will **not alter performance**

<sub>Comparing <code>ibraheemdev:concurrent-progress-bar</code> (0ab84fe) with <code>main</code> (080f095)</sub>



### Summary

`✅ 13` untouched benchmarks






---

_Comment by @ibraheemdev on 2024-04-30 18:39_

We should also probably provide a way to disable this output.

---

_Comment by @charliermarsh on 2024-04-30 18:42_

We could consider making it opt-in. (That would also make it less risky to ship.)

---

_Comment by @zanieb on 2024-04-30 18:49_

What's the risk, specifically? It seems improbable that this will break people's workflows right?

---

_Comment by @zanieb on 2024-04-30 18:50_

We do have a preview option now though, if you really want to gate it. A dedicated setting might make sense too? but opt-in seems wrong for that.

---

_Comment by @charliermarsh on 2024-04-30 18:54_

I think the risk is just that it’s overly verbose for most package installs and may require tuning. I would be fine shipping it under preview so we can get feedback on the UI/UX. I haven’t had a chance to play with it myself yet though.

---

_Comment by @zanieb on 2024-04-30 19:07_

I don't mind iterating on display ux without opt-in, it feels much safer than other changes we make and we release very often.

---

_Comment by @zanieb on 2024-05-03 12:56_

Is this waiting on any feedback?

---

_Comment by @charliermarsh on 2024-05-03 13:16_

Yes, from me at least - not sure if others are planning to review.

---

_Comment by @zanieb on 2024-05-23 14:49_

I'd love to get this merged, I don't think it's great to have it hanging for weeks. Anything I can do to move this forward?

---

_Comment by @charliermarsh on 2024-05-23 14:52_

Ultimately it's on me to review. However you could test and give feedback on the UX -- it would make it higher-confidence for me if we could get more eyes and opinions on it.

---

_Comment by @zanieb on 2024-05-23 15:17_

I did test it and was happy with it but I can do more investigation.

@ibraheemdev can you rebase the branch?

---

_Comment by @charliermarsh on 2024-05-23 15:24_

I will review and merge today.

---

_Comment by @ibraheemdev on 2024-05-23 15:28_

I'll rebase. I am a little concerned about the output when you have a lot of small downloads though, e.g. https://github.com/astral-sh/uv/issues/2706#issuecomment-2115781002. Even kitty was flickering a little. We can probably buffer updates to the terminal a bit.

---

_Comment by @zanieb on 2024-05-23 15:42_

If anyone has "good" test cases post 'em here so we have some common ground when considering the behavior.

---

_Comment by @zanieb on 2024-05-23 15:42_

I'm also supportive of including an opt-out environment variable.

---

_Comment by @charliermarsh on 2024-05-23 16:07_

I would still be fine making it opt-in if we want to get more feedback before stabilizing :)

---

_Comment by @zanieb on 2024-05-23 16:56_

Testing with

```
cargo run -q -- pip install anyio --reinstall --no-cache
cargo run -q -- pip install pydantic --reinstall --no-cache
cargo run -q -- pip install prefect --reinstall --no-cache
cargo run -q -- pip install pyqt5 --reinstall --no-cache
```

All good to me

---

_Comment by @T-256 on 2024-05-23 20:16_

LGTM and as Charlie said, it could be opt-in via `--preview` and after stabilization make it default behavior. Also, as Zanie said we can have an option to opt-out from it.

> We can probably buffer updates to the terminal a bit.

Let's get some feedback from users, then we can set this interval value with more confidence.

---

_Comment by @charliermarsh on 2024-05-23 21:42_

Should we just move behind preview to unblock the decision? @ibraheemdev do you _want_ it to ship to stable?

---

_Comment by @ibraheemdev on 2024-05-23 22:40_

I'm fine with merging this behind preview or an opt-in flag, as long as it's accessible because it can be useful for debugging.

---

_Comment by @zanieb on 2024-05-23 23:13_

I have a medium preference for shipping it in stable with an opt-out, but that requires a separate mechanism than `--preview` 🤷‍♀️ 

---

_Comment by @charliermarsh on 2024-05-23 23:25_

Let's just move it to `--preview` to get it unblocked I think.

---

_Comment by @charliermarsh on 2024-05-24 03:12_

I like how pip's progress bars are left-aligned and have a default width -- as-is the display is too wide IMO on my monitor:

![Screenshot 2024-05-23 at 11 11 30 PM](https://github.com/astral-sh/uv/assets/1309177/81964a17-6249-45eb-a9ea-0d9f9f1f666f)

![Screenshot 2024-05-23 at 11 12 18 PM](https://github.com/astral-sh/uv/assets/1309177/4dd53350-1ef6-46cd-ad59-e394a0d4ba67)

How hard is it to achieve something like that?


---

_Comment by @charliermarsh on 2024-05-24 03:15_

To achieve that, I guess we'd need to put the package name above the bar, or to the right of it, to achieve consistent alignment?

---

_Comment by @charliermarsh on 2024-05-24 03:16_

Or use a fixed size for the package name. I can play with it.

---

_Comment by @charliermarsh on 2024-05-24 03:29_

Here's a slightly different layout. The colors are washed out in the video but it's green on dim:

https://github.com/astral-sh/uv/assets/1309177/b7ada082-cb52-4553-9b1c-1d2e52d1bc62



---

_Comment by @charliermarsh on 2024-05-24 03:29_

Thoughts?

---

_@charliermarsh reviewed on 2024-05-24 03:31_

---

_Review comment by @charliermarsh on `crates/uv-distribution/src/distribution_database.rs`:494 on 2024-05-24 03:31_

I feel like I'm so allergic to unwrap that I'd probably do something like:

```rust
let progress = self
    .reporter
    .as_ref()
    .map(|reporter| (reporter, reporter.on_download_start(dist.name(), size)));
```

And then use `match self.progress` everywhere. What's your instinct?

---

_Review comment by @charliermarsh on `crates/uv-distribution/src/distribution_database.rs`:906 on 2024-05-24 03:31_

Why not parse as `u64`? Just wondering.

---

_@charliermarsh reviewed on 2024-05-24 03:31_

---

_Comment by @ibraheemdev on 2024-05-24 03:57_

@charliermarsh your design looks quite nice, I would be happy to switch to that.

---

_Comment by @charliermarsh on 2024-05-24 13:58_

Sounds good, I'll merge this today. I looked at gating behind preview but honestly looks like kind of a pain haha.

---

_Comment by @charliermarsh on 2024-05-24 13:59_

We can add a `--no-progress` flag?

---

_@ibraheemdev reviewed on 2024-05-24 18:59_

---

_Review comment by @ibraheemdev on `crates/uv-distribution/src/distribution_database.rs`:906 on 2024-05-24 18:59_

Should be `u64`, fixed.

---

_@ibraheemdev reviewed on 2024-05-24 18:59_

---

_Review comment by @ibraheemdev on `crates/uv-distribution/src/distribution_database.rs`:494 on 2024-05-24 18:59_

That is nicer, updated.

---

_Merged by @charliermarsh on 2024-05-27 01:21_

---

_Closed by @charliermarsh on 2024-05-27 01:21_

---

_Comment by @itayporezky on 2024-05-28 20:18_

After seeing this in the release notes i was afraid it would cause [this](https://stackoverflow.com/q/66290522) 
But in my testing it didn't, so that's great :) 

---
