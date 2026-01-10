```yaml
number: 11863
title: "[red-knot]: Add a VendoredFileSystem implementation"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
assignees: []
merged: true
base: main
head: vendored-filesystem
created_at: 2024-06-13T19:16:27Z
updated_at: 2024-06-18T15:59:42Z
url: https://github.com/astral-sh/ruff/pull/11863
synced_at: 2026-01-10T21:56:00Z
```

# [red-knot]: Add a VendoredFileSystem implementation

---

_Pull request opened by @AlexWaygood on 2024-06-13 19:16_

## Summary

Work towards https://github.com/astral-sh/ruff/issues/11653.

This PR adds a `VendoredFileSystem` implementation to the `ruff_db` crate, the idea being that this is the filesystem we would use for resolving vendored typeshed stubs.

Some of this feels a bit awkward, but I'm not sure there's a better way. I ran into two problems while writing this PR:
1. The `ZipArchive` struct exposed by the `zip` crate takes `&mut self` for most of the methods that query the underlying archive for information about files or directories within the archive. This means that in order to query zipped data from trait method implementations that take `&self`, we end up having to clone the underlying archive. I think that's okay, as the `zip` docs say that it should be cheap to clone (at least right now), but it didn't fill me with joy.
2. The `camino` crate strips trailing slashes in a lot of its methods, as they're not semantically significant for most paths. However, for zip archives, trailing slashes are very significant: if a `stdlib/` directory exists inside the zip, `zip.by_name("stdlib")` will not resolve to that directory (only `zip.by_name("stdlib/")` will). I've attempted to abstract over this difference to other kinds of paths, so that the `VendoredFileSystem` is consistent with the other `FileSystem` implementations in this respect. But it was slightly tricky to do so.

## Test Plan

I added lots of parameterized tests.

---

_Label `red-knot` added by @AlexWaygood on 2024-06-13 19:16_

---

_Review requested from @carljm by @AlexWaygood on 2024-06-13 19:16_

---

_Review requested from @MichaReiser by @AlexWaygood on 2024-06-13 19:16_

---

_Converted to draft by @AlexWaygood on 2024-06-13 19:27_

---

_Marked ready for review by @AlexWaygood on 2024-06-13 20:03_

---

_Comment by @github-actions[bot] on 2024-06-13 20:24_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Review comment by @MichaReiser on `crates/ruff_db/src/file_system/vendored.rs`:46 on 2024-06-14 06:21_

We shouldn't implement `FileSystem` for vendored paths. I think that's also the part that @carljm felt some confusion around. There are a few reasons:

* `FileSystem` is an abstraction over `std::fs`. It's not a way of implementing an overlay filesystem that supports reading files from different sources. That's that `Vfs` does
* `FileSystem` works on `FileSystemPath`, but we want to use `VendoredFilePath` and `VendoredFilePathBuf` for vendored file. We intentionally use different types to prevent that someone tries to read a `VendoredPath` from the file system or the other way round
* Many `FileSystem` operations are simply irrelevant for `VendoredPath`s. The only thing that we need to support for vendored path is testing if a file exists (or a simplified metadata that returns the last modified date), and reading the content. We won't need file watching, directory traversal, or operations for testing if something's a directory. We only care about the files, and nothing else

That's why I think we can either implement the methods as free-standing functions similar to `fs::read_to_string`, or as a struct, but where we define our own functions.

the vendored path because vendored path should be represented using `VendoredPath` and `VendoredPathBuf`. 

The other reason is that most operations aren't releve

---

_Review comment by @MichaReiser on `crates/ruff_db/src/file_system/vendored.rs`:150 on 2024-06-14 06:23_

I think we could combine this with the `VendoredFilePath` and `VendoredFilePathBuf` where we require the use of `VendoredFilePathBuf::new` (that normalizes the string). 

---

_Review comment by @MichaReiser on `crates/ruff_db/src/file_system/vendored.rs`:17 on 2024-06-14 06:24_

Can we use a `&'static [u8]` instead of an `Arc<[u8]>`
```suggestion
    pub fn new(raw_bytes: &'static [u8]) -> Result<Self> {
```

---

_Review comment by @MichaReiser on `crates/ruff_db/src/file_system/vendored.rs`:35 on 2024-06-14 06:28_

I'm not sure if I want to rely on this behavior. It feels very easy to get wrong when updating the crate. 

I think what I would do is to wrap the `ZipArchive` in a mutex instead and require explicitly locking. This will reduce concurrency a bit but I don't expect that any of the zip operations take long. 

