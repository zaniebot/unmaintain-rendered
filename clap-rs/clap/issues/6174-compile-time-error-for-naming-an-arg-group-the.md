```yaml
number: 6174
title: Compile-time error for naming an arg group the same as an arg
type: issue
state: closed
author: lgarron
labels:
  - C-enhancement
  - A-derive
  - S-triage
assignees: []
created_at: 2025-11-02T04:37:21Z
updated_at: 2025-11-05T02:58:06Z
url: https://github.com/clap-rs/clap/issues/6174
synced_at: 2026-01-12T16:14:17Z
```

# Compile-time error for naming an arg group the same as an arg

---

_@lgarron_

### Please complete the following tasks

- [x] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [x] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Clap Version

4.5.21

### Describe your use case

I accidentally named an arg group the same as one of its arguments: https://github.com/lgarron/repo/blob/8640d936e3cff823c6c53d771992d52f60c05c07/src/commands/version.rs#L94-L98

This compiled (and also produced a reasonable error in manual testing — I didn't test for the naming conflict because I did not expect it), so I went all the way through publication before realizing it failed. Depending on the invocation, you either get a helpful message:

```
thread 'main' panicked at /Users/lgarron/.local/share/cargo/registry/src/index.crates.io-6f17d22bba15001f/clap_builder-4.5.48/src/builder/debug_asserts.rs:289:9:
Command bump: Argument group name 'commit' must not conflict with argument name
note: run with `RUST_BACKTRACE=1` environment variable to display a backtrace
```

… or an error when one (but not both) of the arguments is invoked:

```
error: the argument '--commit-using <COMMIT_USING>' cannot be used with:
  --commit
  --commit-using <COMMIT_USING>
```

### Describe the solution you'd like

It would be really nice for this to be a compile-time error, if possible

### Alternatives, if applicable

_No response_

### Additional Context

_No response_

---

_Label `C-enhancement` added by @lgarron on 2025-11-02 04:37_

---

_Label `S-triage` added by @lgarron on 2025-11-02 04:37_

---

_Comment by @lgarron on 2025-11-02 04:37_

> Depending on the invocation, you either get a helpful message:
> 
> ```
> thread 'main' panicked at /Users/lgarron/.local/share/cargo/registry/src/index.crates.io-6f17d22bba15001f/clap_builder-4.5.48/src/builder/debug_asserts.rs:289:9:
> Command bump: Argument group name 'commit' must not conflict with argument name
> note: run with `RUST_BACKTRACE=1` environment variable to display a backtrace
> ```
> 
> … or an error when one (but not both) of the arguments is invoked:
> 
> ```
> error: the argument '--commit-using <COMMIT_USING>' cannot be used with:
>   --commit
>   --commit-using <COMMIT_USING>
> ```

Oh, this part seems it might be due to release vs. debug mode. 🤷

---

_Label `A-derive` added by @epage on 2025-11-03 16:06_

---

_Comment by @epage on 2025-11-03 16:17_

In a case like
```rust
#[derive(Args, Debug)]
pub(crate) struct CommitOperationArgs {
    #[clap(long, group = "commit")]
    commit: bool,
    /// Same as `--commit`, but uses a specified VCS.
    #[clap(long, group = "commit")]
    pub commit_using: Option<VcsKind>,
}

```
we can move this check from runtime to compile time.  However, there are a lot of variants of this that we can't, including:
```rust
#[derive(Args, Debug)]
pub(crate) struct CommitOperationArgs {
    /// Same as `--commit`, but uses a specified VCS.
    #[clap(long, group = "commit")]
    pub commit_using: Option<VcsKind>,
    #[command(flatten)]
    more: MoreCommitOperationArgs,
}

#[derive(Args, Debug)]
pub(crate) struct MoreCommitOperationArgs {
    #[clap(long)]
    commit: bool,
}
```
or
```rust
#[derive(Args, Debug)]
#[command(group = ArgGroup::new("commit").arg("commit").arg("commit_using'))]
pub(crate) struct CommitOperationArgs {
    #[clap(long)]
    commit: bool,
    /// Same as `--commit`, but uses a specified VCS.
    #[clap(long)]
    pub commit_using: Option<VcsKind>,
}
```

There could be arguments this covering 90% of the cases but it also sets up an expectation that can't be met.

There is also the maintenance burden of duplicating our runtime checks to compile time.  And this isn't an isolated request, see also #3133.

As for debug vs release, we do [encourage forcing these commits to run in a test](https://docs.rs/clap/latest/clap/_derive/_tutorial/index.html#testing) in several places. #4838  is evaluating implicitly doing that for you.

For your specific use case, an alternative would be to change
```rust
#[derive(Args, Debug)]
pub(crate) struct CommitOperationArgs {
    #[clap(long, group = "commit")]
    commit: bool,
    /// Same as `--commit`, but uses a specified VCS.
    #[clap(long, group = "commit")]
    pub commit_using: Option<VcsKind>,
}
```
to
```rust
#[derive(Args, Debug)]
#[group(multiple = false)]
pub(crate) struct CommitOperationArgs {
    #[clap(long, group = "commit")]
    commit: bool,
    /// Same as `--commit`, but uses a specified VCS.
    #[clap(long, group = "commit")]
    pub commit_using: Option<VcsKind>,
}
```

An `ArgGroup` is automatically created for structs.  Since struct fields are not mutually exclusive, `multiple = true` is automatically set.  You can override this with the `#[group]` attribute.  #2621 is tracking support for enums.

---

_Comment by @lgarron on 2025-11-03 19:18_

> There is also the maintenance burden of duplicating our runtime checks to compile time. And this isn't an isolated request, see also [#3133](https://github.com/clap-rs/clap/issues/3133).

Ah, I'm sorry, I didn't find that issue when filing this one.

Thanks for the other details, those are helpful!

I tend to develop in release mode for some projects, but I'll definitely try using `Cli::command().debug_assert();` in CI!

---

_Comment by @lgarron on 2025-11-05 02:58_

> I tend to develop in release mode for some projects, but I'll definitely try using `Cli::command().debug_assert();` in CI!

Alright, this turns out to be a pretty good solution to this. Thanks!

---

_Closed by @lgarron on 2025-11-05 02:58_

---
