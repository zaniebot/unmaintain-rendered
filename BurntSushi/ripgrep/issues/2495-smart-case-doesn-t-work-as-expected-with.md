```yaml
number: 2495
title: "Smart case doesn't work as expected with character classes"
type: issue
state: closed
author: ariel-miculas
labels:
  - wontfix
assignees: []
created_at: 2023-04-20T16:42:15Z
updated_at: 2023-04-20T18:19:49Z
url: https://github.com/BurntSushi/ripgrep/issues/2495
synced_at: 2026-01-12T16:13:24Z
```

# Smart case doesn't work as expected with character classes

---

_@ariel-miculas_

#### What version of ripgrep are you using?

❯ rg --version
ripgrep 13.0.0
-SIMD -AVX (compiled)
+SIMD +AVX (runtime)

#### How did you install ripgrep?

cargo install ripgrep

#### What operating system are you using ripgrep on?

Replace this text with your operating system and version.
Arch Linux 6.1.2-arch1-1 x86 GNU/Linux

#### Describe your bug.

Smart case doesn't work as expected when I introduce character classes in my regex.

#### What are the steps to reproduce the behavior?
```
❯ echo MY_RUST > sample
❯ alias rg
rg='rg --smart-case'
❯ rg '[^a-zA-Z]rust' sample
```

#### What is the actual behavior?

Actual behavior is that the pattern doesn't match.

#### What is the expected behavior?

I'm expecting the pattern to match, because `rust` is all lowercase, so the search should be case insensitive. I do have `A-Z` in my character class, but conceptually I'm negating the entire character class, so I'm not matching any lowercase or uppercase letters in my (character class) pattern. Ultimately I'm expecting only literal string characters to influence the smart-case, i.e. character classes and other special characters shouldn't be taken into account for the smart-case feature.


---

_Comment by @BurntSushi on 2023-04-20 17:32_

The documentation, behavior and intent all match up here. Here are the docs:

```
    -S, --smart-case                             
            Searches case insensitively if the pattern is all lowercase. Search case
            sensitively otherwise.
            
            A pattern is considered all lowercase if both of the following rules hold:
            
            First, the pattern contains at least one literal character. For example, 'a\w'
            contains a literal ('a') but just '\w' does not.
            
            Second, of the literals in the pattern, none of them are considered to be
            uppercase according to Unicode. For example, 'foo\pL' has no uppercase
            literals but 'Foo\pL' does.
            
            This overrides the -s/--case-sensitive and -i/--ignore-case flags.
```

The `A-Z` in the character class are literals, and those are indeed what disables smart case. I grant that you've stumbled over a somewhat odd case, but what you're asking for is probably not what you intend. You've written the character class `[^a-zA-Z]`, and from what I can tell, because the _negation_ of that class doesn't include `A-Z`, then it follows that it doesn't contain any uppercase characters and thus smart case should be enabled. Do I have you right? If so, there is an incorrect jump there. `[^a-zA-Z]` is not "match all ASCII characters except for `a-z` and `A-Z`." It is, "match all _Unicode scalar values_ except for `a-z` and `A-Z`." That set of Unicode scalar values certainly contains uppercase characters, and thus, smart case wouldn't be enabled in your case even if it followed your rules. You'd have to disable Unicode mode (`--no-unicode`) for your rules to have the intended effect.

I personally think the current behavior is the most intuitive: smart case is enabled when there aren't any _literal_ uppercase characters in your pattern. That's it. Your pattern has uppercase literals in it, and therefore, the search should be case sensitive because you _took case into account when writing your pattern_. That's kind of the point of smart case.

---

_Closed by @BurntSushi on 2023-04-20 17:32_

---

_Label `wontfix` added by @BurntSushi on 2023-04-20 17:32_

---

_Comment by @ariel-miculas on 2023-04-20 17:54_

Probably a better explanation is "my pattern contains uppercase characters, but it excludes them, so I don't actually want smart-case to be case sensitive", but I guess it's best to use the case insensitive option instead of smart-case.

---

_Comment by @BurntSushi on 2023-04-20 17:58_

How would your rule apply to, say, `[^A-F]`?

---

_Comment by @ariel-miculas on 2023-04-20 18:19_

I'm not intentionally matching any uppercase characters, so I'm expecting a case insensitive match. I guess what I expected was `[^a-zA-Z]rust` to behave kind of like `\Wrust` (but I wanted to exclude `_` from the definition of a word). Since `\W` is not treated as an uppercase character, the pattern `\Wrust` performs an insensitive match. But my hand-made `[^a-zA-Z]` disables smart-case's case insensitive match. But the "solution" for me would be "treat character classes like escape patterns", i.e. don't look inside character classes for uppercase characters when deciding the `smart-case` behavior. But I understand this would break the expectations of users doing `[AB]rust` (or maybe not, since I only want case sensitive search for `[AB]Rust` and I want case insensitive search for `[AB]rust`). Anyway, now I agree with your statement along the lines that "smart case is weird".

---