---

_Review comment by @MichaReiser on `crates/ruff_db/src/file_system/vendored.rs`:39 on 2024-06-14 06:29_

Nit: This comment explains the implementation almost 1:1. I prefer comments that are either higher level or to omit the comment entirely, because it doesn't provide any additional information and is only at risk of being outdated.

---

_Review comment by @MichaReiser on `crates/ruff_db/src/file_system/vendored.rs`:122 on 2024-06-14 06:32_

Can't we re-use `archive` here?
```suggestion
```

---

_Review comment by @MichaReiser on `crates/ruff_db/src/file_system/vendored.rs`:119 on 2024-06-14 06:33_

What's the retry logic here? I get the impression that we do some trial and error here to read the file and that feels off. Can't we just return an error and ask callers to construct the right path instead of guessing paths?

---

_Review comment by @MichaReiser on `crates/ruff_db/src/file_system/vendored.rs`:42 on 2024-06-14 06:35_

I think we can just use `read_to_string` here. I haven't followed all the code but `zipfile.read_to_string` calls into `std::read_to_string` and I'm pretty sure they correctly resize the buffer before writing into it. 

---

_Review comment by @MichaReiser on `crates/ruff_db/src/file_system/vendored.rs`:39 on 2024-06-14 06:36_

Nit
```suggestion
    fn read_zipfile(mut zip_file: ZipFile) -> Result<String> {
```

---

_Review comment by @MichaReiser on `crates/ruff_db/src/file_system/vendored.rs`:106 on 2024-06-14 06:37_

We should use a revision other than zero or we might run into problems when the vendored file system content changes. We could use ruff's version, the last modified time of the files in the zip file, or the time when the `VendoredFileSystem` was created.

---

_Review comment by @MichaReiser on `crates/ruff_db/src/file_system/vendored.rs`:195 on 2024-06-14 06:39_

We have almost the same logic in `memory` rs 😆 

---

_Review comment by @MichaReiser on `crates/ruff_db/src/file_system/vendored.rs`:224 on 2024-06-14 06:40_

I don't think we need a file here. You can use a `Vec<u8>` as a target and just write into that. 

But what could be simpler (and closer to the actual code) is to create a zip file manually  and commit it. You can then use `include_bytes!` to include the bytes. 

This also avoids creating a zip file on every test run.

---

_Review comment by @MichaReiser on `crates/ruff_db/src/file_system/vendored.rs`:281 on 2024-06-14 06:43_

I think we don't need to use macros here. They safe very little code but break IDE features. For example, I can't jump or run the test manually anymore. Rustfmt breaks down, and auto completion often doesn't work. They're also hard to read. 

```suggestion
    fn assert_directory(directory_name: &str) {
                let mock_typeshed = mock_typeshed();

                let path = FileSystemPath::new(directory_name);

                assert!(mock_typeshed.exists(path));
                assert!(mock_typeshed.is_directory(path));
                assert!(!mock_typeshed.is_file(path));

                let path_metadata = mock_typeshed.metadata(path).unwrap();
                assert!(path_metadata.file_type().is_directory());
                assert!(path_metadata.permissions().is_some());
                assert_eq!(path_metadata.revision(), FileRevision::zero());
    }
    
    #[test]
    fn stdlib_dir_no_trailing_slash() {
      assert_directory("stdlib");
    }
```

Same for the other tests

---

_@MichaReiser requested changes on 2024-06-14 06:47_

Nice work. 

Overall looks good, but we should address the following items before merging:

