```yaml
number: 1219
title: ripgrep breaks while read loop. maybe stdin detection issue?
type: issue
state: closed
author: davidszotten
labels: []
assignees: []
created_at: 2019-03-11T11:56:31Z
updated_at: 2020-08-25T11:55:37Z
url: https://github.com/BurntSushi/ripgrep/issues/1219
synced_at: 2026-01-12T16:13:23Z
```

# ripgrep breaks while read loop. maybe stdin detection issue?

---

_@davidszotten_

#### What version of ripgrep are you using?

testing 
```
$ rg --version
ripgrep 0.10.0
-SIMD -AVX (compiled)
+SIMD +AVX (runtime)
```

and 
```
$ ./target/release/rg --version
ripgrep 0.10.0 (rev 0913972104)
-SIMD -AVX (compiled)
+SIMD +AVX (runtime)
```
#### How did you install ripgrep?

homebrew / git + compiled manually


#### What operating system are you using ripgrep on?

osx 10.14.2

#### Describe your question, feature request, or bug.

I'm getting surprising behaviours calling `ripgrep` inside a `while read` loop

#### If this is a bug, what are the steps to reproduce the behavior?

[empty dir]

0.10.0 from homebrew (full -v above)
```
$ echo -e "foo\nbar" | while read flag; do echo "flag: $flag" && echo "rg: $(rg -l $flag)" ; done

flag: foo
<stdin>: output kind PathWithMatch requires a file path
rg:
flag: bar
<stdin>: output kind PathWithMatch requires a file path
rg:
```


master manually compiled (full -v above)
```
echo -e "foo\nbar" | while read flag; do echo "flag: $flag" && echo "rg: $(../target/release/rg -l $flag)" ; done
flag: foo
rg:
```

(n.b. aborts the while loop; `flag: bar` is missing)


#### If this is a bug, what is the actual behavior?

repeat of the above with `--debug` (makes it hard to see the regular output)

0.10.0 from homebrew (full -v above)
```
$ echo -e "foo\nbar" | while read flag; do echo "flag: $flag" && echo "rg: $(rg --debug -l $flag)" ; done
flag: foo
DEBUG|grep_regex::literal|grep-regex/src/literal.rs:58: literal prefixes detected: Literals { lits: [Complete(foo)], limit_size: 250, limit_class: 10 }
DEBUG|globset|globset/src/lib.rs:429: built glob set; 0 literals, 0 basenames, 8 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|globset/src/lib.rs:424: glob converted to regex: Glob { glob: "**/.*.sw*", re: "(?-u)^(?:/?|.*/)\\..*\\.sw.*$", opts: GlobOptions { case_insensitive: false, literal_separator: false, backslash_escape: true }, tokens: Tokens([RecursivePrefix, Literal('.'), ZeroOrMore, Literal('.'), Literal('s'), Literal('w'), ZeroOrMore]) }
DEBUG|globset|globset/src/lib.rs:424: glob converted to regex: Glob { glob: "**/.sw*", re: "(?-u)^(?:/?|.*/)\\.sw.*$", opts: GlobOptions { case_insensitive: false, literal_separator: false, backslash_escape: true }, tokens: Tokens([RecursivePrefix, Literal('.'), Literal('s'), Literal('w'), ZeroOrMore]) }
DEBUG|globset|globset/src/lib.rs:429: built glob set; 0 literals, 5 basenames, 1 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 2 regexes
<stdin>: output kind PathWithMatch requires a file path
rg:
flag: bar
DEBUG|grep_regex::literal|grep-regex/src/literal.rs:58: literal prefixes detected: Literals { lits: [Complete(bar)], limit_size: 250, limit_class: 10 }
DEBUG|globset|globset/src/lib.rs:429: built glob set; 0 literals, 0 basenames, 8 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|globset/src/lib.rs:424: glob converted to regex: Glob { glob: "**/.*.sw*", re: "(?-u)^(?:/?|.*/)\\..*\\.sw.*$", opts: GlobOptions { case_insensitive: false, literal_separator: false, backslash_escape: true }, tokens: Tokens([RecursivePrefix, Literal('.'), ZeroOrMore, Literal('.'), Literal('s'), Literal('w'), ZeroOrMore]) }
DEBUG|globset|globset/src/lib.rs:424: glob converted to regex: Glob { glob: "**/.sw*", re: "(?-u)^(?:/?|.*/)\\.sw.*$", opts: GlobOptions { case_insensitive: false, literal_separator: false, backslash_escape: true }, tokens: Tokens([RecursivePrefix, Literal('.'), Literal('s'), Literal('w'), ZeroOrMore]) }
DEBUG|globset|globset/src/lib.rs:429: built glob set; 0 literals, 5 basenames, 1 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 2 regexes
<stdin>: output kind PathWithMatch requires a file path
rg:
```

