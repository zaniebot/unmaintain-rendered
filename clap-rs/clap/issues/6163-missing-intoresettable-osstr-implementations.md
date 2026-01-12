```yaml
number: 6163
title: "Missing IntoResettable<OsStr> implementations"
type: issue
state: open
author: philjb
labels: []
assignees: []
created_at: 2025-10-27T23:31:13Z
updated_at: 2025-10-28T17:24:11Z
url: https://github.com/clap-rs/clap/issues/6163
synced_at: 2026-01-12T16:14:17Z
```

# Missing IntoResettable<OsStr> implementations

---

_@philjb_

`Arg::default_value*` and friends use `impl IntoResettable<OsStr>` for the type of defaults for an argument. Runtime arguments arrive as OsString (or OsStr) this makes sense that a default value needs to convert to the same type as if it had been presented on the runtime execution call.

An implementation of `impl IntoResettable<OsStr> for Option<&'static str>` is provided in the builder crate, which is helpful, when the default value provided is a static compile time string:

https://github.com/clap-rs/clap/blob/4e1a565b8adb4f2ad74a9631565574767fdc37ae/clap_builder/src/builder/resettable.rs#L124-L131


But there are other situations where this is not enough. Namely,

* `impl IntoResettable<OsStr> for Option<OsStr>`
* `impl IntoResettable<OsStr> for Option<std::ffi::OsString>`

are generally useful and needed in my case. The first for completeness and the second to go from an owned OS specific string to the clap::builder::OsStr without having to go through a lossy conversion first. 

Implementation of both is straightforward.
```
impl IntoResettable<OsStr> for Option<OsStr> {
    fn into_resettable(self) -> Resettable<OsStr> {
        match self {
            Some(s) => Resettable::Value(s),
            None => Resettable::Reset,
        }
    }
}

#[cfg(feature = "string")]
impl IntoResettable<OsStr> for Option<std::ffi::OsString> {
    fn into_resettable(self) -> Resettable<OsStr> {
        match self {
            Some(s) => Resettable::Value(s.into()),
            None => Resettable::Reset,
        }
    }
}
```

My situation is that I want the default value for an argument to be the home directory. Getting the home directory can fail, so I'd like to handle this gracefully by returning None when it fails, allowing clap to treat the argument as having no default. This would make it work seamlessly with the derive API:

```
// if home::home_dir() fails, returns None and forces clap into normal behavior for a missing value
// if home::home_dir() succeeds, there's no loss of information in converting to a OsStr
fn default_data_dir() -> Option<std::ffi::OsString> {
    home::home_dir()
        .map(|path| path.join(DATA_DIRECTORY_NAME).into_os_string())
}

// somewhere else
#[arg(default_value_t = default_data_dir())]
pub data_directory: Option<PathBuf>,
```

---

_Comment by @epage on 2025-10-28 16:37_

The reason we limit the `Option`s is then a user typing `None` will have an ambiguous type.

We could potentially add a convenience method on `Resettable`.  I have been questioning the value of `Resettable` though and have been wondering about removing it (and setting of `None`).  For a builder user, that would require conditionally building the results.  As you are using the derive API, I can see how that wouldn't work out as well. 

---

_Comment by @philjb on 2025-10-28 17:24_

Yes, interesting point about ambiguity. However, without more IntoResettable impls, it is impossible to express certain things (such as my examples) as the api structure suggests it should be possible to. I can't add my own impls for these types and traits outside the clap crate because of rust's orphan rules. 

I'm suggesting two more impls; The missing `impl IntoResettable<OsStr> for Option<OsStr>` is unfortunate. I understand that adding any more `for Option<T>` impls makes the None type ambiguous. However, Rust provides a native syntax to resolve ambiguous `None`: `None::<&'static str>` for a literal or `-> Option<&'static str> { None }` for a function return type. The complier will readily fail with a clear error and one of these syntaxes is the standard solution. 

I'm biased but I prefer addressing the "impossible" situations that make the api much more flexible even if it means users need to provide a clarifying type, which is idiomatic rust. I'll open a pr and you can decide. 

--------

Regarding removing Resettable: 🤔 , for builder users, would you still provide a way to clear a previously set value? I think you want to; for example, i have my "standard" builder code (maybe in a macro) and I might modify it based on startup conditions, where one modification is clearing a configuration in the builder. So you're either creating a `_clear` fn for every `_set` method or adding a boolean param to all `set` method type signatures ('true' for clear e.g.) or more rusty, replacing `impl IntoResettable<T>` with an explicit enum type: `enum SetOrClear<T> { Set(T), Clear }`. 

The derive api is just so convenient and communicative/expressive-- easy for maintainers to see what is happening and maintain; I wouldn't hobble it. 

Since many derive apis accept fn calls, I was hoping that the api would accept fns that return a `Result<T,clap::Error>` and clap would automatically bubble up an error as part of the parsing, just like an input error in the invocation. That is most general. It permits the binary to reconfigure its cli arguments based on what it sees at runtime (about the machine it's running on; or env; home dir, or even api calls externally e.g. to a service provider like a license server). I don't expect 100% parity in the builder and derive api but I'd try to make as much available in the derive api as reasonable.

---
