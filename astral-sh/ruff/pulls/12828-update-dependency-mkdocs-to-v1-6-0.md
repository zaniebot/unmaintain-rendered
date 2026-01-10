```yaml
number: 12828
title: Update dependency mkdocs to v1.6.0
type: pull_request
state: merged
author: renovate
labels:
  - internal
assignees: []
merged: true
base: main
head: renovate/mkdocs-1.x
created_at: 2024-08-12T02:02:51Z
updated_at: 2024-08-12T06:19:51Z
url: https://github.com/astral-sh/ruff/pull/12828
synced_at: 2026-01-10T21:38:32Z
```

# Update dependency mkdocs to v1.6.0

---

_Pull request opened by @renovate on 2024-08-12 02:02_

[![Mend Renovate](https://app.renovatebot.com/images/banner.svg)](https://renovatebot.com)

This PR contains the following updates:

| Package | Change | Age | Adoption | Passing | Confidence |
|---|---|---|---|---|---|
| [mkdocs](https://togithub.com/mkdocs/mkdocs) ([changelog](https://www.mkdocs.org/about/release-notes/)) | `==1.5.0` -> `==1.6.0` | [![age](https://developer.mend.io/api/mc/badges/age/pypi/mkdocs/1.6.0?slim=true)](https://docs.renovatebot.com/merge-confidence/) | [![adoption](https://developer.mend.io/api/mc/badges/adoption/pypi/mkdocs/1.6.0?slim=true)](https://docs.renovatebot.com/merge-confidence/) | [![passing](https://developer.mend.io/api/mc/badges/compatibility/pypi/mkdocs/1.5.0/1.6.0?slim=true)](https://docs.renovatebot.com/merge-confidence/) | [![confidence](https://developer.mend.io/api/mc/badges/confidence/pypi/mkdocs/1.5.0/1.6.0?slim=true)](https://docs.renovatebot.com/merge-confidence/) |

---

### Release Notes

<details>
<summary>mkdocs/mkdocs (mkdocs)</summary>

### [`v1.6.0`](https://togithub.com/mkdocs/mkdocs/releases/tag/1.6.0)

[Compare Source](https://togithub.com/mkdocs/mkdocs/compare/1.5.3...1.6.0)

#### Local preview

-   `mkdocs serve` no longer locks up the browser when more than 5 tabs are open. This is achieved by closing the polling connection whenever a tab becomes inactive. Background tabs will no longer auto-reload either - that will instead happen as soon the tab is opened again. Context: [#&#8203;3391](https://togithub.com/mkdocs/mkdocs/issues/3391)

-   New flag `serve --open` to open the site in a browser.\
    After the first build is finished, this flag will cause the default OS Web browser to be opened at the home page of the local site.\
    Context: [#&#8203;3500](https://togithub.com/mkdocs/mkdocs/issues/3500)

##### Drafts

> \[!warning]
> **Changed from version 1.5:**
>
> **The `exclude_docs` config was split up into two separate concepts.**

The `exclude_docs` config no longer has any special behavior for `mkdocs serve` - it now always completely excludes the listed documents from the site.

If you wish to use the "drafts" functionality like the `exclude_docs` key used to do in MkDocs 1.5, please switch to the **new config key `draft_docs`**.

See [documentation](https://www.mkdocs.org/user-guide/configuration/#exclude_docs).

Other changes:

-   Reduce warning levels when a "draft" page has a link to a non-existent file. Context: [#&#8203;3449](https://togithub.com/mkdocs/mkdocs/issues/3449)

#### Update to deduction of page titles

MkDocs 1.5 had a change in behavior in deducing the page titles from the first heading. Unfortunately this could cause unescaped HTML tags or entities to appear in edge cases.

Now tags are always fully sanitized from the title. Though it still remains the case that [`Page.title`](https://www.mkdocs.org/dev-guide/api/#mkdocs.structure.files.pages.Page.title) is expected to contain HTML entities and is passed directly to the themes.

Images (notably, emojis in some extensions) get preserved in the title only through their `alt` attribute's value.

Context: [#&#8203;3564](https://togithub.com/mkdocs/mkdocs/issues/3564), [#&#8203;3578](https://togithub.com/mkdocs/mkdocs/issues/3578)

#### Themes

-   Built-in themes now also support Polish language ([#&#8203;3613](https://togithub.com/mkdocs/mkdocs/issues/3613))

##### "readthedocs" theme

-   Fix: "readthedocs" theme can now correctly handle deeply nested nav configurations (over 2 levels deep), without confusedly expanding all sections and jumping around vertically. ([#&#8203;3464](https://togithub.com/mkdocs/mkdocs/issues/3464))

-   Fix: "readthedocs" theme now shows a link to the repository (with a generic logo) even when isn't one of the 3 known hosters. ([#&#8203;3435](https://togithub.com/mkdocs/mkdocs/issues/3435))

-   "readthedocs" theme now also has translation for the word "theme" in the footer that mistakenly always remained in English. ([#&#8203;3613](https://togithub.com/mkdocs/mkdocs/issues/3613), [#&#8203;3625](https://togithub.com/mkdocs/mkdocs/issues/3625))

##### "mkdocs" theme

The "mkdocs" theme got a big update to a newer version of Bootstrap, meaning a slight overhaul of styles. Colors (most notably of admonitions) have much better contrast.

The "mkdocs" theme now has support for dark mode - both automatic (based on the OS/browser setting) and with a manual toggle. Both of these options are **not** enabled by default and need to be configured explicitly.\
See `color_mode`, `user_color_mode_toggle` in [**documentation**](https://www.mkdocs.org/user-guide/choosing-your-theme/#mkdocs).

> \[!warning]
> **Possible breaking change:**
>
> jQuery is no longer included into the "mkdocs" theme. If you were relying on it in your scripts, you will need to separately add it first (into mkdocs.yml) as an extra script:
>
> ```yaml
> extra_javascript:
>   - https://code.jquery.com/jquery-3.7.1.min.js
> ```
>
> Or even better if the script file is copied and included from your docs dir.

Context: [#&#8203;3493](https://togithub.com/mkdocs/mkdocs/issues/3493), [#&#8203;3649](https://togithub.com/mkdocs/mkdocs/issues/3649)

#### Configuration

##### New "`enabled`" setting for all plugins

You may have seen some plugins take up the convention of having a setting `enabled: false` (or usually controlled through an environment variable) to make the plugin do nothing.

Now *every* plugin has this setting. Plugins can still *choose* to implement this config themselves and decide how it behaves (and unless they drop older versions of MkDocs, they still should for now), but now there's always a fallback for every plugin.

See [**documentation**](https://www.mkdocs.org/user-guide/configuration/#enabled-option). Context: [#&#8203;3395](https://togithub.com/mkdocs/mkdocs/issues/3395)

#### Validation

##### Validation of hyperlinks between pages

##### Absolute links

> Historically, within Markdown, MkDocs only recognized **relative** links that lead to another physical `*.md` document (or media file). This is a good convention to follow because then the source pages are also freely browsable without MkDocs, for example on GitHub. Whereas absolute links were left unmodified (making them often not work as expected or, more recently, warned against).

If you dislike having to always use relative links, now you can opt into absolute links and have them work correctly.

If you set the setting `validation.links.absolute_links` to the new value `relative_to_docs`, all Markdown links starting with `/` will be understood as being relative to the `docs_dir` root. The links will then be validated for correctness according to all the other rules that were already working for relative links in prior versions of MkDocs. For the HTML output, these links will still be turned relative so that the site still works reliably.

So, now any document (e.g. "dir1/foo.md") can link to the document "dir2/bar.md" as `[link](/dir2/bar.md)`, in addition to the previously only correct way `[link](../dir2/bar.md)`.

You have to enable the setting, though. The default is still to just skip any processing of such links.

See [**documentation**](https://www.mkdocs.org/user-guide/configuration/#validation-of-absolute-links). Context: [#&#8203;3485](https://togithub.com/mkdocs/mkdocs/issues/3485)

##### Absolute links within nav

Absolute links within the `nav:` config were also always skipped. It is now possible to also validate them in the same way with `validation.nav.absolute_links`. Though it makes a bit less sense because then the syntax is simply redundant with the syntax that comes without the leading slash.

##### Anchors

There is a new config setting that is recommended to enable warnings for:

```yaml
validation:
  anchors: warn
```

Example of a warning that this can produce:

```text
WARNING -  Doc file 'foo/example.md' contains a link '../bar.md#some-heading', but the doc 'foo/bar.md' does not contain an anchor '#some-heading'.
```

Any of the below methods of declaring an anchor will be detected by MkDocs:

```markdown

#### Heading producing an anchor
#### Another heading {#custom-anchor-for-heading-using-attr-list}

<a id="raw-anchor"></a>

[](){#markdown-anchor-using-attr-list}
```

Plugins and extensions that insert anchors, in order to be compatible with this, need to be developed as treeprocessors that insert `etree` elements as their mode of operation, rather than raw HTML which is undetectable for this purpose.

If you as a user are dealing with falsely reported missing anchors and there's no way to resolve this, you can choose to disable these messages by setting this option to `ignore` (and they are at INFO level by default anyway).

See [**documentation**](https://www.mkdocs.org/user-guide/configuration/#validation). Context: [#&#8203;3463](https://togithub.com/mkdocs/mkdocs/issues/3463)

Other changes:

-   When the `nav` config is not specified at all, the `not_in_nav` setting (originally added in 1.5.0) gains an additional behavior: documents covered by `not_in_nav` will not be part of the automatically deduced navigation. Context: [#&#8203;3443](https://togithub.com/mkdocs/mkdocs/issues/3443)

-   Fix: the `!relative` YAML tag for `markdown_extensions` (originally added in 1.5.0) - it was broken in many typical use cases.

    See [**documentation**](https://www.mkdocs.org/user-guide/configuration/#paths-relative-to-the-current-file-or-site). Context: [#&#8203;3466](https://togithub.com/mkdocs/mkdocs/issues/3466)

-   Config validation now exits on first error, to avoid showing bizarre secondary errors. Context: [#&#8203;3437](https://togithub.com/mkdocs/mkdocs/issues/3437)

-   MkDocs used to shorten error messages for unexpected errors such as "file not found", but that is no longer the case, the full error message and stack trace will be possible to see (unless the error has a proper handler, of course). Context: [#&#8203;3445](https://togithub.com/mkdocs/mkdocs/issues/3445)

#### Upgrades for plugin developers

##### Plugins can add multiple handlers for the same event type, at multiple priorities

See [`mkdocs.plugins.CombinedEvent`](https://www.mkdocs.org/dev-guide/plugins/#mkdocs.plugins.CombinedEvent) in [**documentation**](https://www.mkdocs.org/dev-guide/plugins/#event-priorities). Context: [#&#8203;3448](https://togithub.com/mkdocs/mkdocs/issues/3448)

##### Enabling true generated files and expanding the [`File`](https://www.mkdocs.org/dev-guide/api/#mkdocs.structure.files.File) API

See [**documentation**](https://www.mkdocs.org/dev-guide/api/#mkdocs.structure.files.File).

-   There is a new pair of attributes [`File.content_string`](https://www.mkdocs.org/dev-guide/api/#mkdocs.structure.files.File.content_string]/\[\`content_bytes\`]\[mkdocs.structure.files.File.content_bytes) that becomes the official API for obtaining the content of a file and is used by MkDocs itself.

    This replaces the old approach where one had to manually read the file located at [`File.abs_src_path`](https://www.mkdocs.org/dev-guide/api/#mkdocs.structure.files.File.abs_src_path), although that is still the primary action that these new attributes do under the hood.

-   The content of a `File` can be backed by a string and no longer has to be a real existing file at `abs_src_path`.

    It is possible to **set** the attribute `File.content_string` or `File.content_bytes` and it will take precedence over `abs_src_path`.

    Further, `abs_src_path` is no longer guaranteed to be present and can be `None` instead. MkDocs itself still uses physical files in all cases, but eventually plugins will appear that don't populate this attribute.

-   There is a new constructor [`File.generated()`](https://www.mkdocs.org/dev-guide/api/#mkdocs.structure.files.File.generated) that should be used by plugins instead of the `File()` constructor. It is much more convenient because one doesn't need to manually look up the values such as `docs_dir` and `use_directory_urls`. Its signature is one of:

    ```python
    f = File.generated(config: MkDocsConfig, src_uri: str, content: str | bytes)
    f = File.generated(config: MkDocsConfig, src_uri: str, abs_src_path: str)
    ```

    This way, it is now extremely easy to add a virtual file even from a hook:

    ```python
    def on_files(files: Files, config: MkDocsConfig):
        files.append(File.generated(config, 'fake/path.md', content="Hello, world!"))
    ```

    For large content it is still best to use physical files, but one no longer needs to manipulate the path by providing a fake unused `docs_dir`.

-   There is a new attribute [`File.generated_by`](https://www.mkdocs.org/dev-guide/api/#mkdocs.structure.files.File.generated_by) that arose by convention - for generated files it should be set to the name of the plugin (the key in the `plugins:` collection) that produced this file. This attribute is populated automatically when using the `File.generated()` constructor.

-   It is possible to set the [`edit_uri`](https://www.mkdocs.org/dev-guide/api/#mkdocs.structure.files.File.edit_uri) attribute of a `File`, for example from a plugin or hook, to make it different from the default (equal to `src_uri`), and this will be reflected in the edit link of the document. This can be useful because some pages aren't backed by a real file and are instead created dynamically from some other source file or script. So a hook could set the `edit_uri` to that source file or script accordingly.

-   The `File` object now stores its original `src_dir`, `dest_dir`, `use_directory_urls` values as attributes.

-   Fields of `File` are computed on demand but cached. Only the three above attributes are primary ones, and partly also [`dest_uri`](https://www.mkdocs.org/dev-guide/api/#mkdocs.structure.files.File.dest_uri). This way, it is possible to, for example, overwrite `dest_uri` of a `File`, and `abs_dest_path` will be calculated based on it. However you need to clear the attribute first using `del f.abs_dest_path`, because the values are cached.

-   `File` instances are now hashable (can be used as keys of a `dict`). Two files can no longer be considered "equal" unless it's the exact same instance of `File`.

Other changes:

-   The internal storage of `File` objects inside a `Files` object has been reworked, so any plugins that choose to access `Files._files` will get a deprecation warning.

-   The order of `File` objects inside a `Files` collection is no longer significant when automatically inferring the `nav`. They get forcibly sorted according to the default alphabetic order.

Context: [#&#8203;3451](https://togithub.com/mkdocs/mkdocs/issues/3451), [#&#8203;3463](https://togithub.com/mkdocs/mkdocs/issues/3463)

#### Hooks and debugging

-   Hook files can now import adjacent \*.py files using the `import` statement. Previously this was possible to achieve only through a `sys.path` workaround. See the new mention in [documentation](https://www.mkdocs.org/user-guide/configuration/#hooks). Context: [#&#8203;3568](https://togithub.com/mkdocs/mkdocs/issues/3568)

-   Verbose `-v` log shows the sequence of plugin events in more detail - shows each invoked plugin one by one, not only the event type. Context: [#&#8203;3444](https://togithub.com/mkdocs/mkdocs/issues/3444)

#### Deprecations

-   Python 3.7 is no longer supported, Python 3.12 is officially supported. Context: [#&#8203;3429](https://togithub.com/mkdocs/mkdocs/issues/3429)

-   The theme config file `mkdocs_theme.yml` no longer executes YAML tags. Context: [#&#8203;3465](https://togithub.com/mkdocs/mkdocs/issues/3465)

-   The plugin event `on_page_read_source` is soft-deprecated because there is always a better alternative to it (see the new `File` API or just `on_page_markdown`, depending on the desired interaction).

    When multiple plugins/hooks apply this event handler, they trample over each other, so now there is a warning in that case.

    See [**documentation**](https://www.mkdocs.org/dev-guide/plugins/#on_page_read_source). Context: [#&#8203;3503](https://togithub.com/mkdocs/mkdocs/issues/3503)

##### API deprecations

-   It is no longer allowed to set `File.page` to a type other than `Page` or a subclass thereof. Context: [#&#8203;3443](https://togithub.com/mkdocs/mkdocs/issues/3443) - following the deprecation in version 1.5.3 and [#&#8203;3381](https://togithub.com/mkdocs/mkdocs/issues/3381).

-   `Theme._vars` is deprecated - use `theme['foo']` instead of `theme._vars['foo']`

-   `utils`: `modified_time()`, `get_html_path()`, `get_url_path()`, `is_html_file()`, `is_template_file()` are removed. `path_to_url()` is deprecated.

-   `LiveReloadServer.watch()` no longer accepts a custom callback.

Context: [#&#8203;3429](https://togithub.com/mkdocs/mkdocs/issues/3429)

#### Misc

-   The `sitemap.xml.gz` file is slightly more reproducible and no longer changes on every build, but instead only once per day (upon a date change). Context: [#&#8203;3460](https://togithub.com/mkdocs/mkdocs/issues/3460)

Other small improvements; see [commit log](https://togithub.com/mkdocs/mkdocs/compare/1.5.3...1.6.0).

### [`v1.5.3`](https://togithub.com/mkdocs/mkdocs/releases/tag/1.5.3)

[Compare Source](https://togithub.com/mkdocs/mkdocs/compare/1.5.2...1.5.3)

-   Fix `mkdocs serve` sometimes locking up all browser tabs when navigating quickly ([#&#8203;3390](https://togithub.com/mkdocs/mkdocs/issues/3390))

-   Add many new supported languages for "search" plugin - update lunr-languages to 1.12.0 ([#&#8203;3334](https://togithub.com/mkdocs/mkdocs/issues/3334))

-   Bugfix (regression in 1.5.0): In "readthedocs" theme the styling of "breadcrumb navigation" was broken for nested pages ([#&#8203;3383](https://togithub.com/mkdocs/mkdocs/issues/3383))

-   Built-in themes now also support Chinese (Traditional, Taiwan) language ([#&#8203;3370](https://togithub.com/mkdocs/mkdocs/issues/3370))

-   Plugins can now set `File.page` to their own subclass of `Page`. There is also now a warning if `File.page` is set to anything other than a strict subclass of `Page`. ([#&#8203;3367](https://togithub.com/mkdocs/mkdocs/issues/3367), [#&#8203;3381](https://togithub.com/mkdocs/mkdocs/issues/3381))

    Note that just instantiating a `Page` [sets the file automatically](https://togithub.com/mkdocs/mkdocs/blob/f94ab3f62d0416d484d81a0c695c8ca86ab3b975/mkdocs/structure/pages.py#L34), so care needs to be taken not to create an unneeded `Page`.

Other small improvements; see [commit log](https://togithub.com/mkdocs/mkdocs/compare/1.5.2...1.5.3).

### [`v1.5.2`](https://togithub.com/mkdocs/mkdocs/releases/tag/1.5.2)

[Compare Source](https://togithub.com/mkdocs/mkdocs/compare/1.5.1...1.5.2)

-   Bugfix (regression in 1.5.0): Restore functionality of `--no-livereload`. ([#&#8203;3320](https://togithub.com/mkdocs/mkdocs/issues/3320))

-   Bugfix (regression in 1.5.0): The new page title detection would sometimes be unable to drop anchorlinks - fix that. ([#&#8203;3325](https://togithub.com/mkdocs/mkdocs/issues/3325))

-   Partly bring back pre-1.5 API: `extra_javascript` items will once again be mostly strings, and only sometimes `ExtraStringValue` (when the extra `script` functionality is used).

    Plugins should be free to append strings to `config.extra_javascript`, but when reading the values, they must still make sure to read it as `str(value)` in case it is an `ExtraScriptValue` item. For querying the attributes such as `.type` you need to check `isinstance` first. Static type checking will guide you in that. ([#&#8203;3324](https://togithub.com/mkdocs/mkdocs/issues/3324))

See [commit log](https://togithub.com/mkdocs/mkdocs/compare/1.5.1...1.5.2).

### [`v1.5.1`](https://togithub.com/mkdocs/mkdocs/releases/tag/1.5.1)

[Compare Source](https://togithub.com/mkdocs/mkdocs/compare/1.5.0...1.5.1)

-   Bugfix (regression in 1.5.0): Make it possible to treat `ExtraScriptValue` as a path. This lets some plugins still work despite the breaking change.

-   Bugfix (regression in 1.5.0): Prevent errors for special setups that have 3 conflicting files, such as `index.html`, `index.md` *and* `README.md` ([#&#8203;3314](https://togithub.com/mkdocs/mkdocs/issues/3314))

See [commit log](https://togithub.com/mkdocs/mkdocs/compare/1.5.0...1.5.1).

</details>

---

### Configuration

📅 **Schedule**: Branch creation - "before 4am on Monday" (UTC), Automerge - At any time (no schedule defined).

🚦 **Automerge**: Disabled by config. Please merge this manually once you are satisfied.

♻ **Rebasing**: Whenever PR becomes conflicted, or you tick the rebase/retry checkbox.

🔕 **Ignore**: Close this PR and you won't be reminded about this update again.

---

 - [ ] <!-- rebase-check -->If you want to rebase/retry this PR, check this box

---

This PR was generated by [Mend Renovate](https://www.mend.io/free-developer-tools/renovate/). View the [repository job log](https://developer.mend.io/github/astral-sh/ruff).
<!--renovate-debug:eyJjcmVhdGVkSW5WZXIiOiIzOC4yMC4xIiwidXBkYXRlZEluVmVyIjoiMzguMjAuMSIsInRhcmdldEJyYW5jaCI6Im1haW4iLCJsYWJlbHMiOlsiaW50ZXJuYWwiXX0=-->


---

_Label `internal` added by @renovate[bot] on 2024-08-12 02:02_

---

_Assigned to @dhruvmanila by @dhruvmanila on 2024-08-12 04:25_

---

_Comment by @dhruvmanila on 2024-08-12 06:19_

I built the docs locally and I don't see any changes. Also, I don't see any entries in the changelog that could affect us. I also verified that the problems reported in https://github.com/astral-sh/ruff/pull/12712 is not originated via this upgrade.

---

_Merged by @dhruvmanila on 2024-08-12 06:19_

---

_Closed by @dhruvmanila on 2024-08-12 06:19_

---

_Branch deleted on 2024-08-12 06:19_

---
