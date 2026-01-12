```yaml
number: 17311
title: Initial PEP 792 types and index parsing
type: pull_request
state: merged
author: woodruffw
labels: []
assignees: []
merged: true
base: main
head: ww/pep792-types
created_at: 2026-01-02T22:36:40Z
updated_at: 2026-01-07T16:35:40Z
url: https://github.com/astral-sh/uv/pull/17311
synced_at: 2026-01-12T16:12:43Z
```

# Initial PEP 792 types and index parsing

---

_@woodruffw_

## Summary

Adds initial types and detail response handling for PEP 792 project status markers. 

This is towards #15254.

Ref: https://packaging.python.org/en/latest/specifications/project-status-markers/#

## Test Plan

Added new unit tests for both HTML and JSON index extraction.

---

_@woodruffw reviewed on 2026-01-05 15:36_

---

_Review comment by @woodruffw on `crates/uv-client/src/html.rs`:18 on 2026-01-05 15:36_

Flagging: this is currently marked as dead code, since nothing (outside tests) reads this field. My follow-on PRs will actually read this.

---

_Marked ready for review by @woodruffw on 2026-01-05 16:38_

---

_Review requested from @konstin by @woodruffw on 2026-01-05 16:48_

---

_Review comment by @konstin on `crates/uv-pypi-types/src/simple_json.rs`:23 on 2026-01-06 10:33_

In the docs, the schema looks different (https://packaging.python.org/en/latest/specifications/simple-repository-api/#simple-repository-json-project-detail):

```json
  "meta": {
    "api-version": "1.4",
    "project-status": "active",
    "project-status-reason": "this project is not yet haunted"
  },
```

---

_Review comment by @konstin on `crates/uv-client/src/html.rs`:98 on 2026-01-06 10:37_

Do we know that we need to manually tag this as inline, or can the compiler do that for us?

---

_Review comment by @konstin on `crates/uv-client/src/html.rs`:135 on 2026-01-06 10:45_

Future versions may add additional status codes, so we should ignore an unknown status (while logging)

---

_@konstin reviewed on 2026-01-06 10:57_

---

_@woodruffw reviewed on 2026-01-06 14:41_

---

_Review comment by @woodruffw on `crates/uv-client/src/html.rs`:98 on 2026-01-06 14:41_

I'd expect the compiler to do it at O1 and above given how small the function is, but I don't think Rust (or LLVM) will guarantee it. I can remove the attribute if you prefer without 🙂

---

_@woodruffw reviewed on 2026-01-06 14:50_

---

_Review comment by @woodruffw on `crates/uv-pypi-types/src/simple_json.rs`:23 on 2026-01-06 14:50_

Hm, which part? The missing api-version? Technically that predates the spec so I didn't want to complicate things, but the most correct thing would be to check for 1.4+ and only deserialize project statuses when that's satisfied. 

(I think that would also address your review above, since we could reject unknown statuses on a per version basis.)

---

_Review comment by @konstin on `crates/uv-client/src/html.rs`:98 on 2026-01-06 15:32_

I wouldn't add optimizations we don't know are needed, the rust compiler is really good at optimizing, sometimes in unexpected ways, otherwise it only becomes hard to figure out if we need to do something special performance-wise when changing the code later.

---

_@konstin reviewed on 2026-01-06 15:32_

---

_@konstin reviewed on 2026-01-06 15:49_

---

_Review comment by @konstin on `crates/uv-pypi-types/src/simple_json.rs`:23 on 2026-01-06 15:49_

What I meant is that according to the living spec, the json should look different, with the project status nested in the meta. PEP 792 agrees with the implementation here (https://peps.python.org/pep-0792/#json-index) and the warehouse implementation (judging from https://pypi.org/simple/pepy/?format=application/vnd.pypi.simple.v1+json), but the living spec update in https://github.com/pypa/packaging.python.org/pull/1879 seems to say otherwise?

---

_@woodruffw reviewed on 2026-01-06 16:11_

---

_Review comment by @woodruffw on `crates/uv-pypi-types/src/simple_json.rs`:23 on 2026-01-06 16:11_

Oh, that's my own fault -- it looks like I transcribed the PEP into the living specs by memory, and an older draft of the PEP had those keys under `meta` 🤦 

I'll send a PR fixing the living spec.

---

_@konstin approved on 2026-01-06 16:39_

We should also add a test with a full simple JSON response, but I assume we'll do that anyway for the integration tests in the follow-ups.

---

_@woodruffw reviewed on 2026-01-06 17:43_

---

_Review comment by @woodruffw on `crates/uv-pypi-types/src/simple_json.rs`:23 on 2026-01-06 17:43_

https://github.com/pypa/packaging.python.org/pull/1986

---

_@woodruffw reviewed on 2026-01-06 17:45_

---

_Review comment by @woodruffw on `crates/uv-client/src/html.rs`:135 on 2026-01-06 17:45_

Sounds good -- I'll lower this to an INFO log. 

Technically the most correct thing would be to link known statuses to API versions and reject statuses that are unknown on a per-version basis. But that's kind of overkill/hard to do at the moment without doing a lot of serde mucking.

---

_Review comment by @woodruffw on `crates/uv-client/src/html.rs`:135 on 2026-01-06 17:57_

Okay, I pushed this up to `Status::new` and made that the single source of truth with a custom `Deserialize` impl. That should ensure we always log on unknown statuses.

---

_@woodruffw reviewed on 2026-01-06 17:57_

---

_Merged by @woodruffw on 2026-01-07 16:35_

---

_Closed by @woodruffw on 2026-01-07 16:35_

---

_Branch deleted on 2026-01-07 16:35_

---
