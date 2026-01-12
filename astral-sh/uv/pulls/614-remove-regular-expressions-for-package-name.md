```yaml
number: 614
title: Remove regular expressions for package name normalization
type: pull_request
state: merged
author: charliermarsh
labels:
  - performance
assignees: []
merged: true
base: main
head: charlie/pkg-name
created_at: 2023-12-12T05:41:58Z
updated_at: 2023-12-12T17:18:28Z
url: https://github.com/astral-sh/uv/pull/614
synced_at: 2026-01-12T16:04:04Z
```

# Remove regular expressions for package name normalization

---

_@charliermarsh_

Very random but the hand-written version is about 3-4x faster (benchmarked in a standalone repo).

---

_Label `performance` added by @charliermarsh on 2023-12-12 05:42_

---

_@charliermarsh reviewed on 2023-12-12 05:47_

---

_Review comment by @charliermarsh on `crates/puffin-normalize/src/lib.rs`:23 on 2023-12-12 05:47_

I looked at changing this to return `Cow`, so that we could avoid iterating over the characters twice for the `validate_and_normalize_owned` case, but it ended up being slower. I guess more branches?

---

_Merged by @charliermarsh on 2023-12-12 05:50_

---

_Closed by @charliermarsh on 2023-12-12 05:50_

---

_Branch deleted on 2023-12-12 05:50_

---

_@BurntSushi reviewed on 2023-12-12 14:18_

---

_Review comment by @BurntSushi on `crates/puffin-normalize/src/lib.rs`:26 on 2023-12-12 14:18_

You should be able to iterate over bytes here, since your case analysis is limited to ASCII characters below. That would let you avoid doing UTF-8 decoding. (Which is what `chars()` is doing.)

Same thing for `is_normalized`.

---

_@BurntSushi reviewed on 2023-12-12 14:21_

---

_Review comment by @BurntSushi on `crates/puffin-normalize/src/lib.rs`:23 on 2023-12-12 14:21_

Plausibly. My prior is that a `Cow` would probably be faster here, but I could see it not making much of a difference (or perhaps being slower) if most names didn't need normalization. If most do, then you're doing two passes for most names.

---

_@charliermarsh reviewed on 2023-12-12 14:33_

---

_Review comment by @charliermarsh on `crates/puffin-normalize/src/lib.rs`:26 on 2023-12-12 14:33_

Dumb question, what happens if `name` _does_ contain a UTF-8 character in that case?

---

_@charliermarsh reviewed on 2023-12-12 14:35_

---

_Review comment by @charliermarsh on `crates/puffin-normalize/src/lib.rs`:23 on 2023-12-12 14:35_

Maybe I'll try again. How would you typically structure this? Make `normalized` an `Option`, and initialize it with the preceding contents as soon as you see a mismatching character?

---

_@BurntSushi reviewed on 2023-12-12 14:43_

---

_Review comment by @BurntSushi on `crates/puffin-normalize/src/lib.rs`:26 on 2023-12-12 14:43_

Behavior will remain unchanged. You'll hit the same error case.

The key here is that UTF-8 was specifically designed to be ASCII compatible. That means that any time you see an ASCII byte in valid UTF-8, it's guaranteed to correspond to the equivalent ASCII codepoint. Since here you don't actually care about anything other than ASCII, you can just look at the bytes directly.

---

_@charliermarsh reviewed on 2023-12-12 14:45_

---

_Review comment by @charliermarsh on `crates/puffin-normalize/src/lib.rs`:26 on 2023-12-12 14:45_

But what if the string contains a multi-byte character?

---

_Review comment by @BurntSushi on `crates/puffin-normalize/src/lib.rs`:23 on 2023-12-12 14:45_

I think so yeah, that's where I'd start.

But definitely keep the bigger picture in mind here. Are most names being normalized? If not, it might not help much.

---

_@BurntSushi reviewed on 2023-12-12 14:45_

---

_@BurntSushi reviewed on 2023-12-12 14:47_

---

_Review comment by @BurntSushi on `crates/puffin-normalize/src/lib.rs`:26 on 2023-12-12 14:47_

There is no UTF-8 encoding of any Unicode scalar value where any of its code units is equivalent to an ASCII byte. So you'll just trip your error case. Same as what happens in your code today if I'm reading it right.

This might help: https://play.rust-lang.org/?version=stable&mode=debug&edition=2021&gist=a632ee5d0aa0adb2a154da5475aa3882

---

_@BurntSushi reviewed on 2023-12-12 14:50_

---

_Review comment by @BurntSushi on `crates/puffin-normalize/src/lib.rs`:26 on 2023-12-12 14:50_