* Use regular functions for tests instead of macros
* Remove `FileSystem` implementation and use `VendoredFilePath` and `VendoredFilePathBuf` (and move the definitions into `vendored.rs)
* Move the implementation out of the `vendored` directory.
* If we can, remove the probing where we append a slash at the end of the path when reading failed. This seems like trial and error. 

One open question for me is whether we should create a dedicated `ruff_vendored` crate that also contains all vendored files (and contains the `build.rs`). The benefit of that would be that integration tests can use the actual vendored files, which I think would be desired (but use stubed vendored files for unit tests). If that's undesired, I think leaving it in `ruff_db` is fine.

---

_@AlexWaygood reviewed on 2024-06-14 07:02_

---

_Review comment by @AlexWaygood on `crates/ruff_db/src/file_system/vendored.rs`:17 on 2024-06-14 07:02_

I had that initially, but it made it much harder to test it using a mock zipfile

---

_@AlexWaygood reviewed on 2024-06-14 07:04_

---

_Review comment by @AlexWaygood on `crates/ruff_db/src/file_system/vendored.rs`:42 on 2024-06-14 07:04_

You have to pass in a mutable reference to a pre-existing buffer for the zip crate's `read_to_string` method

---

_@AlexWaygood reviewed on 2024-06-14 07:06_

---

_Review comment by @AlexWaygood on `crates/ruff_db/src/file_system/vendored.rs`:106 on 2024-06-14 07:06_

I thought one of the guarantees of the vendored file system was that the files could never change within a single invocation of Ruff. Or is the concern here that this would lead to incorrect data being loaded from the persistent on-disk cache on a subsequent run of Ruff?

---

_@AlexWaygood reviewed on 2024-06-14 07:08_

---

_Review comment by @AlexWaygood on `crates/ruff_db/src/file_system/vendored.rs`:224 on 2024-06-14 07:08_

> But what could be simpler (and closer to the actual code) is to create a zip file manually and commit it. You can then use `include_bytes!` to include the bytes.
> 
> This also avoids creating a zip file on every test run.

I considered this. But I thought it made for a much more readable test if you could see exactly which files are being put in the mocked zip as part of the test setup. Otherwise it feels unclear what exactly we're testing.

---

_@AlexWaygood reviewed on 2024-06-14 07:11_

---

_Review comment by @AlexWaygood on `crates/ruff_db/src/file_system/vendored.rs`:281 on 2024-06-14 07:11_

Duh. I felt so clever about this... How did I forget about this much simpler solution 😑

(though FWIW, it didn't seem to break any IDE features for me on VSCode — I was still able to run the test manually locally) 

---

_Review comment by @AlexWaygood on `crates/ruff_db/src/file_system/vendored.rs`:122 on 2024-06-14 07:12_

No, or the borrow checker complains that we're mutably borrowing it twice in the same scope.

---

_@AlexWaygood reviewed on 2024-06-14 07:12_

---

_Comment by @AlexWaygood on 2024-06-14 07:13_

> * Move the implementation out of the `vendored` directory.

I'm guessing you mean `file_system` directory? :)

---

_@AlexWaygood reviewed on 2024-06-14 07:22_

---

_Review comment by @AlexWaygood on `crates/ruff_db/src/file_system/vendored.rs`:17 on 2024-06-14 07:22_

This relates to the conversation we're having at https://github.com/astral-sh/ruff/pull/11863#discussion_r1639342264. If we just committed a binary zip to the repo and used that in the test, then we could easily use `&'static` for the implementation. But IMHO that would be a really bad test, because we wouldn't be able to easily see what we'd put in the zip archive, so I feel like any assertions we'd make would honestly not have that much value

---

_@AlexWaygood reviewed on 2024-06-14 07:27_

---

_Review comment by @AlexWaygood on `crates/ruff_db/src/file_system/vendored.rs`:195 on 2024-06-14 07:27_

Not a coincidence 😉 I looked at that function quite a bit while working on this, but in the end I concluded that there was enough that needed to be different that it would require a separate function.

---

_@MichaReiser reviewed on 2024-06-14 07:43_

---

_Review comment by @MichaReiser on `crates/ruff_db/src/file_system/vendored.rs`:122 on 2024-06-14 07:43_

This seems to compile fine

```rust
let normalized = normalize_vendored_path(path);
        let mut archive = self.vendored_archive();
        let lookup_error = match archive.lookup_path(&normalized) {
            Ok(zipfile) => return Self::read_zipfile(zipfile),
            Err(err) => err,
        };
        if normalized.has_trailing_slash() {
            return Err(lookup_error);
        }
        // let mut archive = self.vendored_archive();
        let zipfile = archive.lookup_path(&normalized.with_trailing_slash())?;
        Self::read_zipfile(zipfile)
```

---

_@MichaReiser reviewed on 2024-06-14 07:45_

---

_Review comment by @MichaReiser on `crates/ruff_db/src/file_system/vendored.rs`:42 on 2024-06-14 07:45_

Sorry. What I meant is we can simplify this to 

```rust
let mut buffer = String::new();
zipfile.read_to_string(&buffer)?;
Ok(buffer)
```

Because `read_to_string` already handles the `with_capacity` (it should call `reserve` or similar)

---

_@MichaReiser reviewed on 2024-06-14 07:46_

---

_Review comment by @MichaReiser on `crates/ruff_db/src/file_system/vendored.rs`:106 on 2024-06-14 07:46_

It should probably be fine without it, because we should bust the entire cache if the ruff version changes. But having it in the revision gives us an extra layer of protection, because salsa would then invalidate the queries for us, in case we forget.

---

_@MichaReiser reviewed on 2024-06-14 07:51_

---

_Review comment by @MichaReiser on `crates/ruff_db/src/file_system/vendored.rs`:17 on 2024-06-14 07:51_

That's fair. I think we should allow both then. Using `&'static [u8]` has the benefit that we can pass the bytes directly from the binary without having to copy the zip to the heap first (it's already on the heap because the binary is on the heap, but we can avoid duplicating it). 

You can implement it by having a 

```rust
enum ZipData {
	Static(&'static [u8]),
	Dynamic(Arc<[u8]) // Or Box if we can drop the `Clone` requirement
}
```

And then provide a  `new_static` and `new` function on the `VendoredFilesystem`

---

_@AlexWaygood reviewed on 2024-06-14 07:56_

---

_Review comment by @AlexWaygood on `crates/ruff_db/src/file_system/vendored.rs`:42 on 2024-06-14 07:56_

Got it, thanks!

---

_@MichaReiser reviewed on 2024-06-14 07:59_

---

_Review comment by @MichaReiser on `crates/ruff_db/src/file_system/vendored.rs`:224 on 2024-06-14 07:59_

That makes sense. But we should replace `tempfile` with a `&mut Vec<u8>` 

---

_@AlexWaygood reviewed on 2024-06-14 07:59_

---

_Review comment by @AlexWaygood on `crates/ruff_db/src/file_system/vendored.rs`:17 on 2024-06-14 07:59_

Brilliant, thank you!

---

_@AlexWaygood reviewed on 2024-06-14 08:02_

---

_Review comment by @AlexWaygood on `crates/ruff_db/src/file_system/vendored.rs`:224 on 2024-06-14 08:02_

Yeah, I'll make that change for sure!

---

_@AlexWaygood reviewed on 2024-06-14 10:07_

---

_Review comment by @AlexWaygood on `crates/ruff_db/src/file_system/vendored.rs`:122 on 2024-06-14 10:07_

So it does. I'm sure I tried that yesterday. Ah well -- thanks!

---

_@AlexWaygood reviewed on 2024-06-14 10:16_

---

_Review comment by @AlexWaygood on `crates/ruff_db/src/file_system/vendored.rs`:119 on 2024-06-14 10:16_

This is point (2) that I described in the PR description. For other file systems we use in Ruff, `file_system.exists("stdlib")` will return `true` if there exists a directory called `stdlib`. But without this retry logic, that will return `false` for the vendored filesystem, because zip archives use trailing slashes internally to distinguish between files and directories in the archive. So if there's a `stdlib` directory, then (without this retry logic) `file_system.exists("stdlib/")` would return `true`, but `file_system.exists("stdlib")` would return `false`.

I wanted to try to abstract over that difference in my implementation here, as I think it would be a bug magnet for paths to work differently for the vendored file system compared to how they work with other file systems we're using elsewhere in Ruff.

---

_Comment by @AlexWaygood on 2024-06-14 11:13_

> One open question for me is whether we should create a dedicated `ruff_vendored` crate that also contains all vendored files (and contains the `build.rs`). The benefit of that would be that integration tests can use the actual vendored files, which I think would be desired (but use stubed vendored files for unit tests). If that's undesired, I think leaving it in `ruff_db` is fine.

I wasn't sure while writing this the extent to which we'd want to tie the `VendoredFileSystem` implementation to the exact details of the only repository we realistically expect to be vendoring (typeshed). In this implementation, I tried to keep things relatively abstract, so that it could theoretically work with any vendored zip archive. That means that I expect to have to build another layer of abstraction on top of this for working with typeshed specifically, because we have to take the `VERSIONS` file into account when deciding whether a file "exists" in typeshed. If a file `stdlib/foo.pyi` exists in the typeshed zip archive but `VERSIONS` tells us that it hadn't actually been added to the stdlib yet on the Python version the user has selected to be the target version, then the query to our typeshed abstraction should arguably report that the `stdlib/foo.pyi` doesn't exist at all. That's the reality at runtime on that Python version, after all: if the module hasn't been added to the stdlib yet, the file for that module physically doesn't exist at runtime on that Python version.

---

_Comment by @codspeed-hq[bot] on 2024-06-14 17:31_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/vendored-filesystem)

### Merging #11863 will **not alter performance**

<sub>Comparing <code>vendored-filesystem</code> (3127ff6) with <code>vendored-filesystem</code> (5d511bb)</sub>



### Summary

`✅ 30` untouched benchmarks






---

_@AlexWaygood reviewed on 2024-06-15 10:58_

---

_Review comment by @AlexWaygood on `crates/ruff_db/src/file_system/vendored.rs`:119 on 2024-06-15 10:58_

...but this doesn't make any sense, because you can't read a directory, you can only read a file, and this is the `read()` method 🤦‍♂️

---

_@AlexWaygood reviewed on 2024-06-15 13:59_

---

_Review comment by @AlexWaygood on `crates/ruff_db/src/file_system/vendored.rs`:35 on 2024-06-15 13:59_

This is the problem I described as point (1) in my PR description. Because `ZipArchive::by_name` takes `&mut self`, it's very hard to implement `VendoredFileSystem::read()` (which must take `&self`). I think our options are as follows:
1. Creating a new `ZipArchive` instance every time `read()` is called
2. Using the existing `ZipArchive` instance every time `read()` is called, but cloning it to create an owned copy
3.  Find a solution for reading from the zip at runtime that doesn't use the `zip` crate, and doesn't require mutable references in order to read files from the zip archive
5. Accept that `VendoredFileSystem::read()` will need to take `&mut self` rather than `&self` (I don't like this at all).
6. Unzip typeshed eagerly at runtime so that we don't need to use the `zip` crate at all to read vendored files.
7. something something interior mutability

Currently I'm doing Option (2) in this PR, but maybe I'm missing something.

---

_Review comment by @MichaReiser on `crates/ruff_db/src/file_system.rs`:10 on 2024-06-15 15:12_

I suggest moving `vendored` into its own module, considering that it isn't implementing `FileSystem`.

---

_@MichaReiser reviewed on 2024-06-15 15:12_

---

_@AlexWaygood reviewed on 2024-06-15 15:13_

---

_Review comment by @AlexWaygood on `crates/ruff_db/src/file_system.rs`:10 on 2024-06-15 15:13_

Yes, I'm going to make that change. It still implements `FileSystem` in this PR currently, but I'm working on changing that.

---

_@AlexWaygood reviewed on 2024-06-16 12:45_

---

_Review comment by @AlexWaygood on `crates/ruff_db/src/file_system/vendored.rs`:35 on 2024-06-16 12:45_

I switched the PR to use Option (6): "Something something interior mutability"

---

_Comment by @AlexWaygood on 2024-06-17 10:19_

@MichaReiser -- I think I've addressed all your review comments now except for https://github.com/astral-sh/ruff/pull/11863#discussion_r1639324437 (where we want to do the path normalization). As I mentioned on our call earlier, I think I'd prefer to experiment with that in a followup, if that's okay, and see if we think it improves the API or not. So I think this PR should be ready for re-review now!

---

_Review requested from @MichaReiser by @AlexWaygood on 2024-06-17 10:19_

---

_Review comment by @AlexWaygood on `crates/ruff_db/src/vendored.rs`:29 on 2024-06-17 10:21_

Style question: do you prefer an isolated scope like I currently have, or a call to `drop()`, for this kind of pattern? Not sure which is considered more idiomatic.

```suggestion
            let mut archive = ZipWriter::new(cursor);
            archive.finish().unwrap();
            drop(archive);
```

---

_@AlexWaygood reviewed on 2024-06-17 10:21_

---

_@MichaReiser reviewed on 2024-06-17 11:45_

---

_Review comment by @MichaReiser on `crates/ruff_db/src/vendored.rs`:29 on 2024-06-17 11:45_

I prefer the sub scopes. 

---

_Review comment by @MichaReiser on `crates/ruff_db/src/vendored.rs`:22 on 2024-06-17 11:54_

Nit: I would remove the inner file type. It doesn't provide that much functionality but the nesting becomes somewhat hard to understand. It also allows to remove other wrapper types, reducing the overall code.

---

_Review comment by @MichaReiser on `crates/ruff_db/src/vendored.rs`:97 on 2024-06-17 11:55_

I prefer manual `is_*` implementations for enums with just a few variants. I can see how writing `is_` methods for enums with a lot of variants is a pain, but I think just two variants doesn't justify adding a dependency

---

_Review comment by @MichaReiser on `crates/ruff_db/src/vendored.rs`:60 on 2024-06-17 11:56_

It's unclear to me how you plan on using the API but do we need a `file_type` method? From what I understand, the only operations that we perform are on files.

---

_Review comment by @MichaReiser on `crates/ruff_db/src/vendored.rs`:84 on 2024-06-17 11:57_

I think the only two APIs that we need for implementing module resolution is 

* `read`
* `metadata` where metadata returns the CRC32 hash and is `None` (or an error) if the file doesn't exist. 

Combining `exists`, `metadata`, and `file_type` has the advantage that we don't need to locate the file's that often (and fewer locking overall)

Reducing `VendoredFs` to only support files (and documenting it in the struct) means we can remove the entire `with_trailing_slash` fallback logic, because as far as I understand this is only necessary for directories?

---

_Review comment by @MichaReiser on `crates/ruff_db/src/vfs.rs`:329 on 2024-06-17 12:04_

Nit: Maybe rename to `vendored` to avoid confusion with `FileSystem`
```suggestion
            VendoredVfs::Real(vendored) => vendored.read(path),
```

---

_Review comment by @MichaReiser on `crates/ruff_db/src/vendored.rs`:21 on 2024-06-17 12:05_

Nit: I would just call this `Vendored` or `VendoredFiles` to avoid any confusion with `FileSystem`. 

---

_Review comment by @MichaReiser on `crates/ruff_db/src/vendored.rs`:92 on 2024-06-17 12:07_

What are the reasons why `read_to_string` can fail? I'm not sure if just silently returning `None` in that case is the best solution. Because the file does exist, it just can't be read for obscure reasons. We may want to change this method to return a result instead.

---

_Review comment by @MichaReiser on `crates/ruff_db/src/vendored.rs`:157 on 2024-06-17 12:08_

```suggestion
    fn new(data: &'static [u8]) -> Result<Self> {
```

---

_Review comment by @MichaReiser on `crates/ruff_db/src/vendored.rs`:29 on 2024-06-17 12:10_

What's the use case for implementing `Default`? I think  I strongly prefer not to implement `Default` for this type because it just isn't very useful when empty. 

If you want to support an empty `VendoredFS`, then I think I would model it as an enum with `EMPTY` and `Data(&'static [u8]`) variants. 

---

_Review comment by @MichaReiser on `crates/ruff_db/src/vendored.rs`:197 on 2024-06-17 12:15_

We don't want to use `is_absolute`. It's platform dependent, meaning `VendoredPath`'s suddenly become platform specific. 

If you want to use linux semantics, use `!path.starts_with("/")`

---

_Review comment by @MichaReiser on `crates/ruff_db/src/vendored.rs`:163 on 2024-06-17 12:17_

Should `lookup_path` take a `&VendoredPath` instead and do all the normalization? 

---

_@MichaReiser reviewed on 2024-06-17 12:19_

This overall looks good but I think we can make it much simpler by removing all code that's only necessary for handling directories. From what I understand, we won't need directory support to implement module resolution. All we need is to lookup a specific file path. Or are there operations that need to know about the existence of a directory?

---

_@AlexWaygood reviewed on 2024-06-17 12:21_

---

_Review comment by @AlexWaygood on `crates/ruff_db/src/vendored.rs`:60 on 2024-06-17 12:21_

Yes, I wasn't sure if this was necessary or not, so I think you're right that it probably makes sense to remove it for now until there's a need for it.

---

_@AlexWaygood reviewed on 2024-06-17 12:27_

---

_Review comment by @AlexWaygood on `crates/ruff_db/src/vendored.rs`:29 on 2024-06-17 12:27_

I implemented `Default` here because you'd implemented `Default` on the `VendoredVfs` enum, and I assumed you had a reason for that 😆

I don't see a great reason for implementing `Default` on this struct; I can get rid of it.

---

_@AlexWaygood reviewed on 2024-06-17 12:27_

---

_Review comment by @AlexWaygood on `crates/ruff_db/src/vendored.rs`:163 on 2024-06-17 12:27_

That's a nice idea.

---

_@AlexWaygood reviewed on 2024-06-17 12:28_

---

_Review comment by @AlexWaygood on `crates/ruff_db/src/vfs.rs`:329 on 2024-06-17 12:28_

It _is_ a file system though, it just isn't an _OS_ file system :) that's what the `fs` in `Vfs` stands for, right?

I don't think `vendored` as a name works well here, because the distinction between the two enum branches is whether it's a "Real file system" or a "Stubbed file system", not whether it's a "Real vendored" or a "Stubbed vendored" (that doesn't really make sense).

I would prefer it if we considered renaming the `FileSystem` trait instead

---

_Review comment by @MichaReiser on `crates/ruff_db/src/vfs.rs`:329 on 2024-06-17 12:43_

Yeah not sure. The `FileSystem` maps to `std::fs` and vendored is not that. I don't think `FileSystem` maps to the `OS` file system, because an in memory (or wasm FS) isn't the OS's file system.

I'm currently thinking about renaming `VfsFile` to `Files`, `VfsFile` to `File` (not sure about `VfsPath`). `VendoredFiles` would then fit well into this naming and would also make it clear, that it only supports files. But I'm okay deferring this for now.

---

_@MichaReiser reviewed on 2024-06-17 12:43_

---

_@AlexWaygood reviewed on 2024-06-17 12:46_

---

_Review comment by @AlexWaygood on `crates/ruff_db/src/vendored.rs`:197 on 2024-06-17 12:46_

It's very surprising to me that `is_absolute` is platform specific. Thanks for the reminder!

---

_@AlexWaygood reviewed on 2024-06-17 12:51_

---

_Review comment by @AlexWaygood on `crates/ruff_db/src/vendored.rs`:197 on 2024-06-17 12:51_

Oh wait, no, you said "platform-dependent", not "platforms-specific". I think "platform-dependent" is what I want here. Whatever kind of path we're dealing with here, it should never have been resolved to an absolute OS-filesystem path. It should always be a path that's relative to the root of the zip directory. If this assertion ever fails on either Linux or Windows, I think that indicates a logical error in our code somewhere.

---

_@AlexWaygood reviewed on 2024-06-17 12:56_

---

_Review comment by @AlexWaygood on `crates/ruff_db/src/vendored.rs`:92 on 2024-06-17 12:56_

It returns an `Err` if the data that it is trying to read into the `String` buffer turns out not to be valid UTF-8. I agree that we shouldn't silently return `None` here if that happens.

---

_@MichaReiser reviewed on 2024-06-17 13:04_

---

_Review comment by @MichaReiser on `crates/ruff_db/src/vendored.rs`:197 on 2024-06-17 13:04_

A path starting with `/` will fail on Windows. 

---

_@AlexWaygood reviewed on 2024-06-17 13:31_

---

_Review comment by @AlexWaygood on `crates/ruff_db/src/vendored.rs`:22 on 2024-06-17 13:31_

I got rid of one of the other wrapper types in https://github.com/astral-sh/ruff/pull/11863/commits/f4f97d4a2c183ed1777d6b610fa770f226e073f7, but I think I would prefer to keep using an inner file type for this. I tried changing it to a type alias, and in my opinion it made the `VendoredFileSystem` unduly coupled to the inner representation that used a lock/RefCell; I also found it less readable.

---

_@AlexWaygood reviewed on 2024-06-17 14:01_

---

_Review comment by @AlexWaygood on `crates/ruff_db/src/vendored.rs`:197 on 2024-06-17 14:01_

I'm still not sure I fully understand. The main purpose of this debug assertion is to make sure that the path hasn't been `canonicalize`d or otherwise "made absolute" prior to attempting to resolve the path relative to the zip archive. On a Unix system `canonicalize()` would mean that the path ould start with `/`, but on a Windows system it would mean that it would start with a prefix, right? So I think `is_absolute()` is what we want here?

---

_@AlexWaygood reviewed on 2024-06-17 15:45_

---

_Review comment by @AlexWaygood on `crates/ruff_db/src/vendored.rs`:84 on 2024-06-17 15:45_

(As discussed offline, we probably are going to need more than these two APIs, since you can have subpackages and namespace packages inside the stdlib just like in any other Python directories)

---

_@AlexWaygood reviewed on 2024-06-17 16:13_

---

_Review comment by @AlexWaygood on `crates/ruff_db/src/vendored.rs`:163 on 2024-06-17 16:13_

Because of the "trailing slashes are semantically significant for zip archives" thing combined with the "`ZipFile::by_name()` requires a mutable reference" thing, it's hard for me to see how we could do this while keeping the borrow checker happy and avoiding doing path normalization twice. It doesn't seem possible to call `self.0.by_name()` twice in `lookup_path()` while keeping the borrow checker happy, unfortunately (unless I'm missing something), and because of the trailing-slash thing, we need to call it twice in `lookup_path()` if we're going to do the normalization inside `lookup_path()`.

---

_Review requested from @MichaReiser by @AlexWaygood on 2024-06-17 16:32_

---

_@MichaReiser reviewed on 2024-06-17 16:54_

---

_Review comment by @MichaReiser on `crates/ruff_db/src/vendored.rs`:197 on 2024-06-17 16:54_

I think we want `VendoredPath` to be file system agnostic. It's not a real file system so we should avoid the madness of having different path representations for different systems. I'm also not sure what happens if you lookup the path `C:\test` in the zip file. I would simply assume that this would outright fail?

I recommend that `VendoredPath` uses unix representation exclusively. 

---

_@AlexWaygood reviewed on 2024-06-17 16:57_

---

_Review comment by @AlexWaygood on `crates/ruff_db/src/vendored.rs`:197 on 2024-06-17 16:57_

`VendoredPath` does use unix representation exclusively; this PR doesn't change that, and nor does this assertion. I think my takeaway here is that this debug assertion causes more confusion than clarification, so I think I'll just remove it altogether :)

---

_@MichaReiser reviewed on 2024-06-17 17:14_

---

_Review comment by @MichaReiser on `crates/ruff_db/src/vendored.rs`:197 on 2024-06-17 17:14_

The debug assertion itself is fine, but it can't be `is_absolute` because `is_absolute` returns `false` on Windows if the path starts with `/`

---

_Review comment by @AlexWaygood on `crates/ruff_db/src/vendored.rs`:197 on 2024-06-17 17:20_

> `is_absolute` returns `false` on Windows if the path starts with `/`

I understand that. I still think we're talking past each other a little bit here, but I've got rid of the assertion now anyway. I don't think it was _that_ valuable in the first place.

---

_@AlexWaygood reviewed on 2024-06-17 17:20_

---

_Review requested from @dhruvmanila by @MichaReiser on 2024-06-18 12:05_

---

_Review request for @dhruvmanila removed by @AlexWaygood on 2024-06-18 12:09_

---

_Review comment by @MichaReiser on `crates/ruff_db/src/metadata.rs`:1 on 2024-06-18 13:53_

Maybe rename the module to `file_revision`, considering that it doesn't contain anything else.

---

_Review comment by @MichaReiser on `crates/ruff_db/src/vendored.rs`:48 on 2024-06-18 13:55_

I think some empty lines could help with readability:

* setup
* handle possible file
* handle possible directory

```suggestion
    pub fn metadata(&self, path: &VendoredPath) -> Option<Metadata> {
        let normalized = normalize_vendored_path(path);
        let inner_locked = self.inner.lock();
        let mut archive = inner_locked.borrow_mut();

        if let Ok(zip_file) = archive.lookup_path(&normalized) {
            return Some(Metadata::from_zip_file(zip_file));
        }
        
        if let Ok(zip_file) = archive.lookup_path(&normalized.with_trailing_slash()) {
            return Some(Metadata::from_zip_file(zip_file));
        }
        None
    }
```

---

_Review comment by @MichaReiser on `crates/ruff_db/src/vendored.rs`:21 on 2024-06-18 13:56_

It might be helpful to add some documentation to this struct, even if it's very brief

---

_Review comment by @MichaReiser on `crates/ruff_db/src/vendored.rs`:44 on 2024-06-18 13:57_

I would find some documentation helpful that explains the difference between paths ending with trailing slash and once that don't. Because it isn't clear from reading this code what's happening here and why probing the zip two times is necesary.

---

_Review comment by @MichaReiser on `crates/ruff_db/src/vendored.rs`:34 on 2024-06-18 13:57_

Nit: Maybe just `self.metadata(path).is_some()`

---

_@MichaReiser approved on 2024-06-18 14:00_

---

_@AlexWaygood reviewed on 2024-06-18 14:02_

---

_Review comment by @AlexWaygood on `crates/ruff_db/src/vendored.rs`:34 on 2024-06-18 14:02_

I couldn't resist micro-optimising this 😄 (this avoids calling `zip_file.crc32()` and `zip_file.is_dir()`, which aren't necessary if you only need to know whether the path exists or not)

---

_Merged by @AlexWaygood on 2024-06-18 15:43_

---

_Closed by @AlexWaygood on 2024-06-18 15:43_

---

_Branch deleted on 2024-06-18 15:43_

---
