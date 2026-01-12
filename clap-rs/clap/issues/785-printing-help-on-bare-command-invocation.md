```yaml
number: 785
title: Printing help on bare command invocation
type: issue
state: closed
author: xpe
labels: []
assignees: []
created_at: 2016-12-25T20:07:26Z
updated_at: 2022-05-25T23:38:36Z
url: https://github.com/clap-rs/clap/issues/785
synced_at: 2026-01-12T16:14:09Z
```

# Printing help on bare command invocation

---

_@xpe_

Thanks for this great library!

I've spent some time looking for the easiest path to tell clap show a full help message on a bare command invocation. (This is the behavior with commands such as `java` and `cloc`.)

These commands don't work:

```
let mut app = App::new("name me").args_from_usage("<input_file> 'Sets the input file to use'");
let matches = app.get_matches(); // moves `app`
app.print_help(); // will not compile since `app` has been moved
```

I understand there are (at least) these options to make the API work as I intend:

1. Clone `app` before passing it to `get_matches`.
2. Find a function similar to `get_matches` that does not move `app`. (I'm not sure if one exists.)

I have a *design* question: What is the rationale for `get_matches` moving the `app` value? Perhaps a borrow would work out better?

P.S. For some context, feel free to see: http://stackoverflow.com/questions/40951538/method-call-on-clap-app-moving-ownership-more-than-once

---

_Comment by @kbknapp on 2016-12-27 04:29_

Thanks!

You have a few options. You can either use [`AppSettings::ArgRequiredElseHelp`](https://docs.rs/clap/2.19.2/clap/enum.AppSettings.html#variant.ArgRequiredElseHelp) or you can keep the move from happening by using [`App::get_matches_from_safe_borrow`](https://docs.rs/clap/2.19.2/clap/struct.App.html#method.get_matches_from_safe_borrow).

The downside with the second approach is it returns a `Result`, and you'll also have to provide the args to match against, either by providing your own arbitrary `Vec`, `std::env::args` or `std::env::args_os` (if you wish to support invalid UTF-8).

An example of the second version, should you choose that route would be:

```rust
let matches = // build app like normal
    .get_matches_from_safe_borrow(std::env:args_os())
    .unwrap_or_else(|e| e.exit())
```

The `e.exit()` portion does exactly what `clap` normally does, presenting an error message and gracefully exiting.

Hope this helps!

---

_Label `T: RFC / question` added by @kbknapp on 2016-12-27 04:29_

---

_Closed by @kbknapp on 2016-12-31 06:19_

---

_Comment by @EvanLuo42 on 2022-05-25 12:35_

> Thanks!
> 
> You have a few options. You can either use [`AppSettings::ArgRequiredElseHelp`](https://docs.rs/clap/2.19.2/clap/enum.AppSettings.html#variant.ArgRequiredElseHelp) or you can keep the move from happening by using [`App::get_matches_from_safe_borrow`](https://docs.rs/clap/2.19.2/clap/struct.App.html#method.get_matches_from_safe_borrow).
> 
> The downside with the second approach is it returns a `Result`, and you'll also have to provide the args to match against, either by providing your own arbitrary `Vec`, `std::env::args` or `std::env::args_os` (if you wish to support invalid UTF-8).
> 
> An example of the second version, should you choose that route would be:
> 
> ```rust
> let matches = // build app like normal
>     .get_matches_from_safe_borrow(std::env:args_os())
>     .unwrap_or_else(|e| e.exit())
> ```
> 
> The `e.exit()` portion does exactly what `clap` normally does, presenting an error message and gracefully exiting.
> 
> Hope this helps!

I used this way to create matches. It's like:
```rust
let mut command = Command::new("ctool");
let matches = command.
    .try_get_matches_from(std::env::args_os())
    .unwrap_or_else(|e| e.exit());
```
And I want to use custom error, I did like this:
```rust
let action_undefined_error = command.error(
        clap::ErrorKind::InvalidValue,
        "The given action is not supported, please enter base64, hex, etc."
    );
```

But it still gave:
`borrow of moved value: command E0382 value borrowed here after move Note: this function takes ownership of the receiver self, which moves command`

How to solve this.

---

_Comment by @epage on 2022-05-25 14:30_

`try_get_matches_from` took ownership of `command` and then you are trying to use `command` again.

Options
- Create a `fn command() -> Command<'static>` and write your errors as `command().error(...)`.  This will slow things down but in the error case so its unlikely anyone will notice
- Use `_mut` variant for parsing, whether [`get_matches_mut`](https://docs.rs/clap/latest/clap/struct.App.html#method.get_matches_mut) or `try_get_matches_from_mut`.

---

_Comment by @EvanLuo42 on 2022-05-25 23:38_

> `try_get_matches_from` took ownership of `command` and then you are trying to use `command` again.
> 
> Options
> 
> * Create a `fn command() -> Command<'static>` and write your errors as `command().error(...)`.  This will slow things down but in the error case so its unlikely anyone will notice
> * Use `_mut` variant for parsing, whether [`get_matches_mut`](https://docs.rs/clap/latest/clap/struct.App.html#method.get_matches_mut) or `try_get_matches_from_mut`.

Thank you, it worked.

---
