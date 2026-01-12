```yaml
number: 3454
title: Implement an iterator for universal newlines
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/universal-newlines
created_at: 2023-03-12T04:29:10Z
updated_at: 2023-03-13T04:01:31Z
url: https://github.com/astral-sh/ruff/pull/3454
synced_at: 2026-01-12T04:39:44Z
```

# Implement an iterator for universal newlines

---

_Pull request opened by @charliermarsh on 2023-03-12 04:29_

# Summary

We need to support CR line endings (as opposed to LF and CRLF line endings, which are already supported). They're rare, but they do appear in Python code, and we tend to panic on any file that uses them.

Our `Locator` abstraction now supports CR line endings. However, Rust's `str#lines` implementation does _not_.

This PR adds a `UniversalNewlineIterator` implementation that respects all of CR, LF, and CRLF line endings, and plugs it into most of the `.lines()` call sites.

As an alternative design, it could be nice if we could leverage `Locator` for this. We've already computed all of the line endings, so we could probably iterate much more efficiently?

# Test Plan

Largely relying on automated testing, however, also ran over some known failure cases, like #3404.


---

_Marked ready for review by @charliermarsh on 2023-03-12 04:40_

---

_Comment by @github-actions[bot] on 2023-03-12 04:41_

✅ ecosystem check detected no changes.

<!-- thollander/actions-comment-pull-request "ecosystem-results" -->

---

_Review requested from @MichaReiser by @charliermarsh on 2023-03-12 04:45_

---

_@charliermarsh reviewed on 2023-03-12 04:46_

---

_Review comment by @charliermarsh on `crates/ruff_python_ast/src/whitespace.rs`:87 on 2023-03-12 04:46_

I have a feeling that I'm breaking some idioms _and_ that this code could be far more efficient, so feel free to give it a harsh review if it deserves one :)

---

_Review comment by @MichaReiser on `crates/ruff_python_ast/src/whitespace.rs`:99 on 2023-03-12 10:02_

You must ensure that the iterator ends as soon as `forward` and `back` positions cross. 

---

_Review comment by @MichaReiser on `crates/ruff_python_ast/src/whitespace.rs`:167 on 2023-03-12 10:03_

You can implement `FusedIterator` as it is guaranteed that calling `.next()` after it returned `None` once will always return `None`. 

---

_Review comment by @MichaReiser on `crates/ruff_python_ast/src/whitespace.rs`:152 on 2023-03-12 10:47_

What about `\r\n`?

---

_Review comment by @MichaReiser on `crates/ruff_python_ast/src/whitespace.rs`:219 on 2023-03-12 11:31_

We should add tests that mix `next` and `next_back` calls

---

_Review comment by @MichaReiser on `crates/ruff_python_ast/src/whitespace.rs`:87 on 2023-03-12 12:18_

I tried it myself and haven't been able to come up with something much more concise. It's surprisingly complicated

This implementation mutates the underlying string slices rather than tracking start and end positions explicitly (similar to `Vec::IntoIter` that tracks the start and end pointer). I would need to do some profiling to understand if it is faster than using explicit `start` and `end` positions. It might not, because writing a string slice is two words (pointer, length). 

```rust
/// Like `str#lines`, but accommodates LF, CRLF, and CR line endings,
/// the latter of which are not supported by `str#lines`.
pub struct UniversalNewlineIterator<'a> {
    text: &'a str,
}

impl<'a> UniversalNewlineIterator<'a> {
    pub fn from(text: &'a str) -> UniversalNewlineIterator<'a> {
        UniversalNewlineIterator { text }
    }
}

impl<'a> Iterator for UniversalNewlineIterator<'a> {
    type Item = &'a str;

    #[inline]
    fn next(&mut self) -> Option<&'a str> {
        if self.text.is_empty() {
            return None;
        }

        let line = match self.text.find(['\n', '\r']) {
            // Not the last line
            Some(line_end) => {
                let (line, remainder) = self.text.split_at(line_end);

                self.text = match remainder.as_bytes()[0] {
                    // Explicit branch for `\n` as this is the most likely path
                    b'\n' => &remainder[1..],
                    // '\r\n'
                    b'\r' if remainder.as_bytes().get(1) == Some(&b'\n') => &remainder[2..],
                    // '\r'
                    _ => &remainder[1..],
                };

                line
            }
            // Last line
            None => std::mem::replace(&mut self.text, ""),
        };

        Some(line)
    }
}

impl<'a> DoubleEndedIterator for UniversalNewlineIterator<'_> {
    #[inline]
    fn next_back(&mut self) -> Option<Self::Item> {
        if self.text.is_empty() {
            return None;
        }

        let len = self.text.len();

        // Trim any trailing newlines.
        self.text = match self.text.as_bytes()[len - 1] {
            b'\n' if len > 1 && self.text.as_bytes()[len - 2] == b'\r' => &self.text[..len - 2],
            b'\n' | b'\r' => &self.text[..len - 1],
            _ => &self.text,
        };

        // Find the end of the previous line. The previous line is the text up to, but not including the new line character
        let line = match self.text.rfind(['\n', '\r']) {
            // '\n' or '\r' or '\r\n'
            Some(line_end) => {
                let (remainder, line) = self.text.split_at(line_end + 1);
                self.text = remainder;

                line
            }
            // Last line
            None => std::mem::replace(&mut self.text, ""),
        };

        Some(line)
    }
}

