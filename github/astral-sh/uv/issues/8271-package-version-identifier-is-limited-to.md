---
number: 8271
title: package version identifier is limited to 18446744073709551615
type: issue
state: closed
author: dschneiderch
labels:
  - needs-decision
assignees: []
created_at: 2024-10-16T21:14:15Z
updated_at: 2024-10-28T18:54:29Z
url: https://github.com/astral-sh/uv/issues/8271
synced_at: 2026-01-07T13:12:17-06:00
---

# package version identifier is limited to 18446744073709551615

---

_Issue opened by @dschneiderch on 2024-10-16 21:14_

It seems that UV doesn't support version identifiers great than 18446744073709551615.   I don't believe https://peps.python.org/pep-0440/ has this restriction and assume it is a function of using Int64 to parse the build number.

To reproduce simply add a dependency with a dev version identifier > 18446744073709551615.

Internally we are using a concatenation of the hash of a branch name and the git commit to version development packages. We don't use the local identifier syntax because google won't let us install those in the cloud-composer (even though we use a private artifactory) :(


```% uv run dags/test_dag.py       
error: Failed to build: `vi-composer @ file:///Users/dschneider/bombora/repos/vi-composer`
  Caused by: Failed to parse metadata from built wheel
  Caused by: expected number less than or equal to 18446744073709551615, but number found in "768230559194918072045" exceeds it
bombora-composer3-airflow2 ==0.2.1.dev768230559194918072045
```


 ```% uv --version
uv 0.4.13 (b8f9ee3b4 2024-09-19) 
```
on macos

---

_Assigned to @BurntSushi by @zanieb on 2024-10-16 21:25_

---

_Label `needs-decision` added by @charliermarsh on 2024-10-16 21:35_

---

_Comment by @zanieb on 2024-10-17 01:36_

It's hard to say if we should support larger integers. I assume it will come with more complexity and performance degradation. Curious to see what @BurntSushi thinks though, as he's the expert on this.

---

_Comment by @chrisrodrigue on 2024-10-17 21:34_

https://crates.io/crates/num-bigint

Max size is `~3.079 x 10^22212093154093428519`

---

_Comment by @zanieb on 2024-10-17 21:57_

We have a highly optimized data structure for version identifiers — they're in the critical path for performance.

---

_Comment by @konstin on 2024-10-18 07:09_

We're using an `u64`, an unsigned 64-bit integer, for the dev segment, and I think that's a reasonable upper limit. With higher numbers, we start using potentially non-cpu-native numbers and increase the size of the performance critical `VersionFull` even further.

---

_Closed by @dschneiderch on 2024-10-21 17:08_

---

_Comment by @BurntSushi on 2024-10-28 13:22_

I think an experiment here could be warranted. My understanding here is that `VersionFull` is the "slow" path, where `VersionSmall` is the fast path. We wouldn't change `VersionSmall`, but rather, change `VersionFull`. The problem, I think, is that even though `VersionFull` is specifically constructed in a way that it is "rare," it could still be frequent enough where if we used a bignum, I wouldn't be shocked if that caused perf problems in some workloads. But even that may not be a show-stopper, since we could add a third `VersionFullLarge` variant that handles the case where the parsed integer is bigger than what is natively supported. It would add some moderate complexity overall, but nothing sticks out to me as being infeasible.

With that said, at a higher level, it's not clear that it's worth supporting integers bigger than `2^64 - 1`. @dschneiderch Do you have a work-around for this, or does this block adoption of uv for you?

---

_Comment by @dschneiderch on 2024-10-28 18:54_

Thanks for the response. Yeah, we're good. We just decided to use unixtime
in seconds since epoch . It's unlikely that'll conflict across users and
it's good as int64 for millenia ;)
I was originally just caught off guard by the restriction .

On Mon, Oct 28, 2024, 06:22 Andrew Gallant ***@***.***> wrote:

> I think an experiment here could be warranted. My understanding here is
> that VersionFull is the "slow" path, where VersionSmall is the fast path.
> We wouldn't change VersionSmall, but rather, change VersionFull. The
> problem, I think, is that even though VersionFull is specifically
> constructed in a way that it is "rare," it could still be frequent enough
> where if we used a bignum, I wouldn't be shocked if that caused perf
> problems in some workloads. But even that may not be a show-stopper, since
> we could add a third VersionFullLarge variant that handles the case where
> the parsed integer is bigger than what is natively supported. It would add
> some moderate complexity overall, but nothing sticks out to me as being
> infeasible.
>
> With that said, at a higher level, it's not clear that it's worth
> supporting integers bigger than 2^64 - 1. @dschneiderch
> <https://github.com/dschneiderch> Do you have a work-around for this, or
> does this block adoption of uv for you?
>
> —
> Reply to this email directly, view it on GitHub
> <https://github.com/astral-sh/uv/issues/8271#issuecomment-2441577903>, or
> unsubscribe
> <https://github.com/notifications/unsubscribe-auth/ABY5SZLGBP32XZPD53D3HHTZ5Y3CBAVCNFSM6AAAAABQCLBO5SVHI2DSMVQWIX3LMV43OSLTON2WKQ3PNVWWK3TUHMZDINBRGU3TOOJQGM>
> .
> You are receiving this because you were mentioned.Message ID:
> ***@***.***>
>


---

_Referenced in [astral-sh/uv#9572](../../astral-sh/uv/issues/9572.md) on 2024-12-02 09:26_

---
