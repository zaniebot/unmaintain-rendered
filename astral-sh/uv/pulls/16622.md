```yaml
number: 16622
title: Reject ambiguously parsed URLs
type: pull_request
state: merged
author: woodruffw
labels:
  - enhancement
assignees: []
merged: true
base: main
head: ww/url-ambiguity
created_at: 2025-11-06T21:43:14Z
updated_at: 2025-11-17T03:47:27Z
url: https://github.com/astral-sh/uv/pull/16622
synced_at: 2026-01-10T05:58:11Z
```

# Reject ambiguously parsed URLs

---

_Pull request opened by @woodruffw on 2025-11-06 21:43_

## Summary

This tweaks `DisplaySafeUrl` to reject some ambiguous parsing cases that WHATWG and RFC 3986 otherwise consider valid. Specifically, we reject URLs where the path or fragment component _looks_ like it contains a `user:pass`, indicating that the parser didn't disambiguate a userinfo section. 

The most common underlying reason for this is user error: if a user manually configures something like an index URL with a username or password containing a non-escaped `/` or `#`, both RFC 3986 and WHATWG treat that as the beginning of the path/fragment rather than a part of the username/password itself.

## Test Plan

I've added unit and integration tests for this. There's a nonzero chance that this snares real-world URLs out there, but I think that risk is pretty small.


---

_Assigned to @woodruffw by @woodruffw on 2025-11-06 21:43_

---

_@woodruffw reviewed on 2025-11-06 22:17_

---

_Review comment by @woodruffw on `crates/uv/tests/it/sync.rs`:14758 on 2025-11-06 22:17_

Noting: #16623 has some discussion on why we redact the error message but not the TOML spans here.

---

_Marked ready for review by @woodruffw on 2025-11-06 22:21_

---

_Review requested from @konstin by @woodruffw on 2025-11-06 22:29_

---

_Review requested from @zanieb by @woodruffw on 2025-11-06 22:29_

---

_Review comment by @woodruffw on `crates/uv-pep508/src/verbatim_url.rs`:98 on 2025-11-06 22:31_

Noting: we do this over the original `given` input because we want to redact *everything* between the first `:` and the last `@`, not just the parts that happen to be in the path or fragment. We do this because it's hard to be confident that these sensitive values don't end up in other (non-path or fragment) components.

---

_@woodruffw reviewed on 2025-11-06 22:31_

---

_Review comment by @konstin on `crates/uv-pep508/src/verbatim_url.rs`:60 on 2025-11-07 15:37_

Should we share this code with `DisplaySafeUrl`?

---

_Review requested from @charliermarsh by @woodruffw on 2025-11-07 19:16_

---

_@zanieb reviewed on 2025-11-10 20:42_

---

_Review comment by @zanieb on `crates/uv-pep508/src/verbatim_url.rs`:98 on 2025-11-10 20:42_

I'm a little confused that these aren't just chained `if let Some(...)` without the intermediate `path_or_fragment_is_fishy` variable? Then you don't need to unwrap anything?

---

_@woodruffw reviewed on 2025-11-10 20:47_

---

_Review comment by @woodruffw on `crates/uv-pep508/src/verbatim_url.rs`:98 on 2025-11-10 20:47_

The thinking there was that we want to check for those characters in _only_ the `path` and/or `fragment` components, not the entire user-supplied URL (`given`). Checking across all of `given` would mean some (admittedly unlikely) false positives like:

```
https://hack:me@example.com/deeply:evil@path
```

