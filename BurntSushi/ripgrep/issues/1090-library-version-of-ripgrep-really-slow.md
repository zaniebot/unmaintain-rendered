```yaml
number: 1090
title: Library version of ripgrep really slow
type: issue
state: closed
author: dessalines
labels:
  - question
assignees: []
created_at: 2018-10-22T17:52:02Z
updated_at: 2018-10-22T22:13:42Z
url: https://github.com/BurntSushi/ripgrep/issues/1090
synced_at: 2026-01-12T16:13:22Z
```

# Library version of ripgrep really slow

---

_@dessalines_

So I'm trying to use ripgrep as a library, with the following code, copy-pasted from the official docs.

```rust
fn search_file(file: File, query: &str) -> Result<Vec<String>, Box<Error>> {
  let pattern = query.replace(" ", ".*");

  let matcher = RegexMatcher::new(&pattern)?;
  let mut matches: Vec<String> = vec![];
  Searcher::new().search_file(
    &matcher,
    &file,
    UTF8(|_lnum, line| {
      matches.push(line.to_string());
      Ok(true)
    }),
  )?;

  Ok(matches)
}
```

For some reason this takes sometimes 10 seconds, while the command line version of ripgrep takes ~ 100ms. What could be the reason?

---

_Comment by @BurntSushi on 2018-10-22 18:10_

This isn't an actionable ticket. Please provide a fully reproducible example that I can re-create. Otherwise, it's not possible for me to analyze the performance characteristics of your specific situation.

One possible way in which your example program could be slower than ripgrep proper is if your query has a lot of matches and/or your corpus is very large. Firstly, your program is counting line numbers, and ripgrep doesn't necessarily do that (although it probably does, it's not possible to know since you haven't provided a reproducible example). Secondly, your program is using the `UTF8` sink, which is [explicitly documented to do a UTF-8 validation check](https://github.com/BurntSushi/ripgrep/blob/fb622666206089cf7092ae116612898ac358c91c/grep-searcher/src/sink.rs#L473-L487), which ripgrep doesn't necessarily do by default (but again, it may, but you haven't provided an example so it's not possible for me to know).

---

_Label `question` added by @BurntSushi on 2018-10-22 18:10_

---

_Comment by @dessalines on 2018-10-22 18:30_

The example is from your docs.rs for using grep as a library, [here](https://docs.rs/grep-searcher/0.1.1/grep_searcher/index.html). I'm not sure if that's the preferred way or not.  I just need to know the proper way to use the library version of ripgrep so that its as fast as the command line version. I don't need line numbers. 

I have a repo that has the 300 MB file I'm searching, I could list out git commands that clone the repo so you can replicate it locally, if you wish. 

I've also changed the above `UTF8` sink to `Lossy`, and that doesn't seem to make a difference. 

I'll do this shortly. 

---

_Comment by @dessalines on 2018-10-22 18:55_

Okay, here's the comparison. Same query, one using the library version from the official docs, the other using the command line. 
```sh
git clone -b library_ripgrep https://gitlab.com/dessalines/torrents.csv
cd torrents.csv/server/service
cargo test -- --nocapture
time rg -q -i "sherlock" ../../torrents.csv
```



And the output:
```sh
running 1 test
Query took PT4.906661570S seconds.
test tests::test ... ok

test result: ok. 1 passed; 0 failed; 0 ignored; 0 measured; 0 filtered out

λ rg -q -i "sherlock" ../../torrents.csv

real	0m0.005s
user	0m0.003s
sys	0m0.003s

```
The relevant function is `search_file`.

---

_Comment by @BurntSushi on 2018-10-22 20:00_

Thanks for providing me with an example. In the future, please isolate your code more. If I have to compile a web server to try out your code, then you're making it harder for me to help you.

> I've also changed the above UTF8 sink to Lossy, and that doesn't seem to make a difference.

They both do UTF-8 validation, so I really wouldn't expect there to be a difference. If you want to avoid UTF-8 validation, then you need to use the `Bytes` sink.

> `cargo test -- --nocapture`

If you're doing the timing in a test, then `cargo test` will compile without optimizations by default. Try using `cargo test --release -- --nocapture` instead.

> `time rg -q -i "sherlock" ../../torrents.csv`

This is not equivalent to your code using the ripgrep library. I'd say `time rg -n sherlock ../../torrents.csv > /tmp/out` is closer. I can see a measurable difference between `-n` and `-N` here, so counting lines actually has a cost in your case. The convenience sinks provided for you require counting lines. The performance overhead is very small, but if you don't want it, then you'll need to provide your own implementation of the `Sink` trait.

Try this instead to get bytes:

```rust
fn search_file_bytes(file: File, query: &str) -> Result<Vec<Vec<u8>>, Box<Error>> {
  let pattern = query.replace(" ", ".*");

  let matcher = RegexMatcher::new_line_matcher(&pattern)?;
  let mut matches: Vec<Vec<u8>> = vec![];

  let mut searcher = SearcherBuilder::new()
    .binary_detection(BinaryDetection::quit(b'\x00'))
    // .line_number(false)
    .build();

  searcher.search_file(
    &matcher,
    &file,
    Bytes(|_lnum, line| {
      matches.push(line.to_vec());
      Ok(true)
    }),
  )?;

  Ok(matches)
}
```

After doing this and using `--release`, I get comparable times.

---

_Comment by @dessalines on 2018-10-22 22:13_

Thanks, this did it! The main difference was the release flag. It might be a good idea to add these two methods to the [docs](https://docs.rs/grep-searcher/0.1.1/grep_searcher/). 

---

_Closed by @dessalines on 2018-10-22 22:13_

---