impl FusedIterator for UniversalNewlineIterator<'_> {}

#[cfg(test)]
mod tests {
    use super::UniversalNewlineIterator;

    #[test]
    fn universal_newlines_empty_str() {
        let lines: Vec<_> = UniversalNewlineIterator::from("").collect();
        assert_eq!(lines, Vec::<&str>::default());

        let lines: Vec<_> = UniversalNewlineIterator::from("").rev().collect();
        assert_eq!(lines, Vec::<&str>::default());
    }

    #[test]
    fn universal_newlines_forward() {
        let lines: Vec<_> = UniversalNewlineIterator::from("foo\nbar\n\r\nbaz\rbop").collect();
        assert_eq!(lines, vec!["foo", "bar", "", "baz", "bop"]);

        let lines: Vec<_> = UniversalNewlineIterator::from("foo\nbar\n\r\nbaz\rbop\n").collect();
        assert_eq!(lines, vec!["foo", "bar", "", "baz", "bop"]);

        let lines: Vec<_> = UniversalNewlineIterator::from("foo\nbar\n\r\nbaz\rbop\n\n").collect();
        assert_eq!(lines, vec!["foo", "bar", "", "baz", "bop", ""]);
    }

    #[test]
    fn universal_newlines_backwards() {
        let lines: Vec<_> = UniversalNewlineIterator::from("foo\nbar\n\r\nbaz\rbop")
            .rev()
            .collect();
        assert_eq!(lines, vec!["bop", "baz", "", "bar", "foo"]);

        let lines: Vec<_> = UniversalNewlineIterator::from("foo\nbar\n\nbaz\rbop\n")
            .rev()
            .collect();

        assert_eq!(lines, vec!["bop", "baz", "", "bar", "foo"]);
    }

    #[test]
    fn universal_newlines_mixed() {
        let mut lines = UniversalNewlineIterator::from("foo\nbar\n\r\nbaz\rbop");

        assert_eq!(lines.next_back(), Some("bop"));
        assert_eq!(lines.next(), Some("foo"));
        assert_eq!(lines.next_back(), Some("baz"));
        assert_eq!(lines.next(), Some("bar"));
        assert_eq!(lines.next_back(), Some(""));
        assert_eq!(lines.next(), None);
    }
}
```

I also added a few more tests.

This is a more "traditional" implementation that uses start and end offsets (similar to how `SplitIterator` works)

```rust
/// Like `str#lines`, but accommodates LF, CRLF, and CR line endings,
/// the latter of which are not supported by `str#lines`.
pub struct UniversalNewlineIterator<'a> {
    text: &'a str,
    start_offset: usize,
    end_offset: usize,
}

impl<'a> UniversalNewlineIterator<'a> {
    pub fn from(text: &'a str) -> UniversalNewlineIterator<'a> {
        UniversalNewlineIterator {
            text,
            start_offset: 0,
            end_offset: text.len(),
        }
    }

    fn as_str(&self) -> &'a str {
        // SAFETY: It's guaranteed that start and end offsets are always positioned at character boundaries.
        unsafe { &self.text.get_unchecked(self.start_offset..self.end_offset) }
    }
}

impl<'a> Iterator for UniversalNewlineIterator<'a> {
    type Item = &'a str;

    #[inline]
    fn next(&mut self) -> Option<&'a str> {
        let text = self.as_str();

        if text.is_empty() {
            return None;
        }

        let line = match text.find(['\n', '\r']) {
            // Not the last line
            Some(line_end) => {
                self.start_offset += line_end
                    + match text.as_bytes()[line_end] {
                        // Explicit branch for `\n` as this is the most likely path
                        b'\n' => 1,
                        // '\r\n'
                        b'\r' if text.as_bytes().get(line_end + 1) == Some(&b'\n') => 2,
                        // '\r'
                        _ => 1,
                    };

                &text[..line_end]
            }
            // Last line
            None => {
                self.start_offset = self.end_offset;
                text
            }
        };

        Some(line)
    }
}

impl<'a> DoubleEndedIterator for UniversalNewlineIterator<'_> {
    #[inline]
    fn next_back(&mut self) -> Option<Self::Item> {
        let mut text = self.as_str();

        if text.is_empty() {
            return None;
        }

        let len = text.len();

        // Trim any trailing newlines.
        text = match text.as_bytes()[len - 1] {
            b'\n' if len > 1 && text.as_bytes()[len - 2] == b'\r' => &text[..len - 2],
            b'\n' | b'\r' => &text[..len - 1],
            _ => &text,
        };

        // Find the end of the previous line. The previous line is the text up to, but not including the new line character
        let line = match text.rfind(['\n', '\r']) {
            // '\n' or '\r' or '\r\n'
            Some(newline_pos) => {
                let line_start = newline_pos + 1;
                let line = &text[line_start..];
                self.end_offset = self.start_offset + line_start;

                line
            }
            // Last line
            None => {
                self.end_offset = self.start_offset;
                text
            }
        };

        Some(line)
    }
}

