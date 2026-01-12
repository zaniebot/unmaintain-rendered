```yaml
number: 2960
title: "`grep-printer` not exactly following matcher regex"
type: issue
state: closed
author: OleMussmann
labels: []
assignees: []
created_at: 2025-01-03T09:15:06Z
updated_at: 2025-01-03T13:59:40Z
url: https://github.com/BurntSushi/ripgrep/issues/2960
synced_at: 2026-01-12T16:13:25Z
```

# `grep-printer` not exactly following matcher regex

---

_@OleMussmann_

### Please tick this box to confirm you have reviewed the above.

- [X] I have a different issue.

### What version of ripgrep are you using?

crate: grep = "0.3.2"

### How did you install ripgrep?

Cargo

### What operating system are you using ripgrep on?

NixOS 24.11.20241230.3ffbbdb (Vicuna)

### Describe your bug.

`grep::printer` does not color all the matches when searching for a regex matching the start of a line, only the first one. Regular matches or matches at the end of a line work fine.

### What are the steps to reproduce the behavior?

Example code:
```rust
use grep::{printer, regex, searcher};
use termcolor;

fn main() -> Result<(), Box<dyn std::error::Error>> {
    let content = "abcde abcde\nabcde abcde\nabcde abcde";
    let search_terms = [
        "abcde",  // regular match
        "^abcde", // start of the line
        "abcde$", // end of the line
    ];
    let color_specs = printer::ColorSpecs::new(&["match:fg:red".parse()?]);

    for search_term in search_terms {
        let matcher = regex::RegexMatcherBuilder::new().build(search_term)?;
        let mut printer = printer::StandardBuilder::new()
            .color_specs(color_specs.clone())
            .build(termcolor::StandardStream::stdout(
                termcolor::ColorChoice::Always,
            ));

        println!();
        println!("Searching for \"{search_term}\":");
        searcher::Searcher::new().search_slice(
            &matcher,
            &content.as_bytes(),
            printer.sink(&matcher),
        )?;
    }

    Ok(())
}
```

### What is the actual behavior?

Output image from example code:

![image](https://github.com/user-attachments/assets/e0ff8b50-2c37-42c7-8967-e4507e034c69)

### What is the expected behavior?

My expectation would be that for the search term "^abcde" matches would be colored for _all_ printed lines.

Is it a bug in `grep-printer` or am I using the libraries incorrectly?

---

_Locked by @ghost on 2025-01-03 13:59_

---
