```yaml
number: 757
title: Indentation of App.about() and other App messages
type: issue
state: closed
author: mandeep
labels: []
assignees: []
created_at: 2016-11-25T06:31:20Z
updated_at: 2018-08-02T03:29:57Z
url: https://github.com/clap-rs/clap/issues/757
synced_at: 2026-01-12T16:14:09Z
```

# Indentation of App.about() and other App messages

---

_@mandeep_

### Rust Version

* rustc 1.11.0 (9b21dcd6a 2016-08-15)

### Affected Version of clap

* version = "2.19.0"

### Expected Behavior Summary

When using methods of App such as about(), before_help(), or after_help(), if the messages
contained in the methods span multiple lines they will not be indented correctly when printed in 
terminal.

### Actual Behavior Summary

The messages in these methods should be indented properly when spanning over multiple lines.

### Steps to Reproduce the issue

See below

### Sample Code or Link to Sample Code

Example:
```
App::new("Amortization")
                        .version("0.1.0")
                        .author("Mandeep <info@mandeep.xyz>")
                        .about(
                            "Amortization requires the borrowed amount of the loan,
                            interest rate of the loan, length of the loan in years,
                            and the total number of payments made annually.

                            Example usage: amortization 500000 4 7 12

                            The resulting amortization schedule will print to terminal.")
```
Results in the following when printed in terminal:

```
Amortization requires the borrowed amount of the loan,
                            interest rate of the loan, length of the loan in years,
                            and the total number of payments made annually.

                            Example usage: amortization 500000 4 7 12

                            The resulting amortization schedule will print to terminal.
```


---

_Comment by @kbknapp on 2016-11-26 20:52_

This has to do with the way Rust handles string literals. The string literal contains all the newlines and tabs/spaces you had to input while typing. This is why you'll see most people unindent their string literals like so:

```rust
    App:;new("Amortization")
        .version("1.1")
        .about(
"Amortization requires the borrowed amount of the loan,
interest rate of the loan, length of the loan in years,
and the total number of payments made annually.

Example usage: amortization 500000 4 7 12

The resulting amortization schedule will print to terminal.")
```

Or use `static` strings.

```rust
static AMORTIZATION_ABOUT: &'static str = 
"Amortization requires the borrowed amount of the loan,
interest rate of the loan, length of the loan in years,
and the total number of payments made annually.

Example usage: amortization 500000 4 7 12

The resulting amortization schedule will print to terminal.
";

fn main() {
    App::new("Amortization")
        .version("1.0")
        .about(AMORTIZATION_ABOUT);
}
```

Also note that those newlines are all hard newlines, meaning `clap` can't wrap intelligently at the terminal width. Sometimes this is desireable though, such as if you'd like to have all your line wraps at a specific length. But it's important to know that if you *don't* specifically want that, and you want `clap` to wrap at the terminal width, you have to escape your newlines like this (it's the same whether you use `static` variables or string literals):

```rust
static AMORTIZATION_ABOUT: &'static str = 
"Amortization requires the borrowed amount of the loan, \
interest rate of the loan, length of the loan in years, \
and the total number of payments made annually. \

Example usage: amortization 500000 4 7 12 

The resulting amortization schedule will print to terminal.
";

fn main() {
    App::new("Amortization")
        .version("1.0")
        .about(AMORTIZATION_ABOUT);
}
```

Unfortunately, this isn't something `clap` could easily fix since it's handled at the Rust level. I'm going to close this, but please feel free to let me know if you have other ideas 😉 

---

_Closed by @kbknapp on 2016-11-26 20:52_

---

_Label `T: RFC / question` added by @kbknapp on 2016-11-26 20:52_

---

_Comment by @mandeep on 2016-11-27 00:32_

I'm currently using the first example which is a little unpleasant to read. In the clap-rs docs, the help() method shows multi-line indented text. Is there something different about this method?

---

_Comment by @kbknapp on 2016-11-27 00:56_

Which page in the docs?

---

_Comment by @mandeep on 2016-11-27 01:24_

Sorry I forgot the link in the previous post:
https://docs.rs/clap/2.19.0/clap/struct.App.html#examples-10

---

_Comment by @kbknapp on 2016-11-27 01:35_

Ah yeah in that case I used the escaping newlines, notice all the `\`s at the end of the lines. When you escape the newline in a string literal Rust also escapes all the whitespace leading up to the first printed character.

---

_Comment by @mandeep on 2016-11-27 01:42_

Oops, I didn't even notice. Thanks for letting me know about that!

---

_Comment by @kbknapp on 2016-11-27 01:57_

No problem :+1:

---
