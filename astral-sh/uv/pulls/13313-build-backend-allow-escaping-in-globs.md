```yaml
number: 13313
title: "Build backend: Allow escaping in globs"
type: pull_request
state: merged
author: konstin
labels:
  - enhancement
assignees: []
merged: true
base: main
head: konsti/support-escaping-other-characters
created_at: 2025-05-06T11:04:09Z
updated_at: 2025-05-07T16:31:43Z
url: https://github.com/astral-sh/uv/pull/13313
synced_at: 2026-01-12T16:10:39Z
```

# Build backend: Allow escaping in globs

---

_@konstin_

PEP 639 does not allow any characters that aren't in either their limited glob syntax or the alphanumeric Unicode characters. This means there's no way to express a glob such as `**/@test` for the excludes.

We extend the glob syntax from PEP 639 by introducing backslash escapes, which can escape all characters but path separators (forward and backwards slashes) to be parsed verbatim.

This means we have two glob parsers: The strict PEP 639 parser for `project.license-files`, and our extended parser for `tool.uv`, with a slight difference if you need to use special characters, to both adhere to PEP 639 and to support cases such as #13280.

Fixes #13280

---

_Label `enhancement` added by @konstin on 2025-05-06 11:04_

---

_Review requested from @BurntSushi by @konstin on 2025-05-06 11:04_

---

_@konstin reviewed on 2025-05-06 11:06_

---

_Review comment by @konstin on `crates/uv-globfilter/src/portable_glob.rs`:26 on 2025-05-06 11:06_

Not as pretty as our usual hint logic where we show hints below the error chain, but it avoids doing this processing separately for this one error.

---

_@konstin reviewed on 2025-05-06 11:09_

---

_Review comment by @konstin on `crates/uv-globfilter/src/portable_glob.rs`:309 on 2025-05-06 11:09_

It's verbose that you have to support all non-alphanumeric characters even if they aren't "special", but I expect that these are rare in project paths.

---

_@konstin reviewed on 2025-05-06 11:10_

---

_Review comment by @konstin on `docs/configuration/build-backend.md`:22 on 2025-05-06 11:10_

This should be done by rooster, the merge timing is off.

---

_Review comment by @BurntSushi on `crates/uv-globfilter/src/portable_glob.rs`:309 on 2025-05-06 12:27_

I think this is the thing that perplexes me the most here. Are you doing things this way because of PEP 639? But since you're already going beyond PEP 639 by introducing escaping, does it make sense to only _require_ escaping for meta characters?

---

_@BurntSushi approved on 2025-05-06 12:32_

I think this probably LGTM. The requirement to escape characters like `@`, even though they aren't glob meta characters, is somewhat odd. Although I defer to your judgment here based on PEP 639. And I agree with you that it should be somewhat rare to need to do this.

For additional context, the "traditional" way of escaping things in some glob syntaxes (which don't have backslash escaping mechanisms) is to put them in a character class. So e.g., `[@]` or `[*]` for example. IDK if that helps with respect to PEP 639.

In PEP 639, requiring all globs to only consist of alphanumerics, underscores, hypens, dots and the meta-characters and making literally everything else illegal seems rather odd. Do you know why they did that?

---

_Comment by @konstin on 2025-05-06 14:16_

We've decided to go ahead with this solution from two considerations:

Usually, `[…]` is used as escape as backslashes are reserved for Windows paths, but PEP 639 and our portable paths (just like URLs) both only use forward slashes.

Using backslashes, and applying them conservatively for now allows evolving the rules: We can add more meta characters and allows more characters without escape later, without breaking existing globs.

---

_Merged by @konstin on 2025-05-07 16:31_

---

_Closed by @konstin on 2025-05-07 16:31_

---

_Branch deleted on 2025-05-07 16:31_

---
