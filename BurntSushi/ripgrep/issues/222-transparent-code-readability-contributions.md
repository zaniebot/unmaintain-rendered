```yaml
number: 222
title: Transparent code readability contributions
type: issue
state: closed
author: jacwah
labels: []
assignees: []
created_at: 2016-11-05T20:56:29Z
updated_at: 2016-11-06T02:42:52Z
url: https://github.com/BurntSushi/ripgrep/issues/222
synced_at: 2026-01-12T18:23:11Z
```

# Transparent code readability contributions

---

_@jacwah_

While browsing the source, I came upon some pieces of code I think can be improved from a readability perspective. I think improving readability is important because it is ultimately one of the deciding factors for a potential new contributor. It can also help prevent bugs as you will see in my example below.

```
if !m_ignore.is_none() {
    m_ignore
} else if !m_gi.is_none() {
    m_gi
} else if !m_gi_exclude.is_none() {
    m_gi_exclude
} else if !m_global.is_none() {
    m_global
} else if !m_explicit.is_none() {
    m_explicit
} else {
    Match::None
}
```
I think this block is not as clear as it could be at first glance. Instead, I think a `Match::or` method could help, similar to `Option::or`.
```
m_ignore.or(m_gi).or(m_gi_exclude).or(m_global).or(m_explicit)
```
This prevents the reader from having to match the condition with the value for each if statement. It also mitigates the potential bug where a condition is not matched with its value.

What do you think? Are you interested in these kinds of contributions to ripgrep?

---

_Comment by @BurntSushi on 2016-11-05 22:51_

> What do you think? Are you interested in these kinds of contributions to ripgrep?

In general, yes, I'm definitely interested. And I do agree with your specific example here: using combinators makes the code more readable in this case. I also think the enclosing function is not that great either.

I think my one caveat is that we may not always agree on which code is clearer, so I may not want to accept all such modifications. :-)


---

_Comment by @jacwah on 2016-11-05 23:22_

> I think my one caveat is that we may not always agree on which code is clearer, so I may not want to accept all such modifications. :-)

Of course, I wouldn't expect you to! I'm glad you're interested, I'll make sure to read through more of the code to see what I can find, and we can discuss individual cases in any potential PRs :)


---

_Comment by @BurntSushi on 2016-11-06 02:42_

I think I'm going to close this since I think we're on the same page, and I don't think we need an issue tracking small code improvements.

I forgot to mention: if you want to work on a larger refactoring of code (major internal changes or public API changes, for example), then that's probably worth opening an issue before investing too much time.

I look forward to working with you! Feel free to ping me on IRC on any of the Rust channels if you have any questions or want to chit chat. My handle is `burntsushi`.


---

_Closed by @BurntSushi on 2016-11-06 02:42_

---