impl FusedIterator for UniversalNewlineIterator<'_> {}
```

---

_Review comment by @MichaReiser on `crates/ruff_python_ast/src/whitespace.rs`:87 on 2023-03-12 12:20_

Can we add a doctest on how to use this new struct?

---

_Review comment by @MichaReiser on `crates/ruff_python_ast/Cargo.toml`:28 on 2023-03-12 12:20_

Why did we have to add the `serde` dependency? 

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/pydocstyle/rules/sections.rs`:926 on 2023-03-12 12:25_

Nit: this can be implemented without the need for `collect`


```rust
    let mut lines = NewlineWithTrailingNewline::from(&adjusted_following_lines);

    if let Some(mut current_line) = lines.next() {
        for next_line in lines {
            let current_leading_space = whitespace::leading_space(current_line);
            if current_leading_space == section_level_indent
                && (whitespace::leading_space(next_line).len() > current_leading_space.len())
                && !next_line.trim().is_empty()
            {
                let parameters = if let Some(semi_index) = current_line.find(':') {
                    // If the parameter has a type annotation, exclude it.
                    &current_line[..semi_index]
                } else {
                    // Otherwise, it's just a list of parameters on the current line.
                    current_line.trim()
                };
                // Notably, NumPy lets you put multiple parameters of the same type on the same
                // line.
                for parameter in parameters.split(',') {
                    docstring_args.insert(parameter.trim());
                }
            }

            current_line = next_line;
        }
    }
```

We could go crazy by defaulting to an iterator that only handles `\r\n` and `\n` and fallback to a slower iterator if we find a single `\r` ;)

---

_@MichaReiser approved on 2023-03-12 12:30_

There's an issue with the iterator implementation that it allows `next_back` and `next` to pass each other, which isn't allowed according to the Iterator protocol. Other than that, looks good to me (I proposed an alternative and suggested some more tests but it leads to about the same amount of code)

It would be good to add a few tests we knew were failing before to verify that the new logic handles `\r` correctly. 

I'm undecided if we should create an extension trait to implement `universal_lines` on `&str`. It could help discoverability (over `UniversalNewlinesIterator::from(str)`. 

---

_@MichaReiser reviewed on 2023-03-12 12:37_

---

_Review comment by @MichaReiser on `crates/ruff_python_ast/src/whitespace.rs`:132 on 2023-03-12 12:37_

Let's override `last` so that it calls into `next_back`

---

_@MichaReiser reviewed on 2023-03-12 12:41_

---

_Review comment by @MichaReiser on `crates/ruff_python_ast/src/whitespace.rs`:68 on 2023-03-12 12:41_

Nit: This can be written as

```rust
self.underlying.next().or_else(|| self.trailing.take())
```

---

_@MichaReiser reviewed on 2023-03-12 12:42_

---

_Review comment by @MichaReiser on `crates/ruff_python_ast/src/whitespace.rs`:41 on 2023-03-12 12:42_

```suggestion
/// Like [`UniversalNewlineIterator`], but includes a trailing newline as an empty line.
```

---

_@MichaReiser reviewed on 2023-03-12 12:42_

---

_Review comment by @MichaReiser on `crates/ruff_python_ast/src/whitespace.rs`:76 on 2023-03-12 12:42_

```suggestion
/// Like [`str::lines`], but accommodates LF, CRLF, and CR line endings,
```

---

_@charliermarsh reviewed on 2023-03-12 18:35_

---

_Review comment by @charliermarsh on `crates/ruff_python_ast/Cargo.toml`:28 on 2023-03-12 18:35_

Sorry, I should've documented this. I think it's an issue with how I added the `serde` feature in RustPython. It looks like it doesn't have the `derive` feature enabled. So we need to use "our" `serde`, which _does_ have `derive`, in order for it to compile. (This is based on cursory guess, I didn't look deeply, but I'll create an issue and document.)

---

_@charliermarsh reviewed on 2023-03-13 03:16_

---

_Review comment by @charliermarsh on `crates/ruff_python_ast/src/whitespace.rs`:87 on 2023-03-13 03:16_

I took the first suggestion, since the latter requires `unsafe`, and it'd be our only unsafe block right now.

---

_@charliermarsh reviewed on 2023-03-13 03:16_

---

_Review comment by @charliermarsh on `crates/ruff_python_ast/src/whitespace.rs`:87 on 2023-03-13 03:16_

(I did some cursory benchmarking, and what you had here minorly outperformed the current version in the PR -- but I'd need to run an actual microbenchmark.)

---

_Review comment by @charliermarsh on `crates/ruff_python_ast/src/newlines.rs`:6 on 2023-03-13 03:46_

I went ahead and added an extension trait, but if you have any feedback @MichaReiser happy to address in a follow-up.

---

_@charliermarsh reviewed on 2023-03-13 03:48_

---

_Merged by @charliermarsh on 2023-03-13 04:01_

---

_Closed by @charliermarsh on 2023-03-13 04:01_

---

_Branch deleted on 2023-03-13 04:01_

---
