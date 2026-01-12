```yaml
number: 3018
title: "Globset: curly bracket pattern does not support n items"
type: issue
state: closed
author: ODAncona
labels:
  - invalid
assignees: []
created_at: 2025-03-25T03:05:37Z
updated_at: 2025-03-25T20:13:40Z
url: https://github.com/BurntSushi/ripgrep/issues/3018
synced_at: 2026-01-12T16:13:25Z
```

# Globset: curly bracket pattern does not support n items

---

_@ODAncona_

### Please tick this box to confirm you have reviewed the above.

- [x] I have a different issue.

### What version of ripgrep are you using?

globset = "0.4.15"

### How did you install ripgrep?

Cargo

### What operating system are you using ripgrep on?

Linux, macOs

### Describe your bug.

In the documentation, it's stated

> {a,b} matches a or b where a and b are arbitrary glob patterns. (N.B. Nesting {...} is not currently allowed.)

Nesting is not currently supported, how ever, what about {a,b,c} ?

It leads to unpredictable behavior. For instance, the first (a) misses.

### What are the steps to reproduce the behavior?

I made this unit test in code2prompt repo to test it

```rs
    #[test]
    fn test_brace_expansion_first_item() {
        let env = TestEnv::new();
        let mut cmd = env.command();
        cmd.arg("--include")
            .arg("lowercase/{foo.py,bar.py,baz.py}")
            .arg("--exclude")
            .arg("lowercase/{qux.txt,corge.txt,grault.txt}")
            .assert()
            .success();

        let output = env.read_output();

        assert!(contains("foo.py").eval(&output));
        assert!(contains("content foo.py").eval(&output));
        assert!(contains("bar.py").eval(&output));
        assert!(contains("content bar.py").eval(&output));
        assert!(contains("baz.py").eval(&output));
        assert!(contains("content baz.py").eval(&output));
        assert!(contains("qux.txt").not().eval(&output));
        assert!(contains("corge.txt").not().eval(&output));
        assert!(contains("grault.txt").not().eval(&output));
    }
```

or could use this

```rs
let mut builder = GlobSetBuilder::new();
// This will automatically expand to multiple patterns
builder.add_pattern("lowercase/{foo.py,bar.py,baz.py}")?;
let set = builder.build()?;

// This will match as expected
assert!(set.is_match("lowercase/foo.py"));
assert!(set.is_match("lowercase/bar.py"));
assert!(set.is_match("lowercase/baz.py"));
```

### What is the actual behavior?

Only bar.py,baz.py matches

### What is the expected behavior?

foo.py should match as well as bar.py,baz.py

---

_Comment by @ODAncona on 2025-03-25 03:44_

I kindly asked Claude 3.7 that provided me this answer
Maybe it could be useful ?

```md

# Implementing Brace Expansion for Globset

After analyzing the code, I can see the issue with brace expansion in globset. Currently, when a pattern like `lowercase/{foo.py,bar.py,baz.py}` is used, it's handled as a single regex pattern with alternation `(?:foo\.py|bar\.py|baz\.py)` rather than being expanded to multiple distinct patterns.

## The Solution

The key fix is to implement brace expansion at the `GlobSetBuilder` level. Here's my implementation approach:

```rust
impl GlobSetBuilder {
    /// Add a pattern string to this set.
    /// Automatically expands brace patterns into multiple glob patterns.
    pub fn add_pattern(&mut self, pattern: &str) -> Result<&mut GlobSetBuilder, Error> {
        let expanded = expand_braces(pattern);
        
        for exp in expanded {
            match Glob::new(&exp) {
                Ok(glob) => self.pats.push(glob),
                Err(e) => return Err(e),
            }
        }
        Ok(self)
    }

    // Keep existing add() method for backward compatibility
    pub fn add(&mut self, pat: Glob) -> &mut GlobSetBuilder {
        self.pats.push(pat);
        self
    }
}

