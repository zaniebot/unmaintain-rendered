```yaml
number: 1266
title: Add ignore-comments ability
type: issue
state: closed
author: sagis-tikal
labels:
  - question
assignees: []
created_at: 2019-04-23T12:28:40Z
updated_at: 2019-09-04T13:26:29Z
url: https://github.com/BurntSushi/ripgrep/issues/1266
synced_at: 2026-01-12T16:13:23Z
```

# Add ignore-comments ability

---

_@sagis-tikal_

#### What version of ripgrep are you using?

ripgrep 0.10.0 (rev 8a7db1a918)
-SIMD -AVX (compiled)
+SIMD +AVX (runtime)

#### How did you install ripgrep?

Installed using the following method
$ curl -LO https://github.com/BurntSushi/ripgrep/releases/download/0.10.0/ripgrep_0.10.0_amd64.deb
$ sudo dpkg -i ripgrep_0.10.0_amd64.deb

#### What operating system are you using ripgrep on?

Ubuntu 14.04 - 64-bit

#### Describe your question, feature request, or bug.
Is it possible to ignore matches found in code comments if a flag is set?

Code example apple.rs:
```rust
fn main() {

    // apple is a tasty fruit
   /*

    apple = 8;
    if apple > 5 {
        println("Don't print that");
    }
    */
   let apple = 1;

    println!("There are {} apples in stock", apple);
}

```
If I run the following command:
``` Bash
rg --ignore-comments apple apple.rs
```

The output I would like to receive:
`let apple = 1;`
`println!("There are {} apples in stock", apple);`

And ignore the line `// apple is a tasty fruit` which also contains the search term "apple"

Congrats on version 11.0.0 :)

---

_Comment by @BurntSushi on 2019-04-23 12:35_

Definitely not, sorry. ripgrep has no specific code awareness built into it, other than a mapping from the type of a file to its extension.

In your specific example, ignoring comment lines is pretty trivial:

```
$ rg apple apple.rs | rv -v '^\s*//'
```

Of course, this doesn't work in all cases since some comments, like `/* ... */`, can span multiple lines. Handling those correctly requires very specific syntax aware parsing that is just not part of ripgrep and probably never will be. ripgrep is, fundamentally, a _line oriented search tool_. A search tool that knows about the syntax of programming languages would look very different.

---

_Closed by @BurntSushi on 2019-04-23 12:35_

---

_Label `question` added by @BurntSushi on 2019-04-23 12:35_

---

_Comment by @sagis-tikal on 2019-04-23 12:38_

Thanks anyway

---

_Comment by @romainreignier on 2019-09-04 09:54_

@BurntSushi Thank you for your snippet but I think it should be:

```bash
$ rg apple apple.rs | rg -v '.*:\s*//'
```

* The second command is `rg`, isn't it?
* And I have replaced `^` by `.*:` because the start of the line is the filename provided by the first `rg` command

I answer to a closed issue because if is the first Google answer for 

> ripgrep ignore comment

---

_Comment by @BurntSushi on 2019-09-04 10:59_

No, my snippet is correct. By default, ripgrep does not include the file name
in the output if you're searching a single file:

```
$ cat /tmp/apple.rs
fn main() {
    // apple is a tasty fruit
   let apple = 1;
    println!("There are {} apples in stock", apple);
}

$ rg apple /tmp/apple.rs
2:    // apple is a tasty fruit
3:   let apple = 1;
4:    println!("There are {} apples in stock", apple);

$ rg apple /tmp/apple.rs | rg -v '^\s*//'
   let apple = 1;
    println!("There are {} apples in stock", apple);
```

If you instead have `--with-filename` enabled (perhaps via a config file),
then my snippet won't work and you need something like what you wrote:

```
$ rg apple /tmp/apple.rs --with-filename
/tmp/apple.rs
2:    // apple is a tasty fruit
3:   let apple = 1;
4:    println!("There are {} apples in stock", apple);

$ rg apple /tmp/apple.rs --with-filename | rg -v '^\s*//'
/tmp/apple.rs:    // apple is a tasty fruit
/tmp/apple.rs:   let apple = 1;
/tmp/apple.rs:    println!("There are {} apples in stock", apple);

$ rg apple /tmp/apple.rs --with-filename | rg -v '^.*?:\s*//'
/tmp/apple.rs:   let apple = 1;
/tmp/apple.rs:    println!("There are {} apples in stock", apple);
```

---

_Comment by @romainreignier on 2019-09-04 13:26_

Oh yes, sorry.
I was using it without the file argument so in a directory.
My apologies.

---
