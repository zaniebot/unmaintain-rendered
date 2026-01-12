```yaml
number: 339
title: Add check for W292
type: pull_request
state: merged
author: cnpryer
labels: []
assignees: []
merged: true
base: main
head: W292
created_at: 2022-10-07T05:26:49Z
updated_at: 2022-10-08T01:21:34Z
url: https://github.com/astral-sh/ruff/pull/339
synced_at: 2026-01-12T15:55:04Z
```

# Add check for W292

---

_@cnpryer_

This adds `W292 No newline at end of file`.

It's real late where I am, so I'm opening as a draft to come back to this tomorrow with a fresh mind. First time I'm interacting with the codebase.

A few notes:
- ~~I wasn't sure where to start adding these *additional* style checks. So I just kind of referred to them as "More style".~~
- ~~I also added this as a default. Not sure if that's what you're expecting.~~
- ~~I didn't think it made sense to add as an AST check.~~
- ~~This will add a bunch of noise to `cargo run resources/test/fixtures --no-cache` since a bunch of other fixtures may not contain a new line at the end.~~
- ~~I may have incomplete `noqa` handling (part of why I need to go to sleep lol).~~
- ~~I *think* there's a combo of anyhow and .expect for error handling here. I tried using .expect for the .last, but it was failing when running the fixtures in resources/. I just let it not eval if it fails to collect "last" from `lines`.~~
- ~~Clippy was yelling at me for unrelated .or_insert usage.~~

Notes have been resolved. Ty!

---

_Comment by @cnpryer on 2022-10-07 05:40_

For posterity.. tag #170

---

_Comment by @charliermarsh on 2022-10-07 12:25_

Awesome, thanks for putting this together, will review today!

---

_Comment by @cnpryer on 2022-10-07 18:45_

No rush! I want to reduce the amount of time you spend reviewing this, so I can mark it ready for review after I feel like it's worth your time to look at.

I intentionally chose to start with an easy one to get familiar with the codebase lol

---

_Review comment by @charliermarsh on `src/check_lines.rs`:205 on 2022-10-07 18:50_

Mind moving this into `if !line.is_empty() { ... }` below? No need to instantiate it unless we're going to push it.

---

_Review comment by @charliermarsh on `src/check_lines.rs`:197 on 2022-10-07 18:50_

Alternatively, you could do:

```rust
if let Some(line) = lines.last() {
  ...
}
```

(Since we're never gonna trigger the rule if we have no lines.)


---

_Review comment by @charliermarsh on `src/check_lines.rs`:194 on 2022-10-07 18:53_

You can remove `!enforce_noqa`, which is used for something else. You _do_ need to check if the last line has a `noqa` for W292. I'm also happy to add that in a second pass since the logic can be a little tricky. But the gist of it is:

```rust
let lineno = lines.len() - 1;
let 
let noqa_lineno = noqa_line_for
    .get(lineno)
    .map(|lineno| lineno - 1)
    .unwrap_or(lineno);
let check = Check::new(
    CheckKind::NoNewLineAtEndOfFile,
    Range {
        location: Location::new(lines.len(), line.len() + 1),
        end_location: Location::new(lines.len(), line.len() + 1),
    },
);
match noqa {
    (Directive::All(_, _), matches) => {
        matches.push(check.kind.code().as_str());
    }
    (Directive::Codes(_, _, codes), matches) => {
        if codes.contains(&check.kind.code().as_str()) {
            matches.push(check.kind.code().as_str());
        } else {
            line_checks.push(check);
        }
    }
    (Directive::None, _) => line_checks.push(check),
}
```

(Or something like that...)

---

_Comment by @charliermarsh on 2022-10-07 18:53_

> I wasn't sure where to start adding these additional style checks. So I just kind of referred to them as "More style".

Let's put them just after the `E***` rules. They're still style rules, it's just that `pycodestyle` considers them to be warnings rather than errors.

> I also added this as a default. Not sure if that's what you're expecting.

That sounds fine to me.

> I didn't think it made sense to add as an AST check.

Correct, this should be enforced in `check_lines` like you've done above.

> This will add a bunch of noise to cargo run resources/test/fixtures --no-cache since a bunch of other fixtures may not contain a new line at the end.

That's ok. There's already a lot of noise when running `cargo run resources/test/fixtures`. Each of those fixtures is meant to be run with `--select`, like `cargo run resources/test/fixtures/F401.py --select F401`.

> I may have incomplete noqa handling (part of why I need to go to sleep lol).

Yeah it looks like the `noqa` for this rule should be on the last line -- so the file would be, like:

```py
x = 1  # noqa
``` 

> Clippy was yelling at me for unrelated .or_insert usage.

Hmm, maybe we have different versions. What you have seems good, so I'll upstream that real quick.


---

_@charliermarsh reviewed on 2022-10-07 18:53_

---

_Comment by @charliermarsh on 2022-10-07 18:54_

Oh heh, too late, I just reviewed it! :) Let me know if you have any questions.