master manually compiled (full -v above)
```
$ echo -e "foo\nbar" | while read flag; do echo "flag: $flag" && echo "rg: $(../target/release/rg --debug -l $flag)" ; done
flag: foo
DEBUG|grep_regex::literal|grep-regex/src/literal.rs:58: literal prefixes detected: Literals { lits: [Complete(foo)], limit_size: 250, limit_class: 10 }
DEBUG|globset|globset/src/lib.rs:434: built glob set; 0 literals, 0 basenames, 11 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|globset/src/lib.rs:429: glob converted to regex: Glob { glob: "**/.*.sw*", re: "(?-u)^(?:/?|.*/)\\.[^/]*\\.sw[^/]*$", opts: GlobOptions { case_insensitive: false, literal_separator: true, backslash_escape: true }, tokens: Tokens([RecursivePrefix, Literal('.'), ZeroOrMore, Literal('.'), Literal('s'), Literal('w'), ZeroOrMore]) }
DEBUG|globset|globset/src/lib.rs:429: glob converted to regex: Glob { glob: "**/.sw*", re: "(?-u)^(?:/?|.*/)\\.sw[^/]*$", opts: GlobOptions { case_insensitive: false, literal_separator: true, backslash_escape: true }, tokens: Tokens([RecursivePrefix, Literal('.'), Literal('s'), Literal('w'), ZeroOrMore]) }
DEBUG|globset|globset/src/lib.rs:434: built glob set; 0 literals, 5 basenames, 1 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 2 regexes
rg:
```


#### If this is a bug, what is the expected behavior?

on master, i would expect the while loop to continue, (on 0.10.0 am surprised by the `<stdin>: output kind PathWithMatch requires a file path` output)

What do you think ripgrep should have done?

is there maybe still some issue with stdin detection?


---

_Comment by @BurntSushi on 2019-03-11 12:17_

For `0.10.0`, you're hitting #1106, which is fixed on master.

As for behavior on master... I'm not sure what's going on actually. At first I thought it might be because you were executing a search with no results, and therefore would get a `1` exit code which might cause the loop to stop? But `grep` has the same behavior and it still works:

```
$ echo -e "foo\nbar" | while read flag; do echo "flag: $flag" && echo "rg: $(rg -l $flag)" ; done
flag: foo
rg:

$ echo -e "foo\nbar" | while read flag; do echo "flag: $flag" && echo "rg: $(grep -r -l $flag)" ; done
flag: foo
rg:
flag: bar
rg:

$ for flag in foo bar; do echo "flag: $flag" && echo "rg: $(rg -l $flag)"; done
flag: foo
rg:
flag: bar
rg:

$ for flag in foo bar; do echo "flag: $flag" && echo "rg: $(grep -r -l $flag)"; done
flag: foo
rg:
flag: bar
rg:
```

I couldn't imagine how stdin detection would be impacted here, but it looks like you're right. We can test it by supplying a file path to search directly, which overrides stdin detection.

```
$ echo -e "foo\nbar" | while read flag; do echo "flag: $flag" && echo "rg: $(rg -l $flag ./)" ; done
flag: foo
rg:
flag: bar
rg:
```

Any shell wizards want to explain this? :-)

---

_Comment by @davidszotten on 2019-03-11 15:40_

where is the code for the stdin detection? i tried 
```
use grep_cli::is_readable_stdin;

fn main() {
    println!("{:?}", is_readable_stdin());
}
``` 

