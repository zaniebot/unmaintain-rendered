```yaml
number: 2570
title: "[`pylint`]: bad-str-strip-call"
type: pull_request
state: merged
author: colin99d
labels:
  - rule
assignees: []
merged: true
base: main
head: bad-str-strip-call
created_at: 2023-02-04T16:43:44Z
updated_at: 2023-02-06T21:34:02Z
url: https://github.com/astral-sh/ruff/pull/2570
synced_at: 2026-01-12T15:55:08Z
```

# [`pylint`]: bad-str-strip-call

---

_@colin99d_

ref #970 

---

_Comment by @colin99d on 2023-02-04 16:57_

Currently the autofixer can leave complicated strings looking slightly different. To a certain degree this has to happen because duplicate new lines, and other characters, must be removed. Additionally, I think multi-line strips are very rare, especially if you are not allowed to have duplicated characters. If the formatting is an issue, we could implement a feature that only replaces if there is no newline character. Let me know what you think.

---

_Comment by @charliermarsh on 2023-02-04 21:58_

Can you give an example of what it might change in the way you describe?

---

_Label `rule` added by @charliermarsh on 2023-02-04 21:58_

---

_Comment by @colin99d on 2023-02-04 22:32_

Simple Example: `"Hello World".strip("Hello")` => `"Hello World".strip("Helo")`

Complex Example:
 ```
"Hello World".strip("""
Hello
"""
)
```

=>

 ```
"Hello World".strip("""
Helo"""
)
````

The second newline character is removed, which is "correct" but means the formatting is weird.

---

_Converted to draft by @colin99d on 2023-02-05 04:07_

---

_Comment by @charliermarsh on 2023-02-05 05:11_

Yeah those are interesting... I wonder if autofix is unintuitive here. I think I would be surprised by either of those cases, even if I found the lint error itself helpful. Wdyt?

---

_Comment by @colin99d on 2023-02-05 13:37_

> Yeah those are interesting... I wonder if autofix is unintuitive here. I think I would be surprised by either of those cases, even if I found the lint error itself helpful. Wdyt?

Personally, I think it is useful. Each snippet of code still does the exact same thing, it has just been reformatted to follow best practices. I could see the formatting on the newlines being annoying; however, we could fix this by not autoformatting if the string contains the newline.

At the end of the day its your decision though, let me know what you want me to do.

---

_Marked ready for review by @colin99d on 2023-02-05 13:55_

---

_Comment by @colin99d on 2023-02-05 15:52_

Also Charlie, I just want to let you know that I am doing pylint fixes in the order they are listed, and skipping ones that I think we cannot do practically right now. But if you see one I skipped that you think we can do let me know, and I will get on it.

---

_Merged by @charliermarsh on 2023-02-06 20:34_

---

_Closed by @charliermarsh on 2023-02-06 20:34_

---

_Comment by @charliermarsh on 2023-02-06 20:35_

> Also Charlie, I just want to let you know that I am doing pylint fixes in the order they are listed, and skipping ones that I think we cannot do practically right now. But if you see one I skipped that you think we can do let me know, and I will get on it.

Definitely you should feel free to implement the rules that you think are most impactful, since the list isn't in any opinionated order :)

---

_Branch deleted on 2023-02-06 20:40_

---

_Comment by @ngnpope on 2023-02-06 21:09_

> I wonder if autofix is unintuitive here.

Just came here to say the same thing...

Generally it would seem that the following make sense:
- If someone used `s.lstrip("Hello")` they probably meant `s.removeprefix("Hello")`
- If someone used `s.rstrip("World")` they probably meant `s.removesuffix("World")`
- If someone used `s.strip("something")` they probably wanted `.removesuffix()` (or maybe `.removeprefix()`)
- `s.lstrip("https://")` is 99.99% likely meant to be `s.removeprefix("https://")`

Maybe the autofix can apply if it looks like there are duplicates in a set of arbitrary characters? And if it looked like a dictionary word, then not? _(Although this requires dictionaries, language support, and is likely to be slow, so...)_

Another thought is that we can somehow provide hints for well-known cases, e.g. "Did you mean `.removeprefix()`?"
Maybe these things can be displayed, e.g. in `ruff rule PLE1310` output?

If we keep this autofixable, maybe it should be classed as "unsafe" and/or disabled by default?

---

_Comment by @charliermarsh on 2023-02-06 21:23_

Alright, let's just remove the autofix for now. And I'll add those suggestions to the error message.

---

_Comment by @charliermarsh on 2023-02-06 21:23_

(I was on the fence, but two votes is enough to err on the side of caution. Can revisit when we have autofix-off-by-default support.)

---

_Renamed from "[`pylint`]: bad-str-strip-call (With Autofix)" to "[`pylint`]: bad-str-strip-call" by @charliermarsh on 2023-02-06 21:24_

---

_Comment by @colin99d on 2023-02-06 21:25_

With Nick's response I totally agree now. Even thought the code is "still correct", its probably not what the user wants

---

_Comment by @ngnpope on 2023-02-06 21:31_

> Even thought the code is "still correct", its probably not what the user wants

Agreed. It's technically correct, but I worry that people will blindly use `--fix` and miss the true problem being flagged.

Naturally, even this doesn't capture all cases, e.g. `.rstrip("word")` wouldn't flag because it has no duplicate characters.

But something is better than nothing!

> And I'll add those suggestions to the error message.

Only refer to `.removeprefix()` and `.removesuffix()` for Python 3.9+ though...

---

_Comment by @charliermarsh on 2023-02-06 21:34_

Yup, the suggestion is gated on 3.9+.

---
