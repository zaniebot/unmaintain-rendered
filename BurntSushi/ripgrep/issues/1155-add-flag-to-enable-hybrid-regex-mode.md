```yaml
number: 1155
title: "add flag to enable \"hybrid\" regex mode"
type: issue
state: closed
author: BurntSushi
labels:
  - enhancement
assignees: []
created_at: 2019-01-08T18:42:03Z
updated_at: 2019-04-14T23:29:30Z
url: https://github.com/BurntSushi/ripgrep/issues/1155
synced_at: 2026-01-12T16:13:23Z
```

# add flag to enable "hybrid" regex mode

---

_@BurntSushi_

In some cases, it would be nice for ripgrep to use Rust's regex engine by default whenever possible, and then fall back to PCRE2 when "advanced" regex features are used. In terms of user level documentation, I might suggest this specification:

```
--auto-hybrid-regex
    When this flag is used, ripgrep will dynamically choose between supported
    regex engines depending on the features used in a pattern. When ripgrep
    chooses a regex engine, it applies that choice for every regex provided to
    ripgrep (e.g., via multiple `-e/--regexp` or `-f/--file` flags).

    As an example of how this flag might behave, ripgrep will attempt to use its
    default finite automata based regex engine whenever the pattern can be
    successfully compiled with that regex engine. If PCRE2 is enabled and if the
    pattern given could not be compiled with the default regex engine, then PCRE2
    will be automatically used for searching. If PCRE2 isn't available, then this flag
    has no effect because there is only one regex engine to choose from.

    In the future, ripgrep may adjust its heuristics for how it decides which regex
    engine to use. In general, the heuristics will be limited to a static analysis of
    the patterns, and not to any specific runtime behavior observed while searching
    files.

    The primary downside of using this flag is that it may not always be obvious
    which regex engine ripgrep uses, and thus, the match semantics or performance
    profile of ripgrep may subtly and unexpectedly change. However, in many cases,
    all regex engines will agree on what constitutes a match and it can be nice to
    transparently support more advanced regex features like look-around and
    backreferences without explicitly needing to enable them.

    This flag can be disabled with `--no-auto-hybrid-regex`.
```

I had initially thought about adding this when I added PCRE2, since it's a somewhat logical addition. However, I held off because I wanted a real use case first. One such use case is here: https://github.com/Microsoft/vscode/issues/64606

In terms of implementation, we should add debug logs indicating which regex engine is being used and any compilation errors that are otherwise suppressed in normal output.

cc @roblourens

---

_Label `enhancement` added by @BurntSushi on 2019-01-24 01:10_

---

_Closed by @BurntSushi on 2019-04-14 23:29_

---
