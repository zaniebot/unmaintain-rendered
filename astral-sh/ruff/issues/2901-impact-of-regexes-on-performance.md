```yaml
number: 2901
title: impact of regexes on performance?
type: issue
state: closed
author: BurntSushi
labels:
  - question
assignees: []
created_at: 2023-02-14T21:01:08Z
updated_at: 2023-02-17T12:52:18Z
url: https://github.com/astral-sh/ruff/issues/2901
synced_at: 2026-01-12T15:54:43Z
```

# impact of regexes on performance?

---

_@BurntSushi_

Hi! I'm the author of the regex crate you're using, and I noticed that you're using _a lot_ of regexes in your tool. Do you have a rough idea of how much impact they have on the perf of ruff? If they are a bottleneck I do wonder if there is some room for improvement. For example, I noticed that Unicode mode appears to be enabled for most/all regexes. Do you know if this is strictly required? Just as one example, I see this regex is using Unicode word boundaries:

https://github.com/charliermarsh/ruff/blob/08e0b76587a3d6b291086b66ee2a6d828bbb22c9/crates/ruff/src/rules/pycodestyle/rules/whitespace_around_keywords.rs#L51-L53

but since the alternation is just a bunch of ASCII literals, my guess is that you don't need the word boundary to be Unicode-aware.

So I guess here are my questions more succinctly:

1. Do you have a sense of how much impact regexes have on the perf of ruff?
2. How good is your test suite? For example, if the regexes were tweaked slightly, would you be confident enough in your test suite if it passed?
3. Do you have any way of running benchmarks on realistic workloads as to gauge the impact of changes to the regexes?

---

_Comment by @charliermarsh on 2023-02-14 22:09_

Hey thanks so much for chiming in. Big fan of your work, I often look to your crates for guidance.

> Do you have a sense of how much impact regexes have on the perf of ruff?

They can have a non-trivial impact though it depends on the exact rules that the user has enabled (we try to skip unnecessary work wherever we can, so the selected ruleset drives a lot of the performance characteristics).

The Pycodestyle rules involve a lot of regex (those are 1:1 ports from the originating Python project), and those are pretty impactful. For example, turning off the regex you linked (replacing it with `line.find("False")`, just as an example) reduces runtime from 333.6ms to 255.7ms for that rule alone. Those rules are actually pretty new and I've avoided releasing them (they're behind a feature flag) because I need to optimize the implementation, so it's funny that you ask. (Note that there's overhead in tokenizing and building up the logical lines, which has to happen before we run the regex in the first place, but hopefully that gives you a sense for the impact.)

The Pydocstyle rules use a lot of regex too.

Outside of those, the one generic regex that _always_ runs is the `# noqa` regex, used to detect ignored rules on a per-line basis: https://github.com/charliermarsh/ruff/blob/08e0b76587a3d6b291086b66ee2a6d828bbb22c9/crates/ruff/src/noqa.rs#L17

We've done a few obvious things to ensure that the `# noqa` regex runs as infrequently as possible (i.e., only on lines that contain violations and end with comments), so in my benchmarks, it doesn't have a significant effect.

> How good is your test suite? For example, if the regexes were tweaked slightly, would you be confident enough in your test suite if it passed?

In general I have high confidence in the test suite. I rely on it a lot. As a litmus test, I'll generally merge version bumps etc. ~blindly as long as they pass the test suite.

One caveat: our Unicode coverage specifically isn't as strong as I'd like. We occasionally see bug reports for rules that make invalid single-byte / ASCII assumptions. Of course, we then fix those bugs and augment the suite, so there is some Unicode coverage. But I'd like to increase its scope, and I'd be happy to do so if you have ideas to try around regex optimization...

> Do you have any way of running benchmarks on realistic workloads as to gauge the impact of changes to the regexes?

