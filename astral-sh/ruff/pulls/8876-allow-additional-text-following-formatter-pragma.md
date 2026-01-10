```yaml
number: 8876
title: Allow additional text following formatter pragma comments
type: pull_request
state: closed
author: zanieb
labels: []
assignees: []
draft: true
base: main
head: zb/fmt-skip-reason
created_at: 2023-11-28T19:28:09Z
updated_at: 2023-12-14T18:29:43Z
url: https://github.com/astral-sh/ruff/pull/8876
synced_at: 2026-01-10T23:31:11Z
```

# Allow additional text following formatter pragma comments

---

_Pull request opened by @zanieb on 2023-11-28 19:28_

I'm not sure if we should do this, but I was curious what it would look like to implement.

Closes https://github.com/astral-sh/ruff/issues/8874

---

_Comment by @github-actions[bot] on 2023-11-28 19:42_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Renamed from "Allow additional text to folllow formatter pragma comments" to "Allow additional text to follow formatter pragma comments" by @zanieb on 2023-11-28 19:47_

---

_Renamed from "Allow additional text to follow formatter pragma comments" to "Allow additional text following formatter pragma comments" by @zanieb on 2023-11-28 19:48_

---

_Review requested from @MichaReiser by @zanieb on 2023-11-28 20:14_

---

_@MichaReiser reviewed on 2023-11-28 23:58_

Seems reasonable. Can you double check if we the new implementation is flexible enough to cover Black's preview style https://github.com/astral-sh/ruff/issues/8892 (I don't see why we should gate this behind a preview flag)

---

_@charliermarsh reviewed on 2023-11-29 02:35_

---

_Review comment by @charliermarsh on `crates/ruff_python_formatter/src/comments/mod.rs`:183 on 2023-11-29 02:35_

Could we just replace `match command.trim_whitespace_start()` with `match command.trim_whitespace_start(). split_whitespace().next()`? That is, could we just always split by whitespace and take the first token, rather than nesting the match expressions?

---

_@zanieb reviewed on 2023-11-29 03:30_

---

_Review comment by @zanieb on `crates/ruff_python_formatter/src/comments/mod.rs`:183 on 2023-11-29 03:30_

Yes that's how I implemented it at first but then I was wondering if we should only allow comments on `fmt: off` and `fmt: skip` but not `fmt: on` which led me to this structure. I decided it'd be weird not to support `fmt: on - reason` though and I can see if being useful. I left it this way because I figured there may be a performance benefit and it's still easy to read. I'm happy to go back though.

---

_Comment by @zanieb on 2023-11-29 03:46_

> Can you double check if we the new implementation is flexible enough to cover Black's preview style https://github.com/astral-sh/ruff/issues/8892 

While I agree we should allow pragma comments to follow other comments e.g. `# foo # fmt: skip` or `#foo; fmt: skip`, I don't think this implementation will cover those cases and we'll need to do something a little more involved. Additionally, I don't think the semicolon separated format is supported by our linter pragmas e.g. `# foo; noqa` and we should probably implement support for that in both at once?

Additionally, the syntax proposed here is actually _different_ than Black's preview style. Black does not allow a reason unless it is separated by a `; ` or `# `:

```python
# valid
foo   = 1  # fmt: skip; reason
foo   = 1  # noqa; fmt: skip; reason
foo   = 1  # fmt: skip; noqa
foo   = 1  # fmt: skip # reason
foo   = 1  # fmt: skip # reason # noqa
foo   = 1  # reason # fmt: skip

# invalid
foo   = 1  # fmt: skip - reason; noqa
foo   = 1  # fmt: skip - reason
foo   = 1  # fmt: skip reason
```

I'd be okay with requiring a `; ` separator, I guess. Perhaps I should close this and we can focus on the Black syntax instead?

> I don't see why we should gate this behind a preview flag

Agreed, unless we are unsure it is the right syntax to support.

---

_Comment by @zanieb on 2023-11-29 04:17_

In light of #8892 I think the goal here should be to determine if we want to allow trailing text after pragma directives. I think this question can be considered separately from multiple pragma directives e.g.

```python
foo   =  1  # noqa - some reason; fmt: skip - other reason
```

This seems like a valid use-case?

---

_Comment by @BurntSushi on 2023-12-08 19:52_

I think I like this overall. It might be nice to start by sticking to Black's syntax here. It does look more conservative right? In that we could expand it something more liberal like you have here later?

> I think this question can be considered separately from multiple pragma directives e.g.

My understanding here is that if this PR is merged as-is, then a comment like `# noqa - some reason; fmt: skip - other reason` would today be allowed and be seen as just a `noqa` directive, right? But if you wanted to support that second `fmt: skip` directive, it would potentially change the meaning of what was once just free-form text to a pragma directive. I think a simple restriction on what's allowed in this PR would mitigate that concern? (If it's worth mitigating at all. It could be a non-issue.)

---

_Comment by @charliermarsh on 2023-12-08 19:57_

I think it's correct to allow arbitrary contents after the `# fmt: on` etc. It's also consistent with how we treat `# noqa` in the linter.

---

_Comment by @zanieb on 2023-12-08 20:08_

I think we can close this pull request entirely, as the implementation of #8892 will make whatever happens here irrelevant.

It sounds like there's rough consensus to allow trailing text. We can either do that when implementing #8892 or after, depending on if it's more or less work :)

---

_Closed by @zanieb on 2023-12-14 18:29_

---
