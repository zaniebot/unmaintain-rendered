```yaml
number: 156
title: rg not parsing regex correctly?
type: issue
state: closed
author: isker
labels:
  - bug
assignees: []
created_at: 2016-10-08T05:19:27Z
updated_at: 2016-10-11T12:51:33Z
url: https://github.com/BurntSushi/ripgrep/issues/156
synced_at: 2026-01-12T18:23:11Z
```

# rg not parsing regex correctly?

---

_@isker_

Hi.  Apologies if this question is nonsensical - I am not super experienced with rg or Rust.

I've got ripgrep 0.2.1 from homebrew.  I cannot get it to correctly handle a regex from the command line, while I can get ag to do so.  I've attached a simple example file.

[testcase.txt](https://github.com/BurntSushi/ripgrep/files/517442/testcase.txt)

Running this regex yields expected results:

```
$ rg '#(?:parse|include)' /tmp/testcase.txt
1:#parse('widgets/foo_bar_macros.vm')
2:#parse ( 'widgets/mobile/foo_bar_macros.vm' )
3:#parse ("widgets/foobarhiddenformfields.vm")
4:#parse ( "widgets/foo_bar_legal.vm" )
5:#include( 'widgets/foo_bar_tips.vm' )
6:#include('widgets/mobile/foo_bar_macros.vm')
7:#include ("widgets/mobile/foo_bar_resetpw.vm")
8:#parse('widgets/foo-bar-macros.vm')
9:#parse ( 'widgets/mobile/foo-bar-macros.vm' )
10:#parse ("widgets/foo-bar-hiddenformfields.vm")
11:#parse ( "widgets/foo-bar-legal.vm" )
12:#include( 'widgets/foo-bar-tips.vm' )
13:#include('widgets/mobile/foo-bar-macros.vm')
14:#include ("widgets/mobile/foo-bar-resetpw.vm")
```

But extending it further does not:

```
$ rg $'#(?:parse|include)\s*\(\s*(?:"|\')[./A-Za-z_-]+(?:"|\')' /tmp/testcase.txt
$
```

The Silver Searcher gets the same thing:

```
$ ag $'#(?:parse|include)\s*\(\s*(?:"|\')[./A-Za-z_-]+(?:"|\')' /tmp/testcase.txt
1:#parse('widgets/foo_bar_macros.vm')
2:#parse ( 'widgets/mobile/foo_bar_macros.vm' )
3:#parse ("widgets/foobarhiddenformfields.vm")
4:#parse ( "widgets/foo_bar_legal.vm" )
5:#include( 'widgets/foo_bar_tips.vm' )
6:#include('widgets/mobile/foo_bar_macros.vm')
7:#include ("widgets/mobile/foo_bar_resetpw.vm")
8:#parse('widgets/foo-bar-macros.vm')
9:#parse ( 'widgets/mobile/foo-bar-macros.vm' )
10:#parse ("widgets/foo-bar-hiddenformfields.vm")
11:#parse ( "widgets/foo-bar-legal.vm" )
12:#include( 'widgets/foo-bar-tips.vm' )
13:#include('widgets/mobile/foo-bar-macros.vm')
14:#include ("widgets/mobile/foo-bar-resetpw.vm")
```

The cause of the difference, as indicated by the output colors, is that ripgrep stops matching at the '.' in each line, even though '.' is included in the character class I am matching on.  --debug output indicates that this character should be matched:

```
        Repeat {
            e: Class(
                CharClass {
                    ranges: [
                        ClassRange {
                            start: '-',   <-- '.' is in this range...
                            end: '/'
                        },
                        ClassRange {
                            start: 'A',
                            end: 'Z'
                        },
                        ClassRange {
                            start: '_',
                            end: '_'
                        },
                        ClassRange {
                            start: 'a',
                            end: 'z'
                        }
                    ]
                }
            ),
            r: OneOrMore,
            greedy: true
        },
```

I tested the regex in a simple Rust program to ensure that the regex itself is fine in the eyes of Rust.

```
extern crate regex;

use regex::Regex;

fn main() {
    let re = Regex::new(r##"#(?:parse|include)\s*\(\s*(?:"|')[./A-Za-z_-]+(?:"|')"##).unwrap();
    assert!(re.is_match("#parse('widgets/foo_bar_macros.vm')"));
    assert!(re.is_match("#parse('widgets/mobile/foo-bar-resetpw.vm')"));
}
```

I'm at a bit of a loss.  Not too sure if this is a bug with rg or with me.  Any idea?  Thanks!


---

_Closed by @BurntSushi on 2016-10-11 02:04_

---

_Comment by @BurntSushi on 2016-10-11 02:05_

This was a great find and should now be fixed.

The actual bug was deep inside the [`regex-syntax`](https://github.com/rust-lang-nursery/regex/commit/e5b40638151f2126ddf25b7ab71faa4192dc4efe) library. In short, there was a bug round tripping a regex string to and from a regex AST, and your specific regex tripped it.


---

_Comment by @isker on 2016-10-11 12:38_

Neat.  Thanks!


---

_Label `bug` added by @BurntSushi on 2016-10-11 12:51_

---