---

_Review comment by @cnpryer on `src/check_lines.rs`:197 on 2022-10-07 22:45_

It's funny. This is what I had originally. Will do.

---

_@cnpryer reviewed on 2022-10-07 22:45_

---

_@cnpryer reviewed on 2022-10-07 22:47_

---

_Review comment by @cnpryer on `src/check_lines.rs`:194 on 2022-10-07 22:47_

Yea i noticed this from a check above. I think I was thinking of how I could do something similar without slapping in redundant code, and I didn't feel confident introducing a refactor. So I must of misunderstood `enforce_noqa`. I'll see if I can wrap my head around this.

---

_Comment by @cnpryer on 2022-10-07 22:48_

> Oh heh, too late, I just reviewed it! :) Let me know if you have any questions.

Well I'm happy you did. Very helpful.

---

_@cnpryer reviewed on 2022-10-07 23:16_

---

_Review comment by @cnpryer on `src/check_lines.rs`:194 on 2022-10-07 23:16_

So that was pretty much it. Only a couple things seemed to be needed in addition:

1. Adding
```rs
let noqa = noqa_directives
    .entry(noqa_lineno)
    .or_insert_with(|| (noqa::extract_noqa_directive(lines[noqa_lineno]), vec![]));
```
which if I understand is noqa indicators potentially used and/or related to the check that we need to attempt to match on? It does feels a little black-magic-like at the moment, but I think at a high level I understand what's going on here. I think if I understand the Directives better it'll clear up.

2. `noqa_directives` was moved in the "Enforce that the noqa directive was actually used" piece. So I needed to move the check above it.

---

_Marked ready for review by @cnpryer on 2022-10-07 23:17_

---

_Review comment by @charliermarsh on `src/check_lines.rs`:194 on 2022-10-08 01:08_

Yeah the noqa stuff is a little confusing. There are two main pieces to it:

1. It's not always the case that a `noqa` for a given line of code comes at the end of that line. For example, with multi-line strings, we want the `noqa` to come after the string, but to apply to the _entire_ string. So we have to do some bookkeeping, to map "line in the code" to "line at which the noqa for that line should appear". (This is the `noqa_line_for` vector, which is really a map.)
2. We have to extract the `noqa` directive from each line, and we do that lazily, hence the `noqa_directives` map.


---

_@charliermarsh reviewed on 2022-10-08 01:08_

---

_Comment by @charliermarsh on 2022-10-08 01:08_

Thank you for this! Great PR, much appreciated.

---

_Comment by @charliermarsh on 2022-10-08 01:09_

I tried to add autofix but I think the `fixer` has an issue with empty newlines or something (some kind of edge case). It'll have to come later.

---

_Merged by @charliermarsh on 2022-10-08 01:10_

---

_Closed by @charliermarsh on 2022-10-08 01:10_

---

_Branch deleted on 2022-10-08 01:21_

---
