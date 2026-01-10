```yaml
number: 16199
title: Implement RFC9457 compliant messaging
type: pull_request
state: merged
author: doddi
labels:
  - enhancement
assignees: []
merged: true
base: main
head: RFC9457-problem-messages
created_at: 2025-10-09T08:04:09Z
updated_at: 2025-10-16T19:54:36Z
url: https://github.com/astral-sh/uv/pull/16199
synced_at: 2026-01-10T06:36:15Z
```

# Implement RFC9457 compliant messaging

---

_Pull request opened by @doddi on 2025-10-09 08:04_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

HTTP1.1 [RFC 9112 - HTTP/1.1](https://www.rfc-editor.org/rfc/rfc9112.html#name-status-line) section 4 defines the response status code to optionally include a text description (human readable) of the reason for the status code.

[RFC9113 - HTTP/2](https://www.rfc-editor.org/rfc/rfc9113) is the HTTP2 protocol standard and the response status only considers the [status code](https://www.rfc-editor.org/rfc/rfc9113#name-response-pseudo-header-fiel) and not the reason phrase, and as such important information can be lost in helping the client determine a route cause of a failure.

As per discussion on this [PR](https://github.com/astral-sh/uv/pull/15979) the current feeling is that implementing the RFC9457 standard might be the preferred route. This PR makes those changes to aid the discussion which has also been moved to the [PEP board](https://discuss.python.org/t/block-download-of-components-when-violating-policy/104021/1)

## Test Plan

Pulling components that violate our policy over HTTP2 and without any RFC9457 implementation the following message is presented to the user:
<img width="1482" height="104" alt="image" src="https://github.com/user-attachments/assets/0afcd0d8-ca67-4f94-a6c2-131e3b6d8dcc" />


With the RFC9457 standard implemented, below you can see the advantage in the extra context as to why the component has been blocked:
<img width="2171" height="127" alt="image" src="https://github.com/user-attachments/assets/25bb5465-955d-4a76-9f30-5477fc2c866f" />



---

_Comment by @zanieb on 2025-10-09 13:42_

Cool!

cc @woodruffw 

---

_Review requested from @konstin by @zanieb on 2025-10-09 13:43_

---

_Review requested from @woodruffw by @woodruffw on 2025-10-09 13:44_

---

_Review comment by @woodruffw on `crates/uv-client/src/cached_client.rs`:39 on 2025-10-10 17:34_

Thinking out loud: is ignoring a malformed details response the right thing here? I *think* it is, but maybe it makes sense to emit a warning? I could see it being useful to notify upstream servers that they're emitting a malformed payload.



---

_Review comment by @woodruffw on `crates/uv-client/src/cached_client.rs`:575 on 2025-10-10 17:36_

Should this be an equality check rather than `starts_with`? RFC 9457 says that problem detail responses are identified by exactly the `application/problem+json` content-type, so we probably shouldn't accept `application/problem+json+abc` or similar.

---

_Review comment by @woodruffw on `crates/uv-client/src/cached_client.rs`:34 on 2025-10-10 17:39_

IMO it's okay to remove this check, since we do it at the callsite. We can document that a caller-upheld invariant of this API is that the content-type should be right.

(Alternatively we could keep this check, but remove it from the callsite. But that might be a little harder with the lifetimes/consumption.)

---

_Review comment by @woodruffw on `crates/uv-client/src/error.rs`:52 on 2025-10-10 17:40_

My 0.02c would be for removing this, and just calling `serde_json::from_slice` directly wherever we use it 🙂 


---

_Review comment by @woodruffw on `crates/uv-client/src/error.rs`:39 on 2025-10-10 17:41_

Nitpick, nonblocking: serde defaults sometimes benefit from inlining:

```suggestion
/// Default problem type URI as per RFC 9457
#[inline]
```

---

_Review comment by @woodruffw on `crates/uv-client/src/error.rs`:40 on 2025-10-10 17:44_

serde will ignore unknown fields by default, so my 0.02c would be for removing this until we actually need to process extension fields. It'll also make the parsing slightly faster (not that that matters much on the error path):

```suggestion
```

(The tests should confirm that this works 🙂)

---

_Review comment by @woodruffw on `crates/uv-client/src/error.rs`:59 on 2025-10-10 17:51_

Should we render both of these, if present? I can imagine `{title}: {detail}` being useful.


---

_Review comment by @woodruffw on `crates/uv-client/src/error.rs`:61 on 2025-10-10 17:52_

I think we want to specialize here too -- if `status` isn't given we should probably take it from the parent request, *or* maybe we could say "unknown error (server provided no information)" and rely on the error layer above us rendering the status code.

---

_@woodruffw reviewed on 2025-10-10 18:05_

Thanks @doddi! I did a review pass mostly based on my read of the RFC; I believe @konstin will do one more for uv's idioms (in which case any of his feedback on idioms overrides mine) 🙂 

---

_@zanieb reviewed on 2025-10-10 18:22_

---

_Review comment by @zanieb on `crates/uv-client/src/cached_client.rs`:39 on 2025-10-10 18:22_

I probably wouldn't add a "warning" since it's not something the user can fix, but it might be worth logging.

---

_Label `enhancement` added by @konstin on 2025-10-13 09:25_

---

_@doddi reviewed on 2025-10-14 10:04_

---

_Review comment by @doddi on `crates/uv-client/src/cached_client.rs`:39 on 2025-10-14 10:04_

https://github.com/astral-sh/uv/pull/16199/commits/5a3a59da5b357285b510938363f1ace5bfb11239

---

_@doddi reviewed on 2025-10-14 10:04_

---

_Review comment by @doddi on `crates/uv-client/src/cached_client.rs`:575 on 2025-10-14 10:04_

https://github.com/astral-sh/uv/pull/16199/commits/63175fb00c677b99409100f06aa285ae4a7fbcd3

---

_@doddi reviewed on 2025-10-14 10:04_

---

_Review comment by @doddi on `crates/uv-client/src/cached_client.rs`:34 on 2025-10-14 10:04_

https://github.com/astral-sh/uv/pull/16199/commits/bab16f2fc84ea89102761016ef4189b11778f0e4

---

_@doddi reviewed on 2025-10-14 10:05_

---

_Review comment by @doddi on `crates/uv-client/src/error.rs`:52 on 2025-10-14 10:05_

https://github.com/astral-sh/uv/pull/16199/commits/c1c453c796dcc0a7af837626db43bbe99c010412

---

_@doddi reviewed on 2025-10-14 10:05_

---

_Review comment by @doddi on `crates/uv-client/src/error.rs`:39 on 2025-10-14 10:05_

https://github.com/astral-sh/uv/pull/16199/commits/0ab6ed1a5678d355b1b90575cc55449996a4b521

---

_@doddi reviewed on 2025-10-14 10:05_

---

_Review comment by @doddi on `crates/uv-client/src/error.rs`:40 on 2025-10-14 10:05_

https://github.com/astral-sh/uv/pull/16199/commits/9f252c49bf2bed9aa8a5489a5e0c3deeacc52a4b

---

_@doddi reviewed on 2025-10-14 10:05_

---

_Review comment by @doddi on `crates/uv-client/src/error.rs`:59 on 2025-10-14 10:05_

https://github.com/astral-sh/uv/pull/16199/commits/550a3c46899e457dfedb82749395822adc5fed0f

---

_@doddi reviewed on 2025-10-14 10:06_

---

_Review comment by @doddi on `crates/uv-client/src/error.rs`:61 on 2025-10-14 10:06_

https://github.com/astral-sh/uv/pull/16199/commits/550a3c46899e457dfedb82749395822adc5fed0f

---

_Review comment by @konstin on `crates/uv-client/src/error.rs`:71 on 2025-10-14 11:04_

We should return `None` for this case and fall back to the default case in `WrappedReqwestError` instead of using our own string.

---

_Review comment by @konstin on `crates/uv-client/src/cached_client.rs`:32 on 2025-10-14 11:08_

We should show the inner error in this warning (at least for serde), so it's clear why e.g. the data didn't match the schema.

---

_Review comment by @konstin on `crates/uv-client/src/error.rs`:544 on 2025-10-14 11:17_

Here we need to add something like `Server says: ` so it's clear that that's from the remote and not uv

---

_@konstin reviewed on 2025-10-14 11:20_

Can you add a test that shows the error message in context? `index_has_no_requires_python` and the `network.rs` have reference structures.

---

_@doddi reviewed on 2025-10-14 14:44_

---

_Review comment by @doddi on `crates/uv-client/src/error.rs`:71 on 2025-10-14 14:44_

https://github.com/astral-sh/uv/pull/16199/commits/5f01168b52ed05bf223e98213bcd251deed03d63

---

_@doddi reviewed on 2025-10-14 14:45_

---

_Review comment by @doddi on `crates/uv-client/src/cached_client.rs`:32 on 2025-10-14 14:45_

https://github.com/astral-sh/uv/pull/16199/commits/5f01168b52ed05bf223e98213bcd251deed03d63

---

_@doddi reviewed on 2025-10-14 14:45_

---

_Review comment by @doddi on `crates/uv-client/src/error.rs`:544 on 2025-10-14 14:45_

https://github.com/astral-sh/uv/pull/16199/commits/5f01168b52ed05bf223e98213bcd251deed03d63

---

_Comment by @konstin on 2025-10-14 15:56_

For the clippy failure, we can box the large variant.

---

_Comment by @doddi on 2025-10-16 09:33_

> Add an it test

https://github.com/astral-sh/uv/pull/16199/commits/936ff82e1dd37e23aa3af4cbcf0554cd93bfdda7


> For the clippy failure, we can box the large variant.

https://github.com/astral-sh/uv/pull/16199/commits/7ed10748268e12f429f4a7c4129bff7673637cc5 although I see the CI still failing for clippy error on windows for a different area. Can someone help me understand this?

---

_Comment by @konstin on 2025-10-16 09:48_

Some OS types are larger on Windows than on Unix, which can push the error type size over the clippy threshold just for Windows. You can run Windows clippy locally with cargo-xwin and `cargo xwin clippy`, but usually just `Box`ing the struct that grew in the change in an error enum and rerunning CI does the trick.

---

_Comment by @konstin on 2025-10-16 19:09_

I fixed the linter errors, I have experience with them.

---

_@konstin approved on 2025-10-16 19:25_

Thank you!

---

_Comment by @konstin on 2025-10-16 19:42_

I've changed the boxing to a different location because `is_transient_network_error` relies on having specific error types in the error chains and it considers Boxed type different from the base type there.

---

_Comment by @doddi on 2025-10-16 19:43_

> I've changed the boxing to a different location because `is_transient_network_error` relies on having specific error types in the error chains and it considers Boxed type different from the base type there.

Thank you @konstin, I was struggling to understand this error being new to the code base

---

_Merged by @konstin on 2025-10-16 19:53_

---

_Closed by @konstin on 2025-10-16 19:53_

---

_Comment by @woodruffw on 2025-10-16 19:54_

Thanks @doddi!

---
