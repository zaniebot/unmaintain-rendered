```yaml
number: 2260
title: Unexpected character group issue with grep crate
type: issue
state: closed
author: BeerAndMala
labels:
  - bug
assignees: []
created_at: 2022-07-15T09:46:00Z
updated_at: 2022-07-15T14:03:39Z
url: https://github.com/BurntSushi/ripgrep/issues/2260
synced_at: 2026-01-12T16:13:24Z
```

# Unexpected character group issue with grep crate

---

_@BeerAndMala_

#### What version of ripgrep are you using?

11.0.2
(but irrelevant, this is using grep = "0.2" as dependencies/as a library)

#### How did you install ripgrep?

Unrelated (apt). Cargo for grep

#### What operating system are you using ripgrep on?

Ubuntu Linux

#### Describe your bug.

Trying to match a regex to a file fails for some expressions that should work

#### What are the steps to reproduce the behavior?

Minimal test case: File containing

```
GATGTCCACGAGGTCTCTCCAGCACTCTAGGCGTTTTGTACCGTTCGTATAGCATACATTATACGAACGGTAGGTATAATTACACTTTATATCGAGCTCGAATTCATCGATCTGTCTCTTATACACATCTGACGCTGCCGACGAGCGATC
```

Rust code:

```
use std::{env, fs::File};

use grep::{regex::RegexMatcherBuilder, searcher::{SearcherBuilder, sinks::UTF8}};

fn main() {
    let args: Vec<String> = env::args().collect();
    
    let matcher = RegexMatcherBuilder::new()
        .line_terminator(Some(b'\n'))
        .build(&args[1]).unwrap();
    let mut searcher = SearcherBuilder::new()
        .line_number(true)
        .build();

    let file = File::open(&args[2]).unwrap();
    searcher.search_file(&matcher, &file, UTF8(|_, line| {
        print!("{line}");
        Ok(true)
    })).unwrap();
}
```

#### What is the actual behavior?

Run this with 

`cargo run '^[GATC]{20,}$' samplefilehere` leads to one line of output (expected)

`cargo run '^[NGATC]{20,}$' samplefilehere` leads to no output (note me adding an N to the character class, unexpected)

`grep -E '^[NGATC]{20,}$' samplefilehere` works as expected

rg works as expected

Using the underlying regex crate (i.e. removing the code above and just doing a `BufReader.lines()` and then `Regex.is_match()` works as expected

I admit I might do something very very dumb, but I cannot see it and the same expression (which .. I feel is perfectly fine) works in all other tools. Just not with the grep crate and the RegexMatcherBuilder above.

#### What is the expected behavior?

Any regex supporting tool should match my input with the given expression(s)


---

_Comment by @BurntSushi on 2022-07-15 13:08_

OK so this is definitely a bug, but a pretty gnarly one and the result of two different optimizations in play here that result in inconsistent behavior.

Here's the short term thing you want: you should enable `multi_line` on `RegexMatcherBuilder`. Otherwise, your `^` and `$` are interpreted as "beginning and end of text" instead of "beginning and end of text, or beginning and end of line." The latter is generally what you want in a grep tool, and the former is generally not tested as well and thus uncovered a bug.

The longer story here is that there is an optimization in place that tries to look for matching lines the "fast" way, which avoids iterating over every line in the haystack. When this optimization is used, your regex with `^` and `$` doesn't match because your haystack includes a trailing newline character. That is, `^[GATC]+$` does not match `AGTC\n`. `(?m)^[GATC]+$` will match it though.

So... why does `^[GATC]{20,}$` match but `^[NGATC]{20,}$` does not? Well, the former has literals extracted from it. Namely, `G`, `A`, `T` and `C`. ripgrep will look for matches of those literals before running the full regex engine. The second regex exceeds the heuristic literal extraction routine and _no_ literals are extracted from it. This in means ripgrep runs the full regex engine. In both cases though, the "fast" line matching algorithm is used. In that "fast" variant, we first look for a "candidate" match:

https://github.com/BurntSushi/ripgrep/blob/dd104b5bcd7628b2e2ba9ca8e9832c03fd3dab07/crates/searcher/src/searcher/core.rs#L376

If a candidate is found, then we need to confirm it:

https://github.com/BurntSushi/ripgrep/blob/dd104b5bcd7628b2e2ba9ca8e9832c03fd3dab07/crates/searcher/src/searcher/core.rs#L393-L416

So when literals are extracted (in the case of `[GATC]`), the candidate search just looks for matches of `[GATC]`. When a candidate is found, the **line terminators are stripped** and then we try to confirm the match. Since the line terminator is stripped, your regex matches.

But when literals aren't extracted, the candidate search just runs the full regex engine. But since this is a fast path, the meat of the optimization is that the candidate search doesn't care about lines per se and so there is no opportunity to strip line terminators. Thus, your `[NGATC]` regex never matches.

The right thing to do here is to probably disable the fast line search optimization if there are any anchors in the regex that only match beginning/end of test (as opposed to anchors that match either the beginning/end of text or the beginning/end of lines). The "slow" path iterates over every line, strips the line terminators and then runs the regex as you would expect. And in this case, both of your regexes match. This is why if you remove your `line_terminator(b'\n')` option, then you get expected results. That `line_terminator` option is necessary to trigger the fast path.

---

_Closed by @BurntSushi on 2022-07-15 14:01_

---

_Comment by @BurntSushi on 2022-07-15 14:03_

This should be fixed in `grep 0.2.9`, `grep-regex 0.1.10` and `grep-searcher 0.1.9`.

---

_Label `bug` added by @BurntSushi on 2022-07-15 14:03_

---
