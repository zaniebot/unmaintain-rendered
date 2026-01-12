```yaml
number: 1878
title: Make < exclusive for non-prerelease markers
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/pre-pydantic
created_at: 2024-02-22T17:00:49Z
updated_at: 2024-02-25T08:50:18Z
url: https://github.com/astral-sh/uv/pull/1878
synced_at: 2026-01-12T16:04:46Z
```

# Make < exclusive for non-prerelease markers

---

_@charliermarsh_

## Summary

Even when pre-releases are "allowed", per PEP 440, `pydantic<2.0.0` should _not_ include pre-releases. This PR modifies the specifier translation to treat `pydantic<2.0.0` as `pydantic<2.0.0.min0`, where `min` is an internal-only version segment that's invisible to users.

Closes https://github.com/astral-sh/uv/issues/1641.


---

_Comment by @charliermarsh on 2024-02-22 17:02_

I think (1) and (2) could be "solved" by only applying this logic when pre-releases are already enabled for a given package.

---

_Comment by @mitsuhiko on 2024-02-22 17:22_

What if there is a `PreRelease { kind: PreReleaseKind::PreReleaseHack, number: 0}`` (find a better name) that behaves as a perfect match to `a0` but is special cased in rendering. It would show up as `2.0.0` rather than `2.0.0.a0` maybe with an additional indicator about how it considers prerelease versions.

```
          attrs>20.3.0,<21.2.0 (including pre) cannot be used.
          And because you require attrs>20.3.0,<21.2.0 (including pre), we can conclude that the
```

---

_Review requested from @zanieb by @charliermarsh on 2024-02-22 17:32_

---

_Marked ready for review by @charliermarsh on 2024-02-22 17:32_

---

_Comment by @charliermarsh on 2024-02-22 17:33_

@mitsuhiko - That's also interesting, it could work, though I still wonder if including the pre-release info is useful in cases in which pre-releases are entirely omitted.

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/prerelease_mode.rs`:104 on 2024-02-22 17:34_

There's a potential bug here. If a package has _only_ pre-releases, we _will_ allow it in the candidate selector (e.g., how `black` behaved long ago). However... we won't know about it here (since we don't know this information in advance). I wonder if we should drop support for this.

---

_@charliermarsh reviewed on 2024-02-22 17:34_

---

_Review requested from @mitsuhiko by @charliermarsh on 2024-02-22 17:34_

---

_Label `bug` added by @charliermarsh on 2024-02-22 17:34_

---

_Comment by @charliermarsh on 2024-02-22 17:35_

\cc @czaki who brought up the idea on GitHub.

---

_Comment by @Czaki on 2024-02-22 18:03_

Hm. Why is pre-release even triggered by upper constraint? All time when I would like to test pre-release of some package or see such case, the lower bound is used. But my experience may be not so big.  

I wil ltry to check code later. 

---

_@notatallshaw reviewed on 2024-02-22 18:04_

---

_Review comment by @notatallshaw on `crates/uv-resolver/src/prerelease_mode.rs`:104 on 2024-02-22 18:04_

FYI, if you need a real world use case, `opentelemetry-exporter-prometheus` only has pre-releases, and `apache-airflow[otel]` depends on this (and hence also `apache-airflow[all]`).

pip very much does have this behavior of including packages that only have prereleases into a regular install.

---

_Comment by @dimbleby on 2024-02-22 18:04_

`.dev0` comes before `.a0` anyway

> Within a numeric release (1.0, 2.7.3), the following suffixes are permitted and MUST be ordered as shown:
>
> .devN, aN, bN, rcN, \<no suffix\>, .postN



---

_@charliermarsh reviewed on 2024-02-22 18:12_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/prerelease_mode.rs`:104 on 2024-02-22 18:12_

Thanks!

> pip very much does have this behavior of including packages that only have prereleases into a regular install.

We do too.

---

_Comment by @Czaki on 2024-02-22 18:16_

> `.dev0` comes before `.a0` anyway
> 
> > Within a numeric release (1.0, 2.7.3), the following suffixes are permitted and MUST be ordered as shown:
> >
> > .devN, aN, bN, rcN, \<no suffix\>, .postN
> 
> 

And cannot by uploaded to pypi.

---

_Comment by @dimbleby on 2024-02-22 18:23_

> And cannot by uploaded to pypi.

alas the world is complicated: pypi is not the only source of distributions

either way, if the intention is to make `<2` exclude pre-releases, might just as well exclude `.dev` releases too.

another way might be to represent this as something like `<=1.infinity`, which _might_ work better as being something that you could easily recognise when writing it back: no-one could actually write that down, whereas if what you have is `<2.0.0.dev0` you cannot be certain that this was not the original constraint all along

---

_Comment by @charliermarsh on 2024-02-22 18:26_

This makes sense, I'll likely change to dev.

---

_Comment by @mitsuhiko on 2024-02-22 19:37_

I would just honestly really represent it as something that is not parsable. The `PreReleaseHack` can basically act as the `.infinity` proposal. It can only be expressed by the resolver internally and is otherwise never valid input.

---

_Comment by @konstin on 2024-02-22 20:48_

Have you checked that this also applies to `2.0.0.a0.dev0`?

---

_Comment by @charliermarsh on 2024-02-22 23:04_

I honestly don't mind representing this (even to users) as `<2.0.0.dev`. If they see this, then they opted-in to pre-release handling (it doesn't get included otherwise).

---

_@mitsuhiko approved on 2024-02-23 10:55_

---

_@konstin approved on 2024-02-23 11:11_

---

_@zanieb approved on 2024-02-23 15:12_

---

_@zanieb reviewed on 2024-02-23 15:40_

---

_Review comment by @zanieb on `crates/uv/tests/pip_compile.rs`:4341 on 2024-02-23 15:40_

Is this test case correct? The comment seems to say something different.

---

_@charliermarsh reviewed on 2024-02-23 15:53_

---

_Review comment by @charliermarsh on `crates/uv/tests/pip_compile.rs`:4341 on 2024-02-23 15:53_

Sorry, the comment is wrong.

---

_@charliermarsh reviewed on 2024-02-23 16:00_

---

_Review comment by @charliermarsh on `crates/uv/tests/pip_compile.rs`:4341 on 2024-02-23 16:00_

(`flask<2.0.0rc4` _should_ allow `2.0.0rc2`.)

---

_Review requested from @BurntSushi by @charliermarsh on 2024-02-24 19:06_

---

_Comment by @charliermarsh on 2024-02-24 19:07_

Okay, now using @mitsuhiko's suggested approach which I will begrudgingly admit is a better solution :)

Now handles all cases correctly, even:

```txt
apache-airflow[otel]
opentelemetry-exporter-prometheus<0.44
```

From the brilliant @notatallshaw (thank you for that).


---

_Review requested from @zanieb by @charliermarsh on 2024-02-24 19:10_

---

_@BurntSushi requested changes on 2024-02-24 22:25_

This LGTM, but you'll definitely need to bump the simple cache version in order to invalidate extant caches due to the change in representation of `Version`. (Apologies if you did that and I missed it.)

---

_Review requested from @BurntSushi by @charliermarsh on 2024-02-24 22:37_

---

_@BurntSushi approved on 2024-02-24 22:41_

Sweet

---

_Merged by @charliermarsh on 2024-02-24 23:02_

---

_Closed by @charliermarsh on 2024-02-24 23:02_

---

_Branch deleted on 2024-02-24 23:02_

---