Yeah, I typically benchmark over CPython, where even small changes are noticeable (though there's almost always some overhead, e.g., the parsing step often takes the majority of the time, so if you're optimizing anything in Ruff itself, that's kind of your baseline).

For example, to benchmark the rule you mentioned, I'd clone CPython, then run:

```
cargo build --release --all-features && \
    hyperfine -i --warmup 10 --runs 100 \
    "./target/release/ruff ./crates/ruff/resources/test/cpython --no-cache --silent --select E271"
```


---

_Label `question` added by @charliermarsh on 2023-02-14 22:10_

---

_Comment by @BurntSushi on 2023-02-17 02:46_

I plucked out several regexes that looked "interesting" to me, and throughput actually seems pretty reasonable all around, including your `noqa` regex. I do have a big refactor landing in the regex crate Real Soon Now that will actually give you some small boosts in places without having to do anything (`regexold` is the status quo and `regex/automata/meta` is the new engine):

```
$ rebar cmp tmp/results.csv -e meta -e regexold -e pcre2 -e '^re2$'
benchmark                             pcre2/jit            re2                 rust/regex/meta      rust/regexold
---------                             ---------            ---                 ---------------      -------------
wild/ruff/noqa                        311.5 MB/s (5.96x)   537.7 MB/s (3.45x)  1855.7 MB/s (1.00x)  730.5 MB/s (2.54x)
wild/ruff/space-around-operator       180.3 MB/s (2.45x)   318.2 MB/s (1.39x)  442.5 MB/s (1.00x)   332.6 MB/s (1.33x)
wild/ruff/string-quote-prefix         1849.0 MB/s (1.24x)  652.8 MB/s (3.52x)  2.2 GB/s (1.00x)     876.7 MB/s (2.62x)
wild/ruff/unnecessary-coding-comment  1643.3 MB/s (1.00x)  769.8 MB/s (2.13x)  1384.9 MB/s (1.19x)  1286.1 MB/s (1.28x)
wild/ruff/whitespace-around-keywords  105.8 MB/s (2.96x)   -                   313.5 MB/s (1.00x)   188.7 MB/s (1.66x)
```

I also tried disabling Unicode and I didn't see a huge difference here.

I didn't thoroughly investigate every regex you use in Ruff, but I plucked out the ones that looked (to my eyes) like they could be slow. Many of your regexes have simple literal prefixes or are anchored, which means they're likely to do decently well.

I'm not sure if you want to keep this issue open as I don't think there is anything actionable here. But I'd say that a _very rough_ first pass at finding any particularly slow regexes is done. But someone else might want to do a deeper dive. I also likely went about this in a poor way. I looked for problems from the bottom up instead of trying Ruff itself on real data and looking for bottlenecks.

---

_Comment by @charliermarsh on 2023-02-17 02:55_

Rad, thanks for doing this.

> I do have a big refactor landing in the regex crate Real Soon Now that will actually give you some small boosts in places without having to do anything.

If it's at all helpful to test that refactor on Ruff I'm happy to do so -- though maybe you already ran the test suite against it, not sure.

I'll close this for now -- but your response got me thinking about at least one specific case that's kind of funny:

```rust
static SHEBANG_REGEX: Lazy<Regex> =
    Lazy::new(|| Regex::new(r"^(?P<spaces>\s*)#!(?P<directive>.*)").unwrap());

#[derive(Debug)]
pub enum ShebangDirective<'a> {
    None,
    // whitespace length, start of shebang, end, shebang contents
    Match(usize, usize, usize, &'a str),
}

pub fn extract_shebang(line: &str) -> ShebangDirective {
    // Minor optimization to avoid matches in the common case.
    if !line.contains('!') {
        return ShebangDirective::None;
    }
    match SHEBANG_REGEX.captures(line) {
        Some(caps) => match caps.name("spaces") {
            Some(spaces) => match caps.name("directive") {
                Some(matches) => ShebangDirective::Match(
                    spaces.as_str().chars().count(),
                    matches.start(),
                    matches.end(),
                    matches.as_str(),
                ),
                None => ShebangDirective::None,
            },
            None => ShebangDirective::None,
        },
        None => ShebangDirective::None,
    }
}
```

As the name suggests, we're trying to find `shebang` lines. It turns out that looking for an exclamation mark prior to running the regex is faster in practice (105ms vs. 80ms on the CPython benchmark on my machine). This may be a data-dependent optimization (exclamation marks are extremely rare in Python code, so they'd almost always indicate a shebang), but I thought I'd call it out.