The fact that you return an `InvalidNameError` upon seeing _anything_ that doesn't match `[-_.0-9A-Za-z]` is what makes the optimization work. The error case doesn't care about _what_ you see, only that it isn't `[-_.0-9A-Za-z]`. Since `[-_.0-9A-Za-z]` is all ASCII, the byte version of that class and the codepoint version of that class are precisely equivalent. So in the current code, the error case triggers when you see a _codepoint_ outside of `[-_.0-9A-Za-z]`. But in the byte version, the error case will trigger when you see a UTF-8 code unit that isn't in `[-_.0-9A-Za-z]`. The error cases are precisely equivalent because UTF-8 is ASCII compatible.

---

_Comment by @charliermarsh on 2023-12-12 16:52_

For posterity: I rewrote this to use `Cow`.

```rust
pub(crate) fn cow(
    name: &str,
) -> Result<Cow<str>> {
    let mut normalized: Option<String> = None;

    let mut last = None;
    for (i, char) in name.bytes().enumerate() {
        match char {
            b'A'..=b'Z' => {
                if let Some(normalized) = normalized.as_mut() {
                    normalized.push(char.to_ascii_lowercase() as char);
                } else {
                    let mut string = String::with_capacity(name.len());
                    string.push_str(&name[..i]);
                    string.push(char.to_ascii_lowercase() as char);
                    normalized = Some(string);
                }
            }
            b'a'..=b'z' | b'0'..=b'9' => {
                if let Some(normalized) = normalized.as_mut() {
                    normalized.push(char as char);
                }
            }
            b'-' => {
                match last {
                    // Names can't start with punctuation.
                    None => return Err(anyhow::anyhow!(name.to_string())),
                    Some(b'-') => {
                        // Runs of `-` are normalized to a single `-`.
                        if normalized.is_none() {
                            let mut string = String::with_capacity(name.len());
                            string.push_str(&name[..i]);
                            normalized = Some(string);
                        }
                    }
                    Some(_) => {
                        if let Some(normalized) = normalized.as_mut() {
                            normalized.push('-');
                        }
                    },
                }
            }
            b'_' | b'.' => {
                match last {
                    // Names can't start with punctuation.
                    None => return Err(anyhow::anyhow!(name.to_string())),
                    Some(b'-') | Some(b'_') | Some(b'.') => {
                        if normalized.is_none() {
                            let mut string = String::with_capacity(name.len());
                            string.push_str(&name[..i]);
                            normalized = Some(string);
                        }
                    }
                    Some(_) => {
                        if let Some(normalized) = normalized.as_mut() {
                            normalized.push('-');
                        } else {
                            let mut string = String::with_capacity(name.len());
                            string.push_str(&name[..i]);
                            string.push('-');
                            normalized = Some(string);
                        }
                    }
                }
            }
            _ => return Err(anyhow::anyhow!(name.to_string())),
        }
        last = Some(char);
    }

    // Names can't end with punctuation.
    if matches!(last, Some(b'-') | Some(b'_') | Some(b'.')) {
        return Err(anyhow::anyhow!(name.to_string()));
    }

    if let Some(normalized) = normalized {
        Ok(Cow::Owned(normalized))
    } else {
        Ok(Cow::Borrowed(name))
    }
}
```

It's much faster in the event that there is no normalization, but a touch slower in other cases. However, importantly, it's also not much of an optimization in practice because we always convert the `str` to a `String` after calling this, so it doesn't _actually_ save us an allocation.

---

_Comment by @BurntSushi on 2023-12-12 16:58_

It still does save a second pass over the name though? Although perhaps that's marginal here, and especially so if most names are already normalized.

---

_Comment by @charliermarsh on 2023-12-12 17:06_

In my criterion benchmark, interestingly, the version where we need to normalize an owned string ends up being _slower_ than doing two passes... E.g., given `"meine_stadt_transformers".to_string()`, it's 83ns vs. 77ns for the Cow-based version vs. the two-pass version. Why could that be...?

Do you think this variant is easier to understand and maintain? Perhaps I'll commit it anyway.

---

_Comment by @BurntSushi on 2023-12-12 17:12_

I'm not sure at a glance honestly. I think I'd have to investigate. The branches and the beefier code could definitely be having an impact.

> Do you think this variant is easier to understand and maintain? Perhaps I'll commit it anyway.

I like what's on `main` right now better than the `Cow` stuff.

---

_Comment by @charliermarsh on 2023-12-12 17:18_

Sounds good, moving on :) Thank you!

---
