```yaml
number: 2786
title: "Feature Request: Early Bail Out for Time-Consuming Regex Matches in pcre2"
type: issue
state: closed
author: LangLangBart
labels: []
assignees: []
created_at: 2024-04-23T00:14:15Z
updated_at: 2024-04-23T12:51:04Z
url: https://github.com/BurntSushi/ripgrep/issues/2786
synced_at: 2026-01-12T16:13:24Z
```

# Feature Request: Early Bail Out for Time-Consuming Regex Matches in pcre2

---

_@LangLangBart_

#### Describe your feature request


When using `rg` with `--engine pcre2` for regex patterns that include complex assertions such as **lookaheads**, the search can take an excessive amount of time, especially with very large input strings. It would be beneficial to have an option that allows `rg` to skip strings that exceed a certain processing time threshold, thus preventing long-running searches.

The `rg` command at the bottom of the code block below took around 10 minutes to complete.
```bash
command rg --version
# ripgrep 14.1.0

# features:-simd-accel,+pcre2
# simd(compile):+SSE2,+SSSE3,-AVX2
# simd(runtime):+SSE2,+SSSE3,-AVX2

# PCRE2 10.42 is available (JIT is available)

# Generate a random alphanumeric string and store it in a file
printf "%s" "$(LC_ALL=C tr -dc "[:alnum:]" </dev/urandom | head -c 700000)" >/tmp/rg_test
# Search the file using lookahead assertions
command rg --engine pcre2 '(?=.*test)(?=.*ripgrep)' /tmp/rg_test
```

An early bail-out option in `rg` would help users prioritize search speed when processing complex regex patterns on large data, avoiding long waits for unlikely matches.


#### Example Usage
```bash
command rg --engine pcre2 --max-match-time-pcre2 1000 '(?=.*test)(?=.*ripgrep)'  /tmp/rg_test
```

In this example, `--max-match-time-pcre2 1000` is a new option that instructs `rg` to spend no more than 1000 milliseconds attempting to find a match in a line. If the match attempt exceeds this duration, `rg` will skip it.

<details>

---

<summary>Background Info</summary>

To provide some background information on why this feature request was created: After downloading the `Mailing List Archive` for `zsh`, `rg` was used to search for two words in the same line.

```bash
command rg --engine pcre2 '(?=.*export)(?=.*function)'
```

However, there is at least one base64 string with approximately 700,000 characters, which caused `rg` to take a considerable amount of time to process.
[zsh.org/mla/workers/2015/msg03191.html](https://zsh.org/mla/workers/2015/msg03191.html)

---

</details>


---

_Comment by @ltrzesniewski on 2024-04-23 09:50_

FWIW, here's a faster way to match two words in the same line: `^(?=.*?export)(?=.*?function)`

---

_Comment by @LangLangBart on 2024-04-23 11:44_

> ^(?=.*?export)(?=.*?function)

It serves me well, thank you.


---

_Closed by @LangLangBart on 2024-04-23 11:44_

---

_Comment by @BurntSushi on 2024-04-23 11:44_

Or also, `rg export | rg function`.

Anyway, PCRE2 doesn't support a way to say, "if the search is taking longer than X time, stop." I don't know of any regex engine that supports that. If you think for a moment about how something like that would be implemented, it's pretty clear why: imagine trying to check a timeout value when you're in the middle of some [vectorized SIMD loop](https://github.com/BurntSushi/memchr/blob/20ef11fa92d8b393735f10906c436a8ce6e792a4/src/arch/generic/memchr.rs#L143-L230). It would trash performance. _And_ this sort of timeout check would need to be in virtually every loop everywhere. Ain't going to happen.

PCRE2, like most regex engines, does expose [other types of resource limits](https://www.pcre.org/current/doc/html/pcre2api.html#matchcontext). But none of them really approximate "time."

Your best bet is to filter out longer lines yourself (`rg -v '.{100}'` or something), or use a technique that doesn't require using a backtracking engine. Hopefully some day https://github.com/BurntSushi/ripgrep/issues/875 will happen and that will probably be the "right" way to solve your particular problem.

---

_Comment by @ltrzesniewski on 2024-04-23 12:51_

I don't mean to be pedantic, but I'll add a few comments since it's an interestic topic 🙂 

The .NET regex engine supports timeouts (there's a constructor overload which takes a timeout parameter). This feature is mainly provided for searching with untrusted patterns in order to avoid DoS attacks. I haven't looked at how it's implemented but I believe a check is inserted when backtracking (and maybe in some other cases such as lookarounds). The check is [simple enough](https://github.com/dotnet/runtime/blob/56dcfd700383a9462d4b8632e88abd75b4252357/src/libraries/System.Text.RegularExpressions/src/System/Text/RegularExpressions/RegexRunner.cs#L356-L365) but it still needs to retrieve the clock when enabled. In any case [this article](https://devblogs.microsoft.com/dotnet/regular-expression-improvements-in-dotnet-7/) states:

> `Regex` supports timeouts, and guarantees that it will only do at most `O(n)` work (where `n` is the length of the input) between timeout checks

As for PCRE2, it has a [`PCRE2_AUTO_CALLOUT` flag](https://pcre2project.github.io/pcre2/doc/html/pcre2callout.html) which causes the engine to call a user-provided function before each pattern "item", and you can cancel execution from there, so in theory you could implement timeouts, but that would surely wreck the matching performance.


---