but unless i'm making some mistake, this doesn't reproduce the bug:
```
echo -e "foo\nbar" | while read flag; do echo "flag: $flag" && echo "rg: $(./target/release/rg-bug $flag)" ; done
flag: foo
rg: true
flag: bar
rg: true
```

(maybe narrowing that down to a minimal example would give us some insights)

---

_Comment by @BurntSushi on 2019-03-11 15:46_

That _does_ appear to reproduce the bug though? That's saying that stdin _is_ readable, which will in turn cause ripgrep to try to read stdin. Normally that manifests as ripgrep blocking on stdin, but my guess is that something is then immediately closing stdin which causes ripgrep to terminate without finding anything.

The specific place where ripgrep does stdin detection is here:

https://github.com/BurntSushi/ripgrep/blob/09139721047b1cda6ad88dbf89dc5fa74c66a3a2/src/args.rs#L1188-L1204

In this case, all four parts of that conditional are return false, which means ripgrep will try to search stdin. Interesting.

---

_Comment by @MichaelAquilina on 2019-03-11 16:09_

For what it's worth, I tested this on zsh and it's also present there - so it's not limited to just bash

---

_Comment by @davidszotten on 2019-03-11 16:37_

ok so i wonder if what's happening is this: `while read` is just reading from stdin, the same stdin that commands in the body use:

```rust
use std::io::{self, Read};
fn main() -> io::Result<()> {
    let mut buffer = String::new();
    io::stdin().read_to_string(&mut buffer)?;
    println!("read: '{}'", buffer);
    Ok(())
}
```

```bash
echo -e "foo\nbar" | while read flag; do echo "flag: $flag" && echo "rg: $(./target/debug/rg-bug $flag)" ; done
flag: foo
rg: read: 'bar
'
```

---

_Comment by @okdana on 2019-03-11 16:38_

`rg` detecting standard input inside a `while | read` loop is expected. This is the same sort of problem people run into when they try to call `ssh` in a loop (for which it has the `-n` option).

As for why the loop stops, it's because `rg` has consumed the rest of standard input (and found nothing, because the first `read` already consumed the flag it's searching for), so the second call to `read` returns `1`. Maybe having the second line contain the first better illustrates that:

```
% echo -e "foo\nbar" | while read flag; do echo "flag: $flag" && echo "rg: $(\rg -l $flag)" ; done
flag: foo
rg: 
% echo -e "foo\nfoobar" | while read flag; do echo "flag: $flag" && echo "rg: $(\rg -l $flag)" ; done
flag: foo
rg: <stdin>
```

If you're using GNU `grep` for your `grep` comparison, i think it works because giving it `-r` just makes it ignore standard input.

A general work-around is to have your loop input go to a different file descriptor:

```
% echo bar > tmpfile
% while read -u9 flag; do echo "flag: $flag" && echo "rg: $(\rg -l $flag)" ; done 9< <( echo  -e 'foo\nbar' )
flag: foo
rg: 
flag: bar
rg: tmpfile
```

---

_Comment by @davidszotten on 2019-03-11 17:08_

thanks @okdana ! (though in this case a nicer workaround might be to specify the path to override stdin detection)

or i might just go back to `for` loops. (i originally switched to `while read` because it was a bit less to write, but i think your (albeit very cool!) workaround negates that

thanks again for your helpful explanation

will close as it seems ripgrep is working correctly

---

_Closed by @davidszotten on 2019-03-11 17:08_

---

_Comment by @knutwannheden on 2020-08-25 07:31_

Also just stumbled across this. Would it make sense for `rg` to offer something like `ssh`'s `-n` option?

---

_Comment by @BurntSushi on 2020-08-25 11:50_

Why add a new flag when specifying the directory to search fixes the problem? Use `rg foo ./` instead of `rg foo`.

---

_Comment by @knutwannheden on 2020-08-25 11:55_

> Why add a new flag when specifying the directory to search fixes the problem? Use `rg foo ./` instead of `rg foo`.

Yes, it is what I ended up doing. I was thinking an option or some text in the `rg --help` output might have helped me find the source of the problem more quickly, but maybe that is just wishful thinking :-)

---
