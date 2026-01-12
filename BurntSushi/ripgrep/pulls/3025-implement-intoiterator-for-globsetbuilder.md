```yaml
number: 3025
title: "Implement `IntoIterator` for `GlobSetBuilder`"
type: pull_request
state: closed
author: squidfunk
labels: []
assignees: []
base: master
head: feature/set-builder-iterator
created_at: 2025-04-09T18:37:11Z
updated_at: 2025-04-09T19:26:00Z
url: https://github.com/BurntSushi/ripgrep/pull/3025
synced_at: 2026-01-12T18:23:14Z
```

# Implement `IntoIterator` for `GlobSetBuilder`

---

_@squidfunk_

Originally reported here: https://github.com/BurntSushi/globset/issues/8

This PR fixes the linked issue. See below:

---

The `Debug` implementation of `GlobSetBuilder` is quite noisy. For better introspectability in consuming packages, it would be perfect if one of two things could be implemented, so the `Debug` implementation can be adjusted (favorite) or is more readable:

1. An `IntoIterator<Item = &Glob>` implementation for `&GlobSetBuilder`, so we can use the `Display` implementation of `Glob` where applicable. This would be a non-invasive addition, as the consumer can decide what style they want.
2. A `Debug` implementation for `GlobSetBuilder` that does 1., i.e., prints `Glob` via `Display`.

I'm happy to send a PR for either of which, if this is a good match.

### Example

``` rust 
#[cfg(test)]
mod tests {
    use globset::{Glob, GlobSetBuilder};

    #[test]
    fn test() -> Result<(), Box<dyn std::error::Error>> {
        let mut builder = GlobSetBuilder::new();
        builder.add(Glob::new("src/**/*.rs")?);
        builder.add(Glob::new("*.txt")?);
        builder.add(Glob::new("docs/**/*.md")?);
        println!("{:#?}", builder);
        Ok(())
    }
}
```

### Current Result

``` rust
GlobSetBuilder {
    pats: [
        Glob {
            glob: "src/**/*.rs",
            re: "(?-u)^src(?:/|/.*/).*\\.rs$",
            opts: GlobOptions {
                case_insensitive: false,
                literal_separator: false,
                backslash_escape: true,
            },
            tokens: Tokens(
                [
                    Literal(
                        's',
                    ),
                    Literal(
                        'r',
                    ),
                    Literal(
                        'c',
                    ),
                    RecursiveZeroOrMore,
                    ZeroOrMore,
                    Literal(
                        '.',
                    ),
                    Literal(
                        'r',
                    ),
                    Literal(
                        's',
                    ),
                ],
            ),
        },
        Glob {
            glob: "*.txt",
            re: "(?-u)^.*\\.txt$",
            opts: GlobOptions {
                case_insensitive: false,
                literal_separator: false,
                backslash_escape: true,
            },
            tokens: Tokens(
                [
                    ZeroOrMore,
                    Literal(
                        '.',
                    ),
                    Literal(
                        't',
                    ),
                    Literal(
                        'x',
                    ),
                    Literal(
                        't',
                    ),
                ],
            ),
        },
        Glob {
            glob: "docs/**/*.md",
            re: "(?-u)^docs(?:/|/.*/).*\\.md$",
            opts: GlobOptions {
                case_insensitive: false,
                literal_separator: false,
                backslash_escape: true,
            },
            tokens: Tokens(
                [
                    Literal(
                        'd',
                    ),
                    Literal(
                        'o',
                    ),
                    Literal(
                        'c',
                    ),
                    Literal(
                        's',
                    ),
                    RecursiveZeroOrMore,
                    ZeroOrMore,
                    Literal(
                        '.',
                    ),
                    Literal(
                        'm',
                    ),
                    Literal(
                        'd',
                    ),
                ],
            ),
        },
    ],
}
```

### Ideal Result:

``` rust
GlobSetBuilder {
    pats: [
        "src/**/*.rs",
        "*.txt",
        "docs/**/*.md",
    ]
}
```

Oh and thanks for this library, it is awesome!

---

_Comment by @BurntSushi on 2025-04-09 18:55_

Oh hmmm, I think I might have misread your original issue. I was thinking that we'd just make the `Debug` impl smaller without exposing new APIs.

---

_Comment by @squidfunk on 2025-04-09 19:05_

No problem, so something like this maybe?

``` rust
impl std::fmt::Debug for GlobSetBuilder {
    fn fmt(&self, f: &mut std::fmt::Formatter<'_>) -> std::fmt::Result {
        let pats = self.pats.iter().map(ToString::to_string).collect::<Vec<_>>();
        f.debug_struct("GlobSetBuilder").field("pats", &pats).finish()
    }
}
```

Output:

``` rust
GlobSetBuilder {
    pats: [
        "foo",
        "bar",
    ],
}
```

Or would you want to change the debug impl of `Glob`? I though that maybe that's not what you wanted, given that inspecting globs might be another use case than inspecting a glob set.

---

_Comment by @BurntSushi on 2025-04-09 19:08_

That looks fine to me. But you'll want to branch on `f.alternate()`.

---

_Comment by @squidfunk on 2025-04-09 19:19_

Okay, I hope I understand you correctly:

``` rust
impl std::fmt::Debug for GlobSetBuilder {
    fn fmt(&self, f: &mut std::fmt::Formatter<'_>) -> std::fmt::Result {
        if f.alternate() {
            f.debug_struct("GlobSetBuilder").field("pats", &self.pats).finish()
        // Print the patterns in a way that is easy to read.
        } else {
            let pats = self.pats.iter().map(ToString::to_string).collect::<Vec<_>>();
            f.debug_struct("GlobSetBuilder").field("pats", &pats).finish()
        }
    }
}
```

If that's fine, I can send another PR.

---

_Comment by @squidfunk on 2025-04-09 19:25_

Superseded by #3026.

---

_Closed by @squidfunk on 2025-04-09 19:26_

---