(I could do a chained if-let with those components, but then we'd lose the absolute indexes into `given`.)

---

_Review comment by @konstin on `crates/uv-pep508/src/verbatim_url.rs`:98 on 2025-11-10 20:58_

You can avoid the unwrapping by turning `path_or_fragment_is_fishy` into an `Option<(usize, usize)>` with the indexes, plus a `debug_assert` that `path.contains(':') && path.contains('@') || fragment.contains(':') && fragment.contains('@')` implies it's `Some`.

---

_@konstin reviewed on 2025-11-10 21:00_

---

_@zanieb reviewed on 2025-11-10 21:01_

---

_Review comment by @zanieb on `crates/uv-pep508/src/verbatim_url.rs`:98 on 2025-11-10 21:01_

I see. I think that makes me a little more wary? i.e., we are now relying on the contents `fragment()` and `path()` of a parsed URL matching `given`? It'd be nice to be safer here, we're pretty adverse to panics. 

I took some time to just write an alternative commit 840586b28

---

_@zanieb reviewed on 2025-11-10 21:02_

---

_Review comment by @zanieb on `crates/uv-pep508/src/verbatim_url.rs`:98 on 2025-11-10 21:02_

(sorry I also re-wrapped your comments to match our usual length for the project)

---

_@woodruffw reviewed on 2025-11-10 21:18_

---

_Review comment by @woodruffw on `crates/uv-pep508/src/verbatim_url.rs`:98 on 2025-11-10 21:18_

> I see. I think that makes me a little more wary? i.e., we are now relying on the contents `fragment()` and `path()` of a parsed URL matching `given`? It'd be nice to be safer here, we're pretty adverse to panics.

Yeah, good callout. I can't think of a scenario in which a URL parsed from `given` wouldn't have that correspondence, but the unwraps do indeed feel icky. Thanks for rewriting it!



---

_@zanieb reviewed on 2025-11-10 21:20_

---

_Review comment by @zanieb on `crates/uv-pep508/src/verbatim_url.rs`:98 on 2025-11-10 21:20_

I can't think of one either but I also would want a less murky invariant if we're unwrapping so we're confident it can't regress somehow.

---

_@woodruffw reviewed on 2025-11-10 21:21_

---

_Review comment by @woodruffw on `crates/uv-pep508/src/verbatim_url.rs`:60 on 2025-11-10 21:21_

Sorry, I somehow missed this review earlier. Yeah, sharing this logic between the two makes a lot of sense to me.

(I see `VerbatimUrl` wraps `DisplaySafeUrl`, so perhaps it makes sense to push this check up to that layer?)

---

_@konstin reviewed on 2025-11-11 10:03_

---

_Review comment by @konstin on `crates/uv-pep508/src/verbatim_url.rs`:60 on 2025-11-11 10:03_

I wanted to say that `Url::parse()` is still used a lot, but it's ~all test code, so we can use `DisplaySafeUrl`.

---

_@woodruffw reviewed on 2025-11-11 12:11_

---

_Review comment by @woodruffw on `crates/uv-pep508/src/verbatim_url.rs`:60 on 2025-11-11 12:11_

Yep! I'll do the refactor for this today.

---

_Comment by @woodruffw on 2025-11-11 17:28_

https://github.com/astral-sh/uv/pull/16622/commits/635d1f43cb14fc77cb29645786200cc119973ece pushes the logic up to the `DisplaySafeUrl` layer, but at the cost of a new error type and some invariant preservation risks. I'll flag those in comments on the diff.

---

_@woodruffw reviewed on 2025-11-11 18:25_

---

_Review comment by @woodruffw on `crates/uv-redacted/src/lib.rs`:234 on 2025-11-11 18:25_

Flagging: we end up assuming a preserved invariant here, since we don't call `DisplaySafeUrl::parse` to filter out valid-but-ambiguous URLs. I can think of a few different workarounds to this:

1. Remove the `From<...>` impl entirely. This will cause a decent amount of code churn, since we have `DisplaySafeUrl::from(...)` sprinkled across the codebase. Instead, everything would need to go through `DisplaySafeUrl::parse(url.as_str())` or similar, i.e. go through a re-parse. @zanieb points out (and I agree) that this might be a performance risk, since we probably parse/create a _lot_ of these URL objects over the lifetime of a uv run.
2. Switch to `TryFrom` here, and devolve the ambiguity check in `DisplaySafeUrl::parse` into an associated helper so that we can reuse it. This would still require some code churn, but probably not as much as removing the `From` impl with no replacement.

Beyond those two options, a third option is to do nothing at all and keep this exactly as it is -- the assumption will then be that we're careful to only call `DisplaySafeUrl::from` on `Url` objects that have already had their invariant checked. I *think* this is currently true, since the main place where `DisplaySafeUrl::from` seems to get used is with `request.url()`, where we presumably fed the request a URL that we already validated.

---

_@konstin reviewed on 2025-11-11 18:59_

---

_Review comment by @konstin on `crates/uv-redacted/src/lib.rs`:234 on 2025-11-11 18:59_

I would reduce the number of `From` conversions (https://github.com/astral-sh/uv/pull/16689), this should even be perf-positive.

The remaining ones are URLs we're reading from a request, which look like shouldn't try to do checks on them. Another option is to have an `fn from_url` that's intentionally not `From`/`Into` to annotate that this is not an always-safe conversation, and document the caveat (only call this we `Url` you already validated or that you don't want to validate).

---

_@woodruffw reviewed on 2025-11-12 01:43_

---

_Review comment by @woodruffw on `crates/uv-redacted/src/lib.rs`:234 on 2025-11-12 01:43_

Yeah, an explicit `from_url` makes sense to me -- I'll do that.

---

_Label `enhancement` added by @konstin on 2025-11-12 10:31_

---

_@zanieb approved on 2025-11-12 15:42_

sure!

---

_Merged by @woodruffw on 2025-11-12 16:27_

---

_Closed by @woodruffw on 2025-11-12 16:27_

---

_Branch deleted on 2025-11-12 16:27_

---

_Comment by @matthuisman on 2025-11-17 03:47_

I believe this change has broken local editable installs that contain @ in their directory
https://github.com/astral-sh/uv/issues/16756

---
