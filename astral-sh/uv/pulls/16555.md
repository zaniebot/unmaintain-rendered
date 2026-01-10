```yaml
number: 16555
title: "Allow explicit values with `uv version --bump`"
type: pull_request
state: merged
author: terror
labels: []
assignees: []
merged: true
base: main
head: allow-bump-specific-version
created_at: 2025-11-02T22:41:40Z
updated_at: 2025-11-11T17:27:32Z
url: https://github.com/astral-sh/uv/pull/16555
synced_at: 2026-01-10T06:28:12Z
```

# Allow explicit values with `uv version --bump`

---

_Pull request opened by @terror on 2025-11-02 22:41_

Resolves https://github.com/astral-sh/uv/issues/16427

This PR updates `uv version --bump` so you can pin the exact number you’re targeting, i.e. `--bump patch=10` or `--bump dev=42`. The command-line interface now parses those `component=value` flags, and the bump logic actually sets the version to the number you asked for.


---

_@zanieb reviewed on 2025-11-07 04:09_

---

_Review comment by @zanieb on `docs/guides/package.md`:91 on 2025-11-07 04:09_

As a note, I don't think this belongs in the "guide" long-term — this kind of less common functionality belongs in the "concepts" section but we don't have one for `uv version` yet.

I guess it'd be a "Project version" section in https://docs.astral.sh/uv/concepts/projects/config/ ? We should open an issue to track that work and ref this comment.

---

_Review requested from @Gankra by @zanieb on 2025-11-07 04:10_

---

_Review comment by @Gankra on `crates/uv-cli/src/lib.rs`:718 on 2025-11-08 02:49_

I personally find `.map().unwrap_or()` easier to read

---

_Review comment by @Gankra on `crates/uv-cli/src/lib.rs`:718 on 2025-11-08 02:53_

or just

```rust
let (name, value) = match input.split_once('=') {
    Some((n, v)) => (n, Some(v)),
    None => (input, None),
};
```

---

_Review comment by @Gankra on `crates/uv-cli/src/lib.rs`:719 on 2025-11-08 02:54_

Why did you add this `to_ascii_lowercase`? I don't think we should be doing any lowercasing here, the current implementation doesn't.

---

_Review comment by @Gankra on `crates/uv-cli/src/lib.rs`:737 on 2025-11-08 03:00_

aside: i am perplexed by rustfmt selecting this

---

_Review comment by @Gankra on `crates/uv/src/commands/project/version.rs`:350 on 2025-11-08 03:22_

This doesn't feel like the right approach, it's duplicating a lot of logic that `version.bump` already contains. 

I think we should just make these enums take an `Option<u64>` and handle that in `version.bump`. The only difference between `None` and `Some` is whether you're setting the value-to-update to `cur + 1` or `value`.

---

_@Gankra requested changes on 2025-11-08 03:23_

---

_Comment by @Gankra on 2025-11-08 03:24_

(Btw the actual clap parser work is rad, and thank you for jumping on this!)

---

_Review comment by @terror on `crates/uv-cli/src/lib.rs`:719 on 2025-11-08 03:33_

Ah I assumed wrong that we were case sensitive. Changed this up in my latest commit.

---

_@terror reviewed on 2025-11-08 03:33_

---

_@terror reviewed on 2025-11-08 03:35_

---

_Review comment by @terror on `crates/uv/tests/it/version.rs`:1188 on 2025-11-08 03:35_

aside: There are a few other places we manually dedent like this (and then some places we don't dedent at all). We may want to pull in something like [indoc](https://docs.rs/indoc/latest/indoc/) and use it everywhere to be consistent.

---

_@terror reviewed on 2025-11-08 03:39_

---

_Review comment by @terror on `crates/uv-cli/src/lib.rs`:718 on 2025-11-08 03:39_

Yeah this reads more clear to me, changed this up.

---

_@terror reviewed on 2025-11-08 03:42_

---

_Review comment by @terror on `crates/uv/src/commands/project/version.rs`:350 on 2025-11-08 03:42_

Just implemented this and it feels alot better. I had overlooked some of the existing logic that could have been re-used.

---

_@terror reviewed on 2025-11-08 04:01_

---

_Review comment by @terror on `crates/uv-cli/src/lib.rs`:737 on 2025-11-08 04:01_

Looked into this and it seems to be because of the multiline RHS expression, I kinda like this better:

```rust
let value = match value {
    Some(raw) if raw.is_empty() => {
        return Err("`--bump` values cannot be empty".to_string());
    }
    Some(raw) => Some(
        raw.parse::<u64>()
            .map_err(|_| format!("invalid numeric value `{raw}` for `--bump {name}`"))?,
    ),
    None => None,
};
```

(no rustfmt interference here 😅)

---

_Review requested from @Gankra by @terror on 2025-11-11 00:19_

---

_Review comment by @Gankra on `crates/uv-pep440/src/version.rs`:740 on 2025-11-11 17:26_

This could technically be made into an else-if but it's consistent with the other block to not, so on balance this is probably better..?

---

_@Gankra approved on 2025-11-11 17:27_

Nice, looks good!

---

_Merged by @Gankra on 2025-11-11 17:27_

---

_Closed by @Gankra on 2025-11-11 17:27_

---
