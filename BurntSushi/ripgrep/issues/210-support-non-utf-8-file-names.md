```yaml
number: 210
title: Support non-utf-8 file names
type: issue
state: closed
author: bluss
labels:
  - bug
assignees: []
created_at: 2016-11-01T16:08:04Z
updated_at: 2016-11-18T01:22:17Z
url: https://github.com/BurntSushi/ripgrep/issues/210
synced_at: 2026-01-12T18:23:11Z
```

# Support non-utf-8 file names

---

_@bluss_

File names on linux are just bags of bytes. Regular shell tools work with them no problem:

```
$ cat broken� 
hi
$ ls -d broken�
broken?
$ grep "hi" -n broken� 
1:hi
```

The main annoyance is that it poisons the whole directory with ripgrep:

```
$ rg hi *
Argument 'broken�' is not valid UTF-8. Use hex escape sequences to match arbitrary bytes in a pattern (e.g., \xFF).
```

Doing the right thing should be easy - an argument is always an OsStr when it enters the rust program, which is losslessly converted to a `Path`.


---

_Comment by @BurntSushi on 2016-11-01 16:18_

Yup, this is a silly bug. Note that if you run `rg hi` instead of `rg hi *`, then it works. The specific problem is this very ill-advised code in `src/args.rs`:

``` rust
    pub fn parse() -> Result<Args> {
        // Get all of the arguments, being careful to require valid UTF-8.
        let mut argv = vec![];
        for arg in env::args_os() {
            match arg.into_string() {
                Ok(s) => argv.push(s),
                Err(s) => {
                    errored!("Argument '{}' is not valid UTF-8. \
                              Use hex escape sequences to match arbitrary \
                              bytes in a pattern (e.g., \\xFF).",
                              s.to_string_lossy());
                }
            }
        }
        let mut raw: RawArgs =
            Docopt::new(USAGE)
                .and_then(|d| d.argv(argv).version(Some(version())).decode())
                .unwrap_or_else(|e| e.exit());
```

The specific reason why I did this was to catch invalid UTF-8 in _patterns_, but of course, this catches invalid UTF-8 everywhere.

Finally, Docopt has this:

```
    fn get_argv() -> Vec<String> {
        // Hmm, we should probably handle a Unicode decode error here... ---AG
        ::std::env::args().skip(1).collect()
    }
```

... and the argv parser is built around `&str` instead of `&OsStr`.

Sigh. I should probably fix this inside of Docopt, but I also want to switch to clap. See #136.


---

_Label `bug` added by @BurntSushi on 2016-11-01 16:18_

---

_Comment by @bluss on 2016-11-01 16:25_

Yes, clap makes it possible to do this correctly, it's nice that way.


---

_Comment by @bluss on 2016-11-01 16:27_

The reason I got to this point was that I'm trying to make a way to list matches in rg in latest-modified-first order. First trying to just feed it files in that order.


---

_Comment by @BurntSushi on 2016-11-01 16:28_

Oh interesting. You'll also need to pass `-j1`. If you do that, then the results should come back in the order you gave them.


---

_Closed by @BurntSushi on 2016-11-18 01:22_

---