---

_Closed by @charliermarsh on 2023-02-17 02:55_

---

_Comment by @charliermarsh on 2023-02-17 02:55_

(BTW the speed-ups on your benchmark look really significant? That's awesome.)

---

_Comment by @BurntSushi on 2023-02-17 03:07_

Yeah I did see the shebang regex but skipped over it and missed your `!` prefilter. I added it to my benchmark suite (currently private but will be public Real Soon Now):

```
$ rebar cmp tmp/results.csv -e meta -e regexold
benchmark          rust/regex/meta      rust/regexold
---------          ---------------      -------------
wild/ruff/shebang  1048.3 MB/s (1.00x)  268.4 MB/s (3.91x)
```

So you'll get a boost here too. You're absolutely correct that data dependent optimizations can play a role here. The new regex engine _is_ capable of plucking out that `!` and searching for it, but its current heuristic is to throw away those sorts of first-pass checks for anchored regexes. It's a dark art because there's generally an assumption that an anchored regex is sensitive to latency due to likely searching a small haystack. And in particular, if the majority case is that a match is found (think about extracting bits of information from every line in a log file), then running that prefilter first is just going to slow things down. It's hard to know what to do up front, so it's plausible that your `!` optimization will remain effective. Although, when the refactor lands, it looks like there will be a big boost here. (It's due to a new "one pass" regex engine and not a prefilter on `!`.)

---

_Comment by @BurntSushi on 2023-02-17 03:09_

> (BTW the speed-ups on your benchmark look really significant? That's awesome.)

Yeah. My favorite is the noqa regex. It will actually pluck the `noqa` literal out of the middle of the regex:

```
[2023-02-17T03:07:30Z DEBUG regex_automata::meta::reverse_inner] inner prefixes (len=Some(32)) extracted before optimization: Seq[I("# NOQA:"), E("# NOQA"), I("# NOQa:"), E("# NOQa"), I("# NOqA:"), E("# NOqA"), I("# NOqa:"), E("# NOqa"), I("# NoQA:"), E("# NoQA"), I("# NoQa:"), E("# NoQa"), I("# NoqA:"), E("# NoqA"), I("# Noqa:"), E("# Noqa"), I("# nOQA:"), E("# nOQA"), I("# nOQa:"), E("# nOQa"), I("# nOqA:"), E("# nOqA"), I("# nOqa:"), E("# nOqa"), I("# noQA:"), E("# noQA"), I("# noQa:"), E("# noQa"), I("# noqA:"), E("# noqA"), I("# noqa:"), E("# noqa")]
[2023-02-17T03:07:30Z DEBUG regex_automata::meta::reverse_inner] inner prefixes (len=Some(1)) extracted after optimization: Seq[I("#")]
[2023-02-17T03:07:30Z DEBUG regex_automata::util::prefilter::imp] prefilter built: memchr
```

and then realize that it should just look for the `#` prefix, which is a pretty rare byte overall. So it does well here.

Once it plucks that inner literal out, it splits the regex in two. After finding a `#`, it matches `(?P<spaces>\s*)` in reverse and then `(?::\s?(?P<codes>([A-Z]+[0-9]+(?:[,\s]+)?)+))?)` forwards. (Well, it's a little more complicated then that, but that's the idea.)

---

_Comment by @charliermarsh on 2023-02-17 12:52_

> And in particular, if the majority case is that a match is found (think about extracting bits of information from every line in a log file), then running that prefilter first is just going to slow things down.

Yeah this makes sense -- I figured as much. I wonder if there's been any exploration of some kind of hinting for regexes to take advantage of those optimizations. I don't know much about that design space TBH :)

> My favorite is the noqa regex.

Yeah that's extremely cool. These look like fun problems. Or at least I hope they're fun problems.


---
