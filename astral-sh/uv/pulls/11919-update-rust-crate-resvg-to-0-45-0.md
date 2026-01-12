```yaml
number: 11919
title: Update Rust crate resvg to 0.45.0
type: pull_request
state: closed
author: renovate
labels:
  - internal
assignees: []
base: main
head: renovate/resvg-0.x
created_at: 2025-03-03T03:38:04Z
updated_at: 2025-03-03T03:42:23Z
url: https://github.com/astral-sh/uv/pull/11919
synced_at: 2026-01-12T16:10:04Z
```

# Update Rust crate resvg to 0.45.0

---

_@renovate_

This PR contains the following updates:

| Package | Type | Update | Change |
|---|---|---|---|
| [resvg](https://redirect.github.com/linebender/resvg) | dependencies | minor | `0.29.0` -> `0.45.0` |

---

### Release Notes

<details>
<summary>linebender/resvg (resvg)</summary>

### [`v0.45.0`](https://redirect.github.com/linebender/resvg/blob/HEAD/CHANGELOG.md#0450---2025-02-26)

[Compare Source](https://redirect.github.com/linebender/resvg/compare/v0.44.0...0.45.0)

This is the first release under the stewardship of \[Linebender]\[], who is now responsible for maintenance of this crate.
Many thanks to Yevhenii Reizner for the years of hard work that he has poured into this and other crates.

Please note that the license of this project changed from `MPL-2.0` to `Apache-2.0 OR MIT`.
See [resvg#838](https://redirect.github.com/linebender/resvg/issues/838) for more information.

This release has an MSRV of 1.65 for `usvg` and 1.67.1 for `resvg` and the C API.

##### Added

-   Support for the `background-color` attribute.
-   Support for additional `image-rendering` attributes.
-   Support for the `!important` CSS flag.
-   Support for Luma JPEG images.
-   (c-api) `resvg_options_set_stylesheet`.
    Thanks to \[[@&#8203;michabay05](https://redirect.github.com/michabay05)]\[].
-   (svgtypes) Support for floating point hue in `hsl()` and `hsla()`.

##### Changed

-   License to `Apache-2.0 OR MIT`.
    See [resvg#838](https://redirect.github.com/linebender/resvg/issues/838) for more information.
-   Updated WebP decoder for bug fixes and improved performance.
    Thanks to \[[@&#8203;Shnatsel](https://redirect.github.com/Shnatsel)]\[].
-   MSRV of resvg and c-api bumped to 1.67.1.
-   `fontdb` and `rustybuzz` have been updated.
-   Updated other dependencies.
-   (svgtypes) Simplified color component rounding and bounds checking.
-   Improved handling of paths with paint order `markers` but no actual markers.

##### Fixed

-   Relative unit handling when `use` references `symbol`.
-   (svgtypes) Rounding of hues in HSL to RGB conversion.
-   (svgtypes) Rounding of alpha.

### [`v0.44.0`](https://redirect.github.com/linebender/resvg/blob/HEAD/CHANGELOG.md#0440---2024-09-28)

[Compare Source](https://redirect.github.com/linebender/resvg/compare/v0.43.0...v0.44.0)

##### Added

-   Stylesheet injection support.
    Thanks to [@&#8203;LaurenzV](https://redirect.github.com/LaurenzV).
-   (c-api) `resvg_get_object_bbox`
-   (c-api) `cargo-c` metadata.
    Thanks to [@&#8203;lu-zero](https://redirect.github.com/lu-zero).
-   Implement `From` for `fontdb` and `usvg` font types.
    Thanks to [@&#8203;dhardy](https://redirect.github.com/dhardy).

##### Changed

-   (c-api) `resvg_get_image_bbox` returns a *layer* and not *object* bounding box now.
    Use `resvg_get_object_bbox` to preserve the old behavior.

##### Fixed

-   (svgtypes) Path parsing with `S` or `T` segments after `A`.
-   Bounding box calculation for the root group used for `viewBox` flattening.

### [`v0.43.0`](https://redirect.github.com/linebender/resvg/blob/HEAD/CHANGELOG.md#0430---2024-08-10)

[Compare Source](https://redirect.github.com/linebender/resvg/compare/v0.42.0...v0.43.0)

##### Added

-   Support WebP images.
    Thanks to [@&#8203;notjosh](https://redirect.github.com/notjosh).

##### Changed

-   Use `zune-jpeg` instead of `jpeg-decoder`.
    Thanks to [@&#8203;mattfbacon](https://redirect.github.com/mattfbacon).
-   Update dependencies.

##### Fixed

-   Canvas size limits calculation.
-   SVG fonts handling.
    Thanks to [@&#8203;LaurenzV](https://redirect.github.com/LaurenzV).
-   Transforms in COLR fonts.
    Thanks to [@&#8203;LaurenzV](https://redirect.github.com/LaurenzV).

### [`v0.42.0`](https://redirect.github.com/linebender/resvg/blob/HEAD/CHANGELOG.md#0420---2024-06-01)

[Compare Source](https://redirect.github.com/linebender/resvg/compare/v0.41.0...v0.42.0)

##### Added

-   `resvg` can render color fonts now, aka Emojis.<br>
    In TrueType terms, `COLRv0`, `COLRv1` (mostly), `sbix`, `CBDT` and `SVG` tables are supported.<br>
    Thanks to [@&#8203;LaurenzV](https://redirect.github.com/LaurenzV).
-   Fonts matching and fallback can be controlled by the caller via `usvg::FontResolver` now.
    Thanks to [@&#8203;LaurenzV](https://redirect.github.com/LaurenzV).
-   `usvg::Options::font_resolver`. Similar to `usvg::Options::image_href_resolver` we already had.
-   `usvg::Options::fontdb`
-   Support double-quoted FuncIRIs, aka `url("#id")`.
-   `image` element viewbox flattening.<br>
    Instead of having `usvg::Image::view_box` that the caller should handle themselves,
    we instead replace it with `transform` and optional `clip-path`.
    This greatly simplifies `image` rendering.
-   `usvg::Image::size`
-   Tree viewbox flattening.<br>
    Similar to `image` above, but affects the root `svg` element instead.
-   `pattern` viewbox flattening.<br>
    Similar to `image` above, but for patterns.
-   Improve vertical text rendering.
    Thanks to [@&#8203;LaurenzV](https://redirect.github.com/LaurenzV).

##### Changed

-   `usvg::fontdb::Database` should be set in `usvg::Options` and not passed
    to the parser separately now.
-   `usvg::Options` and `usvg::ImageHrefResolver` have a lifetime now.
-   Replace `usvg::Visibility` enum with just `bool`.
-   `usvg::Path::visibility()` is replaced with `usvg::Path::is_visible()`
-   `usvg::Image::visibility()` is replaced with `usvg::Image::is_visible()`
-   `usvg::TextSpan::visibility()` is replaced with `usvg::TextSpan::is_visible()`
-   Always represent `feImage` content as a link to an element.<br>
    In SVG, `feImage` can contain a link to an element or a base64 image data, just like `image`.
    From now, the inlined base64 data will always be represented by a link to an actual `image` element.
    ```xml
    <filter>
      <feImage xlink:href="data:image/png;base64,..."/>
    </filter>
    ```
    will be parsed as
    ```xml
    <image id="image1" xlink:href="data:image/png;base64,..."/>
    <filter>
      <feImage xlink:href="#image1"/>
    </filter>
    ```
    This simplifies `feImage` rendering, since we don't have to handle both cases now.
-   The `--list-fonts` resvg argument can be used without providing an SVG file now.
    Can simply call `resvg --list-fonts` now.
-   The `--list-fonts` resvg argument includes generic font family names as well now.
-   Make sure all warning and errors are printed to stderr.
    Thanks to [@&#8203;ahaoboy](https://redirect.github.com/ahaoboy).

##### Removed

-   `usvg::ViewBox`, `usvg::AspectRatio`, `usvg::Align` types. Nol longer used.
-   `usvg::filter::Image::aspect`. No longer needed.
-   `usvg::filter::Image::rendering_mode`. No longer needed.
-   `usvg::filter::Image::data`. Use `usvg::filter::Image::root` instead.
-   `usvg::Tree::view_box`. No longer needed.
-   `usvg::Image::view_box`. No longer needed.
-   `usvg::Image::pattern`. No longer needed.
-   `usvg::utils::align_pos`. No longer needed.
-   `usvg::Visibility`. No longer needed.
-   (c-api) `resvg_get_image_viewbox`. Use `resvg_get_image_size` instead.

##### Fixed

-   `context-fill` handling.
    Thanks to [@&#8203;LaurenzV](https://redirect.github.com/LaurenzV).

### [`v0.41.0`](https://redirect.github.com/linebender/resvg/blob/HEAD/CHANGELOG.md#0410---2024-04-03)

[Compare Source](https://redirect.github.com/linebender/resvg/compare/v0.40.0...v0.41.0)

##### Added

-   `context-fill` and `context-stroke` support.
    Thanks to [@&#8203;LaurenzV](https://redirect.github.com/LaurenzV).
-   `usvg::Text::layouted()`, which returns a list of glyph IDs.
    It can be used to manually draw glyphs, unlike with `usvg::Text::flattened()`, which returns
    just vector paths.
    Thanks to [@&#8203;LaurenzV](https://redirect.github.com/LaurenzV).

##### Fixed

-   Missing text when a `text` element uses multiple fonts and one of them produces ligatures.
-   Absolute transform propagation during `use` resolving.
-   Absolute transform propagation during nested `svg` resolving.
-   `Node::abs_transform` documentation. The current element's transform *is* included.

### [`v0.40.0`](https://redirect.github.com/linebender/resvg/blob/HEAD/CHANGELOG.md#0400---2024-02-17)

[Compare Source](https://redirect.github.com/linebender/resvg/compare/v0.39.0...v0.40.0)

##### Added

-   `usvg::Tree` is `Send + Sync` compatible now.
-   `usvg::WriteOptions::preserve_text` to control how `usvg` generates an SVG.
-   `usvg::Image::abs_bounding_box`

##### Changed

-   All types in `usvg` are immutable now. Meaning that `usvg::Tree` cannot be modified
    after creation anymore.
-   All struct fields in `usvg` are private now. Use getters instead.
-   All `usvg::Tree` parsing methods require the `fontdb` argument now.
-   All `defs` children like gradients, patterns, clipPaths, masks and filters are guarantee
    to have a unique, non-empty ID.
-   All `defs` children like gradients, patterns, clipPaths, masks and filters are guarantee
    to have `userSpaceOnUse` units now. No `objectBoundingBox` units anymore.
-   `usvg::Mask` is allowed to have no children now.
-   Text nodes will not be parsed when the `text` build feature isn't enabled.
-   `usvg::Tree::clip_paths`, `usvg::Tree::masks`, `usvg::Tree::filters` returns
    a pre-collected slice of unique nodes now.
    It's no longer a closure and you do not have to deduplicate nodes by yourself.
-   `usvg::filter::Primitive::x`, `y`, `width` and `height` methods were replaced
    with `usvg::filter::Primitive::rect`.
-   Split `usvg::Tree::paint_servers` into `usvg::Tree::linear_gradients`,
    `usvg::Tree::radial_gradients`, `usvg::Tree::patterns`.
    All three returns pre-collected slices now.
-   A `usvg::Path` no longer can have an invalid bbox. Paths with an invalid bbox will be
    rejected during parsing.
-   All `usvg` methods that return bounding boxes return non-optional `Rect` now.
    No `NonZeroRect` as well.
-   `usvg::Text::flattened` returns `&Group` and not `Option<&Group>` now.
-   `usvg::ImageHrefDataResolverFn` and `usvg::ImageHrefStringResolverFn`
    require `fontdb::Database` argument.
-   All shared nodes are stored in `Arc` and not `Rc<RefCell>` now.
-   `resvg::render_node` now includes filters bounding box. Meaning that a node with a blur filter
    no longer be clipped.
-   Replace `usvg::utils::view_box_to_transform` with `usvg::ViewBox::to_transform`.
-   Rename `usvg::XmlOptions` into `usvg::WriteOptions` and embed `xmlwriter::Options`.

##### Removed

-   `usvg::Tree::postprocess()` and `usvg::PostProcessingSteps`. No longer needed.
-   `usvg::ClipPath::units()`, `usvg::Mask::units()`, `usvg::Mask::content_units()`,
    `usvg::Filter::units()`, `usvg::Filter::content_units()`, `usvg::LinearGradient::units()`,
    `usvg::RadialGradient::units()`, `usvg::Pattern::units()`, `usvg::Pattern::content_units()`
    and `usvg::Paint::units()`. They are always `userSpaceOnUse` now.
-   `usvg::Units`. No longer needed.

##### Fixed

-   Text bounding box is accounted during SVG size resolving.
    Previously, only paths and images were included.
-   Font selection when an italic font isn't explicitly marked as one.
-   Preserve `image` aspect ratio when only `width` or `height` are present.
    Thanks to [@&#8203;LaurenzV](https://redirect.github.com/LaurenzV).

### [`v0.39.0`](https://redirect.github.com/linebender/resvg/blob/HEAD/CHANGELOG.md#0390---2024-02-06)

[Compare Source](https://redirect.github.com/linebender/resvg/compare/v0.38.0...v0.39.0)

##### Added

-   `font` shorthand parsing.
    Thanks to [@&#8203;LaurenzV](https://redirect.github.com/LaurenzV).
-   `usvg::Group::abs_bounding_box`
-   `usvg::Group::abs_stroke_bounding_box`
-   `usvg::Path::abs_bounding_box`
-   `usvg::Path::abs_stroke_bounding_box`
-   `usvg::Text::abs_bounding_box`
-   `usvg::Text::abs_stroke_bounding_box`

##### Changed

-   All `usvg-*` crates merged into one. There is just the `usvg` crate now, as before.

##### Removed

-   `usvg::Group::abs_bounding_box()` method. It's a field now.
-   `usvg::Group::abs_filters_bounding_box()`
-   `usvg::TreeParsing`, `usvg::TreePostProc` and `usvg::TreeWriting` traits.
    They are no longer needed.

##### Fixed

-   `font-family` parsing.
    Thanks to [@&#8203;LaurenzV](https://redirect.github.com/LaurenzV).
-   Absolute bounding box calculation for paths.

### [`v0.38.0`](https://redirect.github.com/linebender/resvg/blob/HEAD/CHANGELOG.md#0380---2024-01-21)

[Compare Source](https://redirect.github.com/linebender/resvg/compare/v0.37.0...v0.38.0)

##### Added

-   Each `usvg::Node` stores its absolute transform now.
    `Node::abs_transform()` executes in constant time now.
-   `usvg::Tree::calculate_bounding_boxes` to calculate all bounding boxes beforehand.
-   `usvg::Node::bounding_box` which returns a precalculated node's bounding box in object coordinates.
-   `usvg::Node::abs_bounding_box` which returns a precalculated node's bounding box in canvas coordinates.
-   `usvg::Node::stroke_bounding_box` which returns a precalculated node's bounding box,
    including stroke, in object coordinates.
-   `usvg::Node::abs_stroke_bounding_box` which returns a precalculated node's bounding box,
    including stroke, in canvas coordinates.
-   (c-api) `resvg_get_node_stroke_bbox`
-   `usvg::Node::filters_bounding_box`
-   `usvg::Node::abs_filters_bounding_box`
-   `usvg::Tree::postprocess`

##### Changed

-   `resvg` renders `usvg::Tree` directly again. `resvg::Tree` is gone.
-   `usvg` no longer uses `rctree` for the nodes tree implementation.
    The tree is a regular `enum` now.
    -   A caller no longer need to use the awkward `*node.borrow()`.
    -   No more panics on incorrect mutable `Rc<RefCell>` access.
    -   Tree nodes respect tree's mutability rules. Before, one could mutate tree nodes when the tree
        itself is not mutable. Because `Rc<RefCell>` provides a shared mutable access.
-   Filters, clip paths, masks and patterns are stored as `Rc<RefCell<T>>` instead of `Rc<T>`.
    This is required for proper mutability since `Node` itself is no longer an `Rc`.
-   Rename `usvg::NodeKind` into `usvg::Node`.
-   Upgrade to Rust 2021 edition.

##### Removed

-   `resvg::Tree`. No longer needed. `resvg` can render `usvg::Tree` directly once again.
-   `rctree::Node` methods. The `Node` API is completely different now.
-   `usvg::NodeExt`. No longer needed.
-   `usvg::Node::calculate_bbox`. Use `usvg::Node::abs_bounding_box` instead.
-   `usvg::Tree::convert_text`. Use `usvg::Tree::postprocess` instead.
-   `usvg::TreeTextToPath` trait. No longer needed.

##### Fixed

-   Mark `mask-type` as a presentation attribute.
-   Do not show needless warnings when parsing some attributes.
-   `feImage` rendering with a non-default position.
    Thanks to [@&#8203;LaurenzV](https://redirect.github.com/LaurenzV).

### [`v0.37.0`](https://redirect.github.com/linebender/resvg/blob/HEAD/CHANGELOG.md#0370---2023-12-16)

[Compare Source](https://redirect.github.com/linebender/resvg/compare/v0.36.0...v0.37.0)

##### Added

-   `usvg` can write text back to SVG now.
    Thanks to [@&#8203;LaurenzV](https://redirect.github.com/LaurenzV).
-   `--preserve-text` flag to the `usvg` CLI tool.
    Thanks to [@&#8203;LaurenzV](https://redirect.github.com/LaurenzV).
-   Support [`transform-origin`](https://drafts.csswg.org/css-transforms/#transform-origin-property)
    property.
    Thanks to [@&#8203;LaurenzV](https://redirect.github.com/LaurenzV).
-   Support non-default markers order via
    [`paint-order`](https://svgwg.org/svg2-draft/painting.html#PaintOrder).
    Previously, only fill and stroke could have been swapped.
    Thanks to [@&#8203;LaurenzV](https://redirect.github.com/LaurenzV).
-   `usvg_tree::Text::flattened` that will contain a flattened/outlined text.
-   `usvg_tree::Text::bounding_box`. Will be set only after text flattening.
-   Optimize `usvg_tree::NodeExt::abs_transform` by storing absolute transforms in the tree
    instead of calculating them each time.

##### Changed

-   `usvg_tree::Text::positions` was replaced with `usvg_tree::Text::dx` and `usvg_tree::Text::dy`.<br>
    `usvg_tree::CharacterPosition::x` and `usvg_tree::CharacterPosition::y` are gone.
    They were redundant and you should use `usvg_tree::TextChunk::x`
    and `usvg_tree::TextChunk::y` instead.
-   `usvg_tree::LinearGradient::id` and `usvg_tree::RadialGradient::id` are moved to
    `usvg_tree::BaseGradient::id`.
-   Do not generate element IDs during parsing. Previously, some elements like `clipPath`s
    and `filter`s could have generated IDs, but it wasn't very reliable and mostly unnecessary.
    Renderer doesn't rely on them and usvg writer would generate them anyway.
-   Text-to-paths conversion via `usvg_text_layout::Tree::convert_text` no longer replaces
    original text elements with paths, but instead puts them into `usvg_tree::Text::flattened`.

##### Removed

-   The `transform` field from `usvg_tree::Path`, `usvg_tree::Image` and `usvg_tree::Text`.
    Only `usvg_tree::Group` can have it.<br>
    It doesn't break anything, because those properties were never used before anyway.<br>
    Thanks to [@&#8203;LaurenzV](https://redirect.github.com/LaurenzV).
-   `usvg_tree::CharacterPosition`
-   `usvg_tree::Path::text_bbox`. Use `usvg_tree::Text::bounding_box` instead.
-   `usvg_text_layout::TextToPath` trait for `Text` nodes.
    Only the whole tree can be converted at once.

##### Fixed

-   Path object bounding box calculation. We were using point bounds instead of tight contour bounds.
    Was broken since v0.34
-   Convert text-to-paths in embedded SVGs as well. The one inside the `Image` node.
    Thanks to [@&#8203;LaurenzV](https://redirect.github.com/LaurenzV).
-   Indirect `text-decoration` resolving in some cases.
    Thanks to [@&#8203;LaurenzV](https://redirect.github.com/LaurenzV).
-   (usvg) Clip paths writing to SVG.
    Thanks to [@&#8203;LaurenzV](https://redirect.github.com/LaurenzV).

### [`v0.36.0`](https://redirect.github.com/linebender/resvg/blob/HEAD/CHANGELOG.md#0360---2023-10-01)

[Compare Source](https://redirect.github.com/linebender/resvg/compare/v0.35.0...v0.36.0)

##### Added

-   `stroke-linejoin=miter-clip` support. SVG2.
    Thanks to [@&#8203;torokati44](https://redirect.github.com/torokati44).
-   Quoted FuncIRI support. Like `fill="url('#gradient')"`. SVG2.
    Thanks to [@&#8203;romanzes](https://redirect.github.com/romanzes).
-   Allow float values in `rgb()` and `rgba()` colors. SVG2.
    Thanks to [@&#8203;yisibl](https://redirect.github.com/yisibl).
-   `auto-start-reverse` variant support to `orient` in markers. SVG2.
    Thanks to [@&#8203;EpicEricEE](https://redirect.github.com/EpicEricEE).

##### Changed

-   Update dependencies.

##### Fixed

-   Increase precision of the zero-scale transform check.
    Was rejecting some valid transforms before.
-   Panic when rendering a very specific text.
-   Greatly improve parsing performance when an SVG has a lot of references.
    Thanks to [@&#8203;wez](https://redirect.github.com/wez).
-   (Qt API) Fix scaling factor calculation.
    Thanks to [@&#8203;missdeer](https://redirect.github.com/missdeer).

### [`v0.35.0`](https://redirect.github.com/linebender/resvg/blob/HEAD/CHANGELOG.md#0350---2023-06-27)

[Compare Source](https://redirect.github.com/linebender/resvg/compare/v0.34.1...v0.35.0)

##### Fixed

-   Panic when an element is completely outside the viewbox.

##### Removed

-   `FillPaint` and `StrokePaint` filter inputs support.
    It's a mostly undocumented SVG feature that no one supports and no one uses.
    And it was adding a significant complexity to the codebase.
-   `usvg::filter::Filter::fill_paint` and `usvg::filter::Filter::stroke_paint`.
-   `BackgroundImage`, `BackgroundAlpha`, `FillPaint` and `StrokePaint` from `usvg::filter::Input`.
-   `usvg::Group::filter_fill_paint` and `usvg::Group::filter_stroke_paint`.

### [`v0.34.1`](https://redirect.github.com/linebender/resvg/blob/HEAD/CHANGELOG.md#0341---2023-05-28)

[Compare Source](https://redirect.github.com/linebender/resvg/compare/v0.34.0...v0.34.1)

##### Fixed

-   Transform components order. Affects only `usvg` SVG output and C API.

### [`v0.34.0`](https://redirect.github.com/linebender/resvg/blob/HEAD/CHANGELOG.md#0340---2023-05-27)

[Compare Source](https://redirect.github.com/linebender/resvg/compare/v0.33.0...v0.34.0)

##### Changed

-   `usvg` uses `tiny-skia` geometry primitives now, including the `Path` container.<br>
    The main difference compared to the old `usvg` primitives
    is that `tiny-skia` uses `f32` instead of `f64`.
    So while in theory we could loose some precision, in practice, `f32` is used mainly
    as a storage type and precise math operations are still done using `f64`.<br>
    `tiny-skia` primitives are move robust, strict and have a nicer API.<br>
    More importantly, this change reduces the peak memory usages for SVGs with large paths
    (in terms of the number of segments).
    And removes the need to convert `usvg::PathData` into `tiny-skia::Path` before rendering.
    Which was just a useless reallocation.
-   All numbers are stored as `f32` instead of `f64` now.
-   Because we use `tiny-skia::Path` now, we allow *quadratic curves* as well.
    This includes `usvg` CLI output.
-   Because we allow *quadratic curves* now, text might render slightly differently (better?).
    This is because TrueType fonts contain only *quadratic curves*
    and we were converting them to cubic before.
-   `usvg::Path` no longer implements `Default`. Use `usvg::Path::new` instead.
-   Replace `usvg::Rect` with `tiny_skia::NonZeroRect`.
-   Replace `usvg::PathBbox` with `tiny_skia::Rect`.
-   Unlike the old `usvg::PathBbox`, `tiny_skia::Rect` allows both width and height to be zero.
    This is not an error.
-   `usvg::filter::Turbulence::base_frequency` was split into `base_frequency_x` and `base_frequency_y`.
-   `usvg::NodeExt::calculate_bbox` no longer includes stroke bbox.
-   (c-api) Use `float` instead of `double` everywhere.
-   The `svgfilters` crate was merged into `resvg`.
-   The `rosvgtree` crate was merged into `usvg-parser`.
-   `usvg::Group::filter_fill` moved to `usvg::filter::Filter::fill_paint`.
-   `usvg::Group::filter_stroke` moved to `usvg::filter::Filter::stroke_paint`.

##### Remove

-   `usvg::Point`. Use `tiny_skia::Point` instead.
-   `usvg::FuzzyEq`. Use `usvg::ApproxEqUlps` instead.
-   `usvg::FuzzyZero`. Use `usvg::ApproxZeroUlps` instead.
-   (c-api) `resvg_path_bbox`. Use `resvg_rect` instead.
-   `svgfilters` crate.
-   `rosvgtree` crate.

##### Fixed

-   Write `transform` on `clipPath` children in `usvg` SVG output.
-   Do not duplicate marker children IDs.
    Previously, each element resolved for a marker would preserve its ID.
    Affects only `usvg` SVG output and doesn't affect rendering.

### [`v0.33.0`](https://redirect.github.com/linebender/resvg/blob/HEAD/CHANGELOG.md#0330---2023-05-17)

[Compare Source](https://redirect.github.com/linebender/resvg/compare/v0.32.0...v0.33.0)

##### Added

-   A new rendering algorithm.<br>
    When rendering [isolated groups](https://razrfalcon.github.io/notes-on-svg-parsing/isolated-groups.html),
    aka layers, we have to know the layer bounding box beforehand, which is ridiculously hard in SVG.<br>
    Previously, resvg would simply use the canvas size for all the layers.
    Meaning that to render a 10x10px layer on a 1000x1000px canvas, we would have to allocate and then blend
    a 1000x1000px layer, which is just a waste of CPU cycles.<br>
    The new rendering algorithm is able to calculate layer bounding boxes, which dramatically improves
    performance when rendering a lot of tiny layers on a large canvas.<br>
    Moreover, it makes performance more linear with a canvas size increase.<br>
    The [paris-30k.svg](https://redirect.github.com/google/forma/blob/681e8bfd348caa61aab47437e7d857764c2ce522/assets/svgs/paris-30k.svg)
    sample from [google/forma](https://redirect.github.com/google/forma) is rendered *115 times* faster on M1 Pro now.
    From ~33760ms down to ~290ms. 5269x3593px canvas.<br>
    If we restrict the canvas to 1000x1000px, which would contain only the actual `paris-30k.svg` content,
    then we're *13 times* faster. From ~3252ms down to ~253ms.
-   `resvg::Tree`, aka a render tree, which is an even simpler version of `usvg::Tree`.
    `usvg::Tree` had to be converted into `resvg::Tree` before rendering now.

##### Changed

-   Restructure the root directory. All crates are in the `crates` directory now.
-   Restructure tests. New directory structure and naming scheme.
-   Use `resvg::Tree::render` instead of `resvg::render`.
-   resvg's `--export-area-drawing` option uses calculated bounds instead of trimming
    excessive alpha now. It's faster, but can lead to a slightly different output.
-   (c-api) Removed `fit_to` argument from `resvg_render`.
-   (c-api) Removed `fit_to` argument from `resvg_render_node`.
-   `usvg::ScreenSize` moved to `resvg`.
-   `usvg::ScreenRect` moved to `resvg`.
-   Rename `resvg::ScreenSize` into `resvg::IntSize`.
-   Rename `resvg::ScreenRect` into `resvg::IntRect`.

##### Removed

-   `filter` build feature from `resvg`. Filters are always enabled now.
-   `resvg::FitTo`
-   `usvg::utils::view_box_to_transform_with_clip`
-   `usvg::Size::to_screen_size`. Use `resvg::IntSize::from_usvg` instead.
-   `usvg::Rect::to_screen_size`. Use `resvg::IntSize::from_usvg(rect.size())` instead.
-   `usvg::Rect::to_screen_rect`. Use `resvg::IntRect::from_usvg` instead.
-   (c-api) `resvg_fit_to`
-   (c-api) `resvg_fit_to_type`

##### Fixed

-   Double quotes parsing in `font-family`.

### [`v0.32.0`](https://redirect.github.com/linebender/resvg/blob/HEAD/CHANGELOG.md#0320---2023-04-23)

[Compare Source](https://redirect.github.com/linebender/resvg/compare/v0.31.1...v0.32.0)

##### Added

-   Clipping and masking is up to 20% faster.
-   `mask-type` property support. SVG2
-   `usvg_tree::MaskType`
-   `usvg_tree::Mask::kind`
-   (rosvgtree) New SVG 2 mask attributes.

##### Changed

-   `BackgroundImage` and `BackgroundAlpha` filter inputs will produce the same output
    as `SourceGraphic` and `SourceAlpha` respectively.

##### Removed

-   `enable-background` support. This feature was never supported by browsers
    and was deprecated in SVG 2. To my knowledge, only Batik has a good support of it.
    Also, it's a performance nightmare, which caused multiple issues in resvg already.
-   `usvg_tree::EnableBackground`
-   `usvg_tree::Group::enable_background`
-   `usvg_tree::NodeExt::filter_background_start_node`

##### Fixed

-   Improve rectangular clipping anti-aliasing quality.
-   Mask's RGB to Luminance converter was ignoring premultiplied alpha.

### [`v0.31.1`](https://redirect.github.com/linebender/resvg/blob/HEAD/CHANGELOG.md#0311---2023-04-22)

[Compare Source](https://redirect.github.com/linebender/resvg/compare/v0.31.0...v0.31.1)

##### Fixed

-   Use the latest `tiny-skia` to fix SVGs with large masks rendering.

### [`v0.31.0`](https://redirect.github.com/linebender/resvg/blob/HEAD/CHANGELOG.md#0310---2023-04-10)

[Compare Source](https://redirect.github.com/linebender/resvg/compare/v0.30.0...v0.31.0)

##### Added

-   `usvg::Tree::paint_servers`
-   `usvg::Tree::clip_paths`
-   `usvg::Tree::masks`
-   `usvg::Tree::filters`
-   `usvg::Node::subroots`
-   (usvg) `--coordinates-precision` and `--transforms-precision` writing options.
    Thanks to [@&#8203;flxzt](https://redirect.github.com/flxzt).

##### Fixed

-   `fill-opacity` and `stroke-opacity` resolving.
-   Double `transform` when resolving `symbol`.
-   `symbol` clipping when its viewbox is the same as the document one.
-   (usvg) Deeply nested gradients, patterns, clip paths, masks and filters
    were ignored during SVG writing.
-   Missing text in nested clip paths and mask, text decoration patterns, filter inputs and feImage.

### [`v0.30.0`](https://redirect.github.com/linebender/resvg/blob/HEAD/CHANGELOG.md#0300---2023-03-25)

[Compare Source](https://redirect.github.com/linebender/resvg/compare/v0.29.0...v0.30.0)

##### Added

-   Readd `usvg` CLI tool. Can be installed via cargo as before.

##### Changed

-   Extract most `usvg` internals into new `usvg-tree` and `usvg-parser` crates.
    `usvg-tree` contains just the SVG tree and all the types.
    `usvg-parser` parsers SVG into `usvg-tree`.
    And `usvg` is just an umbrella crate now.
-   To use `usvg::Tree::from*` methods one should import the `usvg::TreeParsing` trait now.
-   No need to import `usvg-text-layout` manually anymore. It is part of `usvg` now.
-   `rosvgtree` no longer reexports `svgtypes`.
-   `rosvgtree::Node::attribute` returns just a string now.
-   `rosvgtree::Node::find_attribute` returns just a `rosvgtree::Node` now.
-   Rename `usvg::Stretch` into `usvg::FontStretch`.
-   Rename `usvg::Style` into `usvg::FontStyle`.
-   `usvg::FitTo` moved to `resvg::FitTo`.
-   `usvg::IsDefault` trait is private now.

##### Removed

-   `rosvgtree::FromValue`. Due to Rust's orphan rules this trait is pretty useless.

##### Fixed

-   Recursive markers detection.
-   Skip malformed `transform` attributes without skipping the whole element.
-   Clipping path rectangle calculation for nested `svg` elements.
    Thanks to [@&#8203;LaurenzV](https://redirect.github.com/LaurenzV).
-   Panic when applying `text-decoration` on text with only one cluster.
    Thanks to [@&#8203;LaurenzV](https://redirect.github.com/LaurenzV).
-   (Qt API) Image size wasn't initialized. Thanks to [@&#8203;missdeer](https://redirect.github.com/missdeer).
-   `resvg` CLI allows files with XML DTD again.
-   (svgtypes) Handle implicit MoveTo after ClosePath segments.

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

This PR was generated by [Mend Renovate](https://mend.io/renovate/). View the [repository job log](https://developer.mend.io/github/astral-sh/uv).
<!--renovate-debug:eyJjcmVhdGVkSW5WZXIiOiIzOS4xNzYuMiIsInVwZGF0ZWRJblZlciI6IjM5LjE3Ni4yIiwidGFyZ2V0QnJhbmNoIjoibWFpbiIsImxhYmVscyI6WyJpbnRlcm5hbCJdfQ==-->


---

_Label `internal` added by @renovate[bot] on 2025-03-03 03:38_

---

_Closed by @charliermarsh on 2025-03-03 03:41_

---

_Comment by @renovate[bot] on 2025-03-03 03:42_

### Renovate Ignore Notification

Because you closed this PR without merging, Renovate will ignore this update (`0.45.0`). You will get a PR once a newer version is released. To ignore this dependency forever, add it to the `ignoreDeps` array of your Renovate config.

If you accidentally closed this PR, or if you changed your mind: rename this PR to get a fresh replacement PR.

---

_Branch deleted on 2025-03-03 03:42_

---
