```yaml
number: 2996
title: bytes_searched is inaccurate
type: issue
state: closed
author: amorey
labels: []
assignees: []
created_at: 2025-02-23T05:37:23Z
updated_at: 2025-02-23T21:32:31Z
url: https://github.com/BurntSushi/ripgrep/issues/2996
synced_at: 2026-01-12T16:13:25Z
```

# bytes_searched is inaccurate

---

_@amorey_

### Please tick this box to confirm you have reviewed the above.

- [x] I have a different issue.

### What version of ripgrep are you using?

ripgrep 14.1.1


### How did you install ripgrep?

homebrew

### What operating system are you using ripgrep on?

macOS 15.3.1

### Describe your bug.

I'm trying to get an accurate reading for `bytes_searched` but I noticed that it increments in large steps as I adjust `--max-count`. Is this behavior expected? If so, is there a way to return an accurate value for `bytes_searched`?


### What are the steps to reproduce the behavior?

Search for a string in a file and adjust `--max-count` upwards.



### What is the actual behavior?

Here are some results for the attached file ([loggen.log](https://github.com/user-attachments/files/18928300/loggen.log)):

```console
$ rg --stats -m --debug 30 "about" loggen.log
rg: DEBUG|rg::flags::parse|crates/core/flags/parse.rs:97: no extra arguments found from configuration file
rg: DEBUG|rg::flags::hiargs|crates/core/flags/hiargs.rs:1083: number of paths given to search: 1
rg: DEBUG|rg::flags::hiargs|crates/core/flags/hiargs.rs:1094: is_one_file? true
rg: DEBUG|rg::flags::hiargs|crates/core/flags/hiargs.rs:1269: found hostname for hyperlink configuration: mac.lan
rg: DEBUG|rg::flags::hiargs|crates/core/flags/hiargs.rs:1279: hyperlink format: ""
rg: DEBUG|rg::flags::hiargs|crates/core/flags/hiargs.rs:174: using 1 thread(s)
rg: DEBUG|grep_regex::config|/private/tmp/ripgrep-20240910-8913-ej3fe9/ripgrep-14.1.1/crates/regex/src/config.rs:175: assembling HIR from 1 fixed string literals
...
30 matches
30 matched lines
1 files contained matches
1 files searched
6088 bytes printed
0 bytes searched
0.000255 seconds spent searching
0.001128 seconds

$ rg --stats -m 31 "about" loggen.log
...
31 matches
31 matched lines
1 files contained matches
1 files searched
6315 bytes printed
65432 bytes searched
0.000245 seconds spent searching
0.001430 seconds

$ rg --stats -m 45 "about" loggen.log
...
45 matches
45 matched lines
1 files contained matches
1 files searched
9290 bytes printed
65432 bytes searched
0.000661 seconds spent searching
0.001768 seconds
```

### What is the expected behavior?

I expected `bytes_searched` to return the byte position after the last match found in these examples.

---

_Comment by @BurntSushi on 2025-02-23 12:44_

"bytes searched" is for how many bytes have been searched. It isn't for returning the last position of the match. Moreover, `--stats` are meant to be summary statistics for diagnostic purposes. They may be inaccurate. The docs should be updated to mention this.

As far as I can tell, the current behavior is correct/intended. Your MRE is not as simple as it could be, but I tried this:

```
$ rg --stats -m 1 . loggen.log
1:2025-02-07T13:32:24.851999698Z stdout F 78.21.94.129 - - [07/02/2025:13:32:24] "POST /api/v1/settings HTTP/1.1" 200 162 "-" "Mozilla/5.0 (iPhone13,2; U; CPU iPhone OS 14_0 like Mac OS X) AppleWebKit/602.1.50 (KHTML, like Gecko) Version/10.0 Mobile/15E148 Safari/602.1"

268 matches
1 matched lines
1 files contained matches
1 files searched
271 bytes printed
269 bytes searched
0.000030 seconds spent searching
0.000527 seconds
```

And then increased the number of matches by 1:

```
$ rg --stats -m 2 . loggen.log
1:2025-02-07T13:32:24.851999698Z stdout F 78.21.94.129 - - [07/02/2025:13:32:24] "POST /api/v1/settings HTTP/1.1" 200 162 "-" "Mozilla/5.0 (iPhone13,2; U; CPU iPhone OS 14_0 like Mac OS X) AppleWebKit/602.1.50 (KHTML, like Gecko) Version/10.0 Mobile/15E148 Safari/602.1"
2:2025-02-07T13:32:26.441555379Z stdout F 123.25.44.1 - - [07/02/2025:13:32:26] "GET /settings HTTP/1.1" 200 162 "-" "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/42.0.2311.135 Safari/537.36 Edge/12.246"

512 matches
2 matched lines
1 files contained matches
1 files searched
518 bytes printed
514 bytes searched
0.000064 seconds spent searching
0.000805 seconds
```

Getting the length of the second line and adding it to the number of bytes searched from the first example gives the same result as ripgrep:

```
>>> len('2025-02-07T13:32:26.441555379Z stdout F 123.25.44.1 - - [07/02/
2025:13:32:26] "GET /settings HTTP/1.1" 200 162 "-" "Mozilla/5.0 (Window
s NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/42.
0.2311.135 Safari/537.36 Edge/12.246"\n')
245
>>> 269 + 245
514
```

(Note: the `len` function only works for our purposes here because the string is all ASCII. If it had non-ASCII, you'd want an `encode('utf-8')` at the end.)

It is technically true that ripgrep could stop searching after finding that first match based on the options provided. However, this is not actually the case here, because for me, I have colors enabled. And so ripgrep does actually search the entire line to figure out what it should highlight. Having the "bytes searched" statistic match what ripgrep does precisely here does not seem worth the trouble and I'm unlikely to make such a change.

If you're looking for the offset of the last match, you might be interested in using `--json`. You can also use the `--byte-offset` flag.

---

_Closed by @BurntSushi on 2025-02-23 12:44_

---

_Comment by @amorey on 2025-02-23 14:15_

Thanks! `--byte-offset` is exactly what I was looking for. Sorry I missed it in the flag descriptions.

I have some related questions (happy to post elsewhere if that's more appropriate):

1.   I'm working on a docker log search tool where log lines are stored in JSON or in a text format like this: `<ISO8601 timestamp> stdout F <message>`. When performing a search I only want to match on `<message>` but I want to send the original line to the sink. Does `rg` have a built-in way to do this? I have it working with a custom `Matcher` that wraps `RegexMatcher` but maybe there's a better way.

2. Does `rg` have a built-in way to search line-by-line backwards through a file?


---

_Comment by @BurntSushi on 2025-02-23 14:33_

> Does rg have a built-in way to search line-by-line backwards through a file?

No. You probably want `tac` for this.

> I'm working on a docker log search tool where log lines are stored in JSON or in a text format like this: `<ISO8601 timestamp> stdout F <message>`. When performing a search I only want to match on `<message>` but I want to send the original line to the sink. Does `rg` have a built-in way to do this? I have it working with a custom `Matcher` that wraps `RegexMatcher` but maybe there's a better way.

Nothing beyond just using regex. So if you have a pattern `p` to match on `messasge`, you would create a new pattern `[0-9]{4}-[0-9]{2}-[0-9]{2}T[0-9]{2}:[0-9]{2}:[0-9]{2}Z stdout F {p}`, where `{p}` is substituted by the actual pattern you want to match against `message`.

---

_Comment by @amorey on 2025-02-23 16:33_

> Nothing beyond just using regex. So if you have a pattern p to match on messasge, you would create a new pattern [0-9]{4}-[0-9]{2}-[0-9]{2}T[0-9]{2}:[0-9]{2}:[0-9]{2}Z stdout F {p}, where {p} is substituted by the actual pattern you want to match against message.

I wanted to avoid using regex for the prefix for performance reasons. In my current implementation I'm using `memchr` to find "stdout F" and then sending the rest to a RegexMatcher like this:

```rust
use grep::{
    matcher::{self, Match, Matcher},
    regex::{self, RegexMatcher, RegexMatcherBuilder},
};
use memchr::memmem;

pub struct LogFileRegexMatcher {
    inner: RegexMatcher,
}

impl LogFileRegexMatcher {
    pub fn new(inner_pattern: &str) -> Result<LogFileRegexMatcher, regex::Error> {
        let inner = RegexMatcherBuilder::new()
            .line_terminator(Some(b'\n'))
            .case_smart(false)
            .case_insensitive(false)
            .build(inner_pattern)?;
        Ok(LogFileRegexMatcher { inner })
    }
}

impl Matcher for LogFileRegexMatcher {
    type Captures = regex::RegexCaptures;
    type Error = matcher::NoError;

    fn find_at(&self, haystack: &[u8], start: usize) -> Result<Option<Match>, Self::Error> {
        // Start + 19 starts looking after the non-decimal part of ISO8601 timestamp
        if let Some(offset) = find_log_message_start(haystack, start + 19) {
            self.inner.find_at(haystack, offset)
        } else {
            Ok(None)
        }
    }

    fn new_captures(&self) -> Result<Self::Captures, Self::Error> {
        self.inner.new_captures()
    }
}

fn find_log_message_start(haystack: &[u8], start: usize) -> Option<usize> {
    if start >= haystack.len() {
        return None;
    }

    // Define the literal part that follows the timestamp.
    // The prefix is: "<ISO8601 timestamp> stdout f "
    // We assume the timestamp is of variable length and we just search for " stdout f ".
    let literal = b" stdout F ";
    // Search for the literal in the haystack.
    memmem::find(&haystack[start..], literal).map(|pos| start + pos + literal.len())
}
```

It seems to work but I'm not sure if I'll run into problems with edge cases. Does this approach look ok to you? Could I do something similar with JSON logs and unwrap them before sending the inner message to a `RegexMatcher` instance?

---

_Comment by @BurntSushi on 2025-02-23 16:53_

> I wanted to avoid using regex for the prefix for performance reasons.

Could you actually see a perf difference? For a regex like that with a literal, it _should_ be very fast.

I think your code looks okay? I'm not really context switched into ripgrep's internals enough to say with confidence though.

---

_Comment by @amorey on 2025-02-23 21:27_

Nonscientifically, on my MacBook Air M3 searching for a non-existent phrase in a 1GB file I'm seeing:
- using single regex: ~1.3 sec
- using find_log_message_start(): ~0.3 sec

Note that I modified `find_log_message_start()` so it looks for the first empty space and then jumps ahead past "stdout F" which should be slightly faster than the previous version.

Here's the code for the single regex:
```rust
use std::process::ExitCode;

use grep::cli::stdout;
use grep::printer::StandardBuilder;
use grep::regex::RegexMatcherBuilder;
use grep::searcher::SearcherBuilder;
use grep_searcher::MmapChoice;
use termcolor::ColorChoice;

pub fn run(path: &str, query: &str, first: &u64) -> ExitCode {
    let prefix = "Z.*stdout F ";
    let combined_pattern = format!(r"{}.*{}", prefix, query);
    let matcher = RegexMatcherBuilder::new()
        .line_terminator(Some(b'\n'))
        .case_smart(false)
        .case_insensitive(false)
        .build(&combined_pattern)
        .unwrap();

    let mut searcher = SearcherBuilder::new()
        .line_number(false)
        .memory_map(MmapChoice::never())
        .build();

    let stdout = stdout(ColorChoice::Never);

    let mut printer = StandardBuilder::new()
        .max_matches(Some(*first))
        .byte_offset(true)
        .build(stdout);

    let mut sink = printer.sink(&matcher);

    let _ = searcher.search_path(&matcher, path, &mut sink);

    ExitCode::SUCCESS
}
```

For JSON log lines, could I extract the message from the JSON and then send that as the `haystack` to the inner `RegexMatcher`?


---
