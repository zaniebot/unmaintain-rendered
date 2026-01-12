```yaml
number: 486
title: Provide a common fail mechanism
type: issue
state: closed
author: hgrecco
labels:
  - C-enhancement
assignees: []
created_at: 2016-04-18T00:20:32Z
updated_at: 2018-08-02T03:29:49Z
url: https://github.com/clap-rs/clap/issues/486
synced_at: 2026-01-12T16:14:09Z
```

# Provide a common fail mechanism

---

_@hgrecco_

While contributing `cut` to redox's coreutils, I came across the `OptionExt` trait of [libextra](https://github.com/redox-os/libextra/blob/master/src/option.rs#L9). It is really nice as it provides a consistent way to exit a program when a problem occurs. Being clap a command line argument parser, I think it could be very useful if it provides such trait (and its implementation). 

If there is consensus that this is a desired feature, I think we can discuss a few things regarding the implementation. For example, I think it might be a good idea to have a function that accepts the exit code. Additionally I think that there should be a function that defaults to stderr.


---

_Comment by @kbknapp on 2016-04-18 04:19_

Currently we have a [naive implementation](https://github.com/kbknapp/clap-rs/blob/fdbd12e83069386234d3ead0d7d8e70fb09b3551/src/errors.rs#L337-354).

I'm open to extended this to be a better implementation! I do, however, want to maintain a either a degree of separation between what should be handled by the argument parser, and what should be handled in user code (i.e. currently users can use `std::process::exit` and do whatever they want). Not to say we can't support adding those features `clap` but it just wouldn't be the default (or the default would at least be something 99.9% of users would do/need).

Ideas?


---

_Label `T: enhancement` added by @kbknapp on 2016-04-18 04:20_

---

_Label `W: 2.x` added by @kbknapp on 2016-04-18 04:20_

---

_Label `C: errors` added by @kbknapp on 2016-04-18 04:20_

---

_Comment by @hgrecco on 2016-04-19 01:04_

I think that providing a way to terminate a command line program is a good idea. It might not belong in clap, though. I am open for discussions.

Currently clap contains a way to handle argument parsing related errors. I think this works really well, and should not be touched nor expanded (Except maybe provide a way to select which error code to use on exit and internalization of the error message, but that is a different story)

What I am proposing here is an Trait extending `Option` and `Result` that will give clap users a way to exit a program easily. Imagine that the trait is called `OptionExt` and contains just a `unwrap_or_exit` method that takes an error message. This will be a program that uses hyper to download something from the web and then saves it to a file.

``` rust
extern crate clap;
extern crate hyper;

use std::io::{Read, Write};
use std::fs::File;

use clap::{Arg, App, SubCommand, OptionExt };
use hyper::*;

fn main() {
    /// clap stuff resulting in matches

    let url = matches.value_of("url").unwrap()

    let client = Client::new();
    let mut res = client.get(url).send().unwrap_or_exit("Could not send request.");

    let mut buffer = Vec::new();
    res.read_to_end(&mut buffer).unwrap_or_exit("Could not read response.");

    let filename = matches.value_of("filename").unwrap()
    file = File::create(filename).unwrap_or_exit("Could not create output file.");
    file.write_all(&buffer).unwrap_or_exit("Could not write to output file.");
}
```

This might not seem like a big win but it saves quite a lot of code. More importantly keeps the code clean and focused. 

You might argue that the error message is too concise and that the exact reason of the error is not given. That is true, but I think there is a way to provide this as well. 

As I said, I am open for discussions and I might also implement this outside clap and see if it fits.


---

_Comment by @kbknapp on 2016-05-03 01:55_

Ah ok, yeah I can see the utility in this, especially in providing consistent error messages. Let me think on a way to best expose or do this. I think providing an extension to `Option` and `Result` is fine since users would have to manually import those traits to use. Perhaps also a macro or function which allows users to print an error message in the same style that `clap` does for consistency with an optional error code.

Granted I think these are all easy things to add manually, i.e. not in `clap` but I also don't see the harm in including them. Especially for things like, only providing colored output when `clap` is compiled with colors, etc. All of that would be handled automatically behind the scenes with the user doing nothing more than proving a `str` for the message.


---

_Label `T: new feature` added by @kbknapp on 2016-05-03 01:55_

---

_Label `P4: nice to have` added by @kbknapp on 2016-05-03 01:55_

---

_Label `D: intermediate` added by @kbknapp on 2016-05-03 01:55_

---

_Comment by @vks on 2016-08-30 15:08_

> All of that would be handled automatically behind the scenes with the user doing nothing more than proving a str for the message.

Note that this can be achieved with #641 and `Error::exit`.


---

_Comment by @vks on 2016-09-06 22:15_

@hgrecco As of version 2.11.2, your suggestion can be implemented by users of clap:

``` rust
trait UnwrapOrExit<T>
    where Self: Sized
{
    fn unwrap_or_else<F>(self, f: F) -> T
        where F: FnOnce() -> T;

    fn unwrap_or_exit(self, message: &str) -> T {
        let err = clap::Error::with_description(message, clap::ErrorKind::InvalidValue);
        // Ther ErrorKind does not really matter, because we are only interested in exiting and
        // creating a nice error message in case of failure.
        self.unwrap_or_else(|| err.exit())
    }
}

impl<T> UnwrapOrExit<T> for Option<T> {
    fn unwrap_or_else<F>(self, f: F) -> T
        where F: FnOnce() -> T
    {
        self.unwrap_or_else(f)
    }
}

impl<T, E> UnwrapOrExit<T> for Result<T, E> {
    fn unwrap_or_else<F>(self, f: F) -> T
        where F: FnOnce() -> T
    {
        self.unwrap_or_else(|_| f())
    }
}
```


---

_Closed by @kbknapp on 2017-01-30 04:57_

---
