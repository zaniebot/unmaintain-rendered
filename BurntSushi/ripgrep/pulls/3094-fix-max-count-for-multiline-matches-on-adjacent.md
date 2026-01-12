```yaml
number: 3094
title: fix max count for multiline matches on adjacent lines
type: pull_request
state: closed
author: fspv
labels:
  - rollup
assignees: []
draft: true
base: master
head: issue-3076-max-count-multiline
created_at: 2025-07-06T17:38:09Z
updated_at: 2025-09-18T19:09:21Z
url: https://github.com/BurntSushi/ripgrep/pull/3094
synced_at: 2026-01-12T18:23:15Z
```

# fix max count for multiline matches on adjacent lines

---

_@fspv_

Fixes https://github.com/BurntSushi/ripgrep/issues/3076

When we have matches on adjacent lines using multiline searcher we group them together before passing them to the sink. This behaviour allows replacement logic to work correctly, when we try to replace "\n" with something else (see https://github.com/BurntSushi/ripgrep/issues/1311 for more details).

However, this also means that the `--max-count` option is not respected when grouping matches together, because max count is only enforced in a sink. And if we have multiple matches on adjacent lines they will be passed to the sink together, potentially going over the limit.

This PR fixes that by also calculating the number of matches in the searcher itself and breaking early in case max count is reached during the search. This way we make sure we don't overcount the matches.

```
$ cat file
line 2
line 3 x
line 2
line 3
$ target/debug/rg --max-count=1 -U "line 2\nline 3" file --stats --trace
rg: DEBUG|rg::flags::parse|crates/core/flags/parse.rs:97: no extra arguments found from configuration file
rg: DEBUG|rg::flags::hiargs|crates/core/flags/hiargs.rs:1084: number of paths given to search: 1
rg: DEBUG|rg::flags::hiargs|crates/core/flags/hiargs.rs:1095: is_one_file? true
rg: DEBUG|rg::flags::hiargs|crates/core/flags/hiargs.rs:1270: found hostname for hyperlink configuration: devserver
rg: DEBUG|rg::flags::hiargs|crates/core/flags/hiargs.rs:1280: hyperlink format: ""
rg: DEBUG|rg::flags::hiargs|crates/core/flags/hiargs.rs:174: using 1 thread(s)
rg: DEBUG|ignore::gitignore|crates/ignore/src/gitignore.rs:393: opened gitignore file: /home/spv/.gitignore_global
rg: DEBUG|globset|crates/globset/src/lib.rs:448: glob converted to regex: Glob { glob: "**/.aider*", re: "(?-u)^(?:/?|.*/)\\.aider[^/]*$", opts: GlobOptions { case_insensitive: false, literal_separator: tr
ue, backslash_escape: true, empty_alternates: false }, tokens: Tokens([RecursivePrefix, Literal('.'), Literal('a'), Literal('i'), Literal('d'), Literal('e'), Literal('r'), ZeroOrMore]) }
rg: DEBUG|globset|crates/globset/src/lib.rs:448: glob converted to regex: Glob { glob: "**/.claude*", re: "(?-u)^(?:/?|.*/)\\.claude[^/]*$", opts: GlobOptions { case_insensitive: false, literal_separator:
true, backslash_escape: true, empty_alternates: false }, tokens: Tokens([RecursivePrefix, Literal('.'), Literal('c'), Literal('l'), Literal('a'), Literal('u'), Literal('d'), Literal('e'), ZeroOrMore]) }
rg: DEBUG|globset|crates/globset/src/lib.rs:453: built glob set; 0 literals, 8 basenames, 4 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 2 regexes
rg: TRACE|grep_regex::matcher|/home/spv/git/github.com/fspv/ripgrep/crates/regex/src/matcher.rs:66: final regex: "(?:line 2\nline 3)"
rg: TRACE|grep_regex::literal|crates/regex/src/literal.rs:59: skipping inner literal extraction, no line terminator is set
rg: DEBUG|globset|crates/globset/src/lib.rs:453: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
rg: TRACE|rg::search|crates/core/search.rs:254: file: binary detection: BinaryDetection(Convert(0))
rg: TRACE|grep_searcher::searcher|/home/spv/git/github.com/fspv/ripgrep/crates/searcher/src/searcher/mod.rs:695: Some("file"): searching via memory map
rg: TRACE|grep_searcher::searcher|/home/spv/git/github.com/fspv/ripgrep/crates/searcher/src/searcher/mod.rs:794: slice reader: searching via multiline strategy
1:line 2
2:line 3 x

1 matches
2 matched lines
1 files contained matches
1 files searched
20 bytes printed
29 bytes searched
0.000038 seconds spent searching
0.003625 seconds
```

---

_Comment by @fspv on 2025-08-10 08:48_

Hey @BurntSushi. Just getting back to this, don't want to waste the effort. Any chance you can take a look at this? (see more context in https://github.com/BurntSushi/ripgrep/issues/3076)

---

_Comment by @BurntSushi on 2025-09-07 16:35_

Thank you for working on this! Unfortunately, this is a rather difficult bug to fix. Even if #1311 were reverted, then using a pattern like `line 2\nline 3|x\nline 2\n` will still given an unexpected result:

```rust
    #[test]
    fn max_matches_multi_line4() {
        let matcher =
            RegexMatcher::new(r"line 2\nline 3|x\nline 2\n").unwrap();
        let mut printer = StandardBuilder::new()
            .max_matches(Some(1))
            .build(NoColor::new(vec![]));
        SearcherBuilder::new()
            .line_number(false)
            .multi_line(true)
            .build()
            .search_reader(
                &matcher,
                "line 2\nline 3 x\nline 2\nline 3 x\n".as_bytes(),
                printer.sink(&matcher),
            )
            .unwrap();

        let got = printer_contents(&mut printer);
        let expected = "\
line 2
line 3 x
";
        assert_eq_printed!(expected, got);
    }
```

gives

```
expected:
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
line 2
line 3 x

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

got:
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
line 2
line 3 x
line 2

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
```

It provokes the same sort of underlying problem. Indeed, this test fails with this PR too.

I'm going to try and think more about this problem. I wonder if perhaps #1311 needs to be fixed in a different way.

---

_Closed by @BurntSushi on 2025-09-07 16:35_

---

_Comment by @BurntSushi on 2025-09-07 17:53_

I think your instinct to put this in the `Searcher` is probably correct. I don't really see how this can be fixed in the printer easily. The problem is that the `SinkMatch` passed to the printer is fundamentally violating some implicit contracts here. e.g., That every match found in `SinkMatch::buffer` is a valid match that should be printed and handled. When multi-line mode is disabled, this is fine, because `--max-count` is specifically defined to be at the granularity of a _line_. But with multi-line mode, a single match can extend over multiple lines and that should only be counted once.

The other source of tension here is that the `Searcher` tries very hard to avoid not only finding literally every match (since one line may contain multiple matches and the most basic functionality of a grep does not require finding all matches on each line, just that a line matches _somewhere_, once or more) but also to avoid finding the starting position of a match if it isn't needed (because doing so is extra work). This is why a lot of stuff is pushed down in the printer and the _printer_ decides if all of the matches need to be found, not the searcher. But in order to implement `--max-count` when matches can span multiple lines, the searcher does need to find all matches.

I think in the multi-line case this is okay...

But it is not ideal to have a `max_matches` option on both the printer and the searcher. So I'm going to look into adding it to the searcher and removing it from the printer. And also having it only work for multi-line searches is very weird.

If anyone ever wonders why "just split logic out into their own crates" is hard, this is yet another example of it. I clearly fucked up the abstraction boundaries, and because there's a semver boundary at those abstraction boundaries, introducing coupling or tweaking those boundaries is annoyingly difficult.

---

_Label `rollup` added by @BurntSushi on 2025-09-18 19:09_

---