/// Expands brace patterns in a glob string into multiple glob strings.
/// For example, "a{b,c}d" expands to ["abd", "acd"].
fn expand_braces(pattern: &str) -> Vec<String> {
    // If no braces, return the pattern as is
    if !pattern.contains('{') || !pattern.contains('}') {
        return vec![pattern.to_string()];
    }
    
    // Find the first matching pair of braces
    let mut depth = 0;
    let mut start_idx = None;
    
    for (i, c) in pattern.chars().enumerate() {
        match c {
            '{' => {
                depth += 1;
                if depth == 1 {
                    start_idx = Some(i);
                }
            }
            '}' => {
                if depth == 1 && start_idx.is_some() {
                    // Found a matching pair
                    let prefix = &pattern[0..start_idx.unwrap()];
                    let suffix = &pattern[i+1..];
                    let options_str = &pattern[start_idx.unwrap()+1..i];
                    
                    // Parse options, handling nested commas properly
                    let mut options = Vec::new();
                    let mut start = 0;
                    let mut nested_depth = 0;
                    
                    for (j, ch) in options_str.chars().enumerate() {
                        match ch {
                            '{' => nested_depth += 1,
                            '}' => nested_depth -= 1,
                            ',' if nested_depth == 0 => {
                                options.push(&options_str[start..j]);
                                start = j + 1;
                            }
                            _ => {}
                        }
                    }
                    
                    // Add the last option
                    options.push(&options_str[start..]);
                    
                    // Recursively expand each option with the prefix and suffix
                    let mut expanded = Vec::new();
                    for opt in options {
                        let new_pattern = format!("{}{}{}", prefix, opt, suffix);
                        // Recursively expand in case there are more braces
                        for exp in expand_braces(&new_pattern) {
                            expanded.push(exp);
                        }
                    }
                    
                    return expanded;
                }
                depth -= 1;
            }
            _ => {}
        }
    }
    
    // If we get here, there was no matching pair of braces
    vec![pattern.to_string()]
}
```

## Usage

With this implementation, users can now do:

```rust
let mut builder = GlobSetBuilder::new();
// This will automatically expand to multiple patterns
builder.add_pattern("lowercase/{foo.py,bar.py,baz.py}")?;
let set = builder.build()?;

// This will match as expected
assert!(set.is_match("lowercase/foo.py"));
assert!(set.is_match("lowercase/bar.py"));
assert!(set.is_match("lowercase/baz.py"));
```

## Key Features of This Implementation

1. **Non-breaking change**: Adds a new method rather than changing existing behavior
2. **Recursive expansion**: Correctly handles nested braces like `{a,b{c,d}}`
3. **Proper nesting**: Handles commas inside nested braces correctly
4. **Simple API**: A single method call handles all the expansion complexity

This approach maintains compatibility with existing code while adding the much-needed brace expansion functionality in a clean, intuitive way.
```

---

_Comment by @BurntSushi on 2025-03-25 03:47_

Huh? I can't tell what the heck you're saying. Ate you reporting a bug? If so, please provide an MRE.

---

_Comment by @ODAncona on 2025-03-25 18:49_

@BurntSushi 

The issue is specifically for `globset` crate.

This is the MRE.

```rs
let mut builder = GlobSetBuilder::new();
builder.add_pattern("folder/{foo,bar,baz}")?;
let set = builder.build()?;

assert!(set.is_match("folder/foo"));
assert!(set.is_match("folder/bar"));
assert!(set.is_match("folder/baz"));
```

It looks like you are using regex to handle the glob `foo/{a,b}` that will match `foo/a` or `foo/b`.

However, this syntax doesn't work: `foo/{a,b,c,...,n}` which should match `foo/a`, `foo/b`, `foo/c`, ... , `foo/n`.

I provided the files `globset/lib.rs` and `globset/glob.rs` to Claude (Anthropic LLM) to ask him what changes should be done to globset codebase in order to support the syntax `foo/{a,b,c,d,...}` and provided the answer. 

I thought you could directly identify if the proposed code was horsesh*t or useful as I'm not familiar with `globset` repo. And I wanted to hear you about that syntax ? I'm not sure to understand what is specified in the doc.

> {a,b} matches a or b where a and b are arbitrary glob patterns. (N.B. Nesting {...} is not currently allowed.)

Thank you


---

_Comment by @BurntSushi on 2025-03-25 20:13_

Again... _wat_?

Your program doesn't even compile. I had to change it to this:

```rust
use globset::{Glob, GlobSetBuilder};

fn main() -> anyhow::Result<()> {
    let mut builder = GlobSetBuilder::new();
    builder.add(Glob::new("folder/{foo,bar,baz}")?);
    let set = builder.build()?;

    assert!(set.is_match("folder/foo"));
    assert!(set.is_match("folder/bar"));
    assert!(set.is_match("folder/baz"));

    Ok(())
}
```

With this `Cargo.toml`:

```toml
[package]
publish = false
name = "globset-i3018"
version = "0.1.0"
edition = "2024"

[dependencies]
anyhow = "1.0.97"
globset = "0.4.16"

[[bin]]
name = "globset-i3018"
path = "main.rs"

[profile.release]
debug = true
```

And when I run it, all assertions pass, as expected:

```
$ cargo r
    Finished `dev` profile [unoptimized + debuginfo] target(s) in 0.00s
     Running `target/debug/globset-i3018`
$
```

So I don't see any problem here.

---

_Closed by @BurntSushi on 2025-03-25 20:13_

---

_Label `invalid` added by @BurntSushi on 2025-03-25 20:13_

---
