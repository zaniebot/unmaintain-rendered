```yaml
number: 2368
title: Bump resvg from 0.29.0 to 0.40.0
type: pull_request
state: closed
author: dependabot
labels:
  - internal
assignees: []
base: main
head: dependabot/cargo/resvg-0.40.0
created_at: 2024-03-11T21:35:13Z
updated_at: 2024-03-11T21:46:40Z
url: https://github.com/astral-sh/uv/pull/2368
synced_at: 2026-01-12T16:05:00Z
```

# Bump resvg from 0.29.0 to 0.40.0

---

_@dependabot_

Bumps [resvg](https://github.com/RazrFalcon/resvg) from 0.29.0 to 0.40.0.
<details>
<summary>Release notes</summary>
<p><em>Sourced from <a href="https://github.com/RazrFalcon/resvg/releases">resvg's releases</a>.</em></p>
<blockquote>
<h2>v0.40.0</h2>
<ul>
<li><code>viewsvg</code> is a simple application that showcases <em>resvg</em> capabilities</li>
<li><code>resvg-0.*.0.tar.xz</code> is a sources archive with vendored Rust dependencies</li>
<li><code>resvg-explorer-extension.exe</code> is an SVG thumbnailer for Windows Explorer</li>
</ul>
<h2>v0.39.0</h2>
<ul>
<li><code>viewsvg</code> is a simple application that showcases <em>resvg</em> capabilities</li>
<li><code>resvg-0.*.0.tar.xz</code> is a sources archive with vendored Rust dependencies</li>
<li><code>resvg-explorer-extension.exe</code> is an SVG thumbnailer for Windows Explorer</li>
</ul>
<h2>v0.38.0</h2>
<ul>
<li><code>viewsvg</code> is a simple application that showcases <em>resvg</em> capabilities</li>
<li><code>resvg-0.*.0.tar.xz</code> is a sources archive with vendored Rust dependencies</li>
<li><code>resvg-explorer-extension.exe</code> is an SVG thumbnailer for Windows Explorer</li>
</ul>
<h2>v0.37.0</h2>
<ul>
<li><code>viewsvg</code> is a simple application that showcases <em>resvg</em> capabilities</li>
<li><code>resvg-0.*.0.tar.xz</code> is a sources archive with vendored Rust dependencies</li>
<li><code>resvg-explorer-extension.exe</code> is an SVG thumbnailer for Windows Explorer</li>
</ul>
<h2>v0.36.0</h2>
<ul>
<li><code>viewsvg</code> is a simple application that showcases <em>resvg</em> capabilities</li>
<li><code>resvg-0.*.0.tar.xz</code> is a sources archive with vendored Rust dependencies</li>
<li><code>resvg-explorer-extension.exe</code> is an SVG thumbnailer for Windows Explorer</li>
</ul>
<h2>v0.35.0</h2>
<ul>
<li><code>viewsvg</code> is a simple application that showcases <em>resvg</em> capabilities</li>
<li><code>resvg-0.*.0.tar.xz</code> is a sources archive with vendored Rust dependencies</li>
<li><code>resvg-explorer-extension.exe</code> is an SVG thumbnailer for Windows Explorer</li>
</ul>
<h2>v0.34.1</h2>
<ul>
<li><code>viewsvg</code> is a simple application that showcases <em>resvg</em> capabilities</li>
<li><code>resvg-0.*.0.tar.xz</code> is a sources archive with vendored Rust dependencies</li>
<li><code>resvg-explorer-extension.exe</code> is an SVG thumbnailer for Windows Explorer</li>
</ul>
<h2>v0.34.0</h2>
<ul>
<li><code>viewsvg</code> is a simple application that showcases <em>resvg</em> capabilities</li>
<li><code>resvg-0.*.0.tar.xz</code> is a sources archive with vendored Rust dependencies</li>
<li><code>resvg-explorer-extension.exe</code> is an SVG thumbnailer for Windows Explorer</li>
</ul>
<h2>v0.33.0</h2>
<ul>
<li><code>viewsvg</code> is a simple application that showcases <em>resvg</em> capabilities</li>
<li><code>resvg-0.*.0.tar.xz</code> is a sources archive with vendored Rust dependencies</li>
<li><code>resvg-explorer-extension.exe</code> is an SVG thumbnailer for Windows Explorer</li>
</ul>
<h2>v0.32.0</h2>
<ul>
<li><code>viewsvg</code> is a simple application that showcases <em>resvg</em> capabilities</li>
<li><code>resvg-0.*.0.tar.xz</code> is a sources archive with vendored Rust dependencies</li>
<li><code>resvg-explorer-extension.exe</code> is an SVG thumbnailer for Windows Explorer</li>
</ul>
<!-- raw HTML omitted -->
</blockquote>
<p>... (truncated)</p>
</details>
<details>
<summary>Changelog</summary>
<p><em>Sourced from <a href="https://github.com/RazrFalcon/resvg/blob/master/CHANGELOG.md">resvg's changelog</a>.</em></p>
<blockquote>
<h2>[0.40.0] - 2024-02-17</h2>
<h3>Added</h3>
<ul>
<li><code>usvg::Tree</code> is <code>Send + Sync</code> compatible now.</li>
<li><code>usvg::WriteOptions::preserve_text</code> to control how <code>usvg</code> generates an SVG.</li>
<li><code>usvg::Image::abs_bounding_box</code></li>
</ul>
<h3>Changed</h3>
<ul>
<li>All types in <code>usvg</code> are immutable now. Meaning that <code>usvg::Tree</code> cannot be modified
after creation anymore.</li>
<li>All struct fields in <code>usvg</code> are private now. Use getters instead.</li>
<li>All <code>usvg::Tree</code> parsing methods require the <code>fontdb</code> argument now.</li>
<li>All <code>defs</code> children like gradients, patterns, clipPaths, masks and filters are guarantee
to have a unique, non-empty ID.</li>
<li>All <code>defs</code> children like gradients, patterns, clipPaths, masks and filters are guarantee
to have <code>userSpaceOnUse</code> units now. No <code>objectBoundingBox</code> units anymore.</li>
<li><code>usvg::Mask</code> is allowed to have no children now.</li>
<li>Text nodes will not be parsed when the <code>text</code> build feature isn't enabled.</li>
<li><code>usvg::Tree::clip_paths</code>, <code>usvg::Tree::masks</code>, <code>usvg::Tree::filters</code> returns
a pre-collected slice of unique nodes now.
It's no longer a closure and you do not have to deduplicate nodes by yourself.</li>
<li><code>usvg::filter::Primitive::x</code>, <code>y</code>, <code>width</code> and <code>height</code> methods were replaced
with <code>usvg::filter::Primitive::rect</code>.</li>
<li>Split <code>usvg::Tree::paint_servers</code> into <code>usvg::Tree::linear_gradients</code>,
<code>usvg::Tree::radial_gradients</code>, <code>usvg::Tree::patterns</code>.
All three returns pre-collected slices now.</li>
<li>A <code>usvg::Path</code> no longer can have an invalid bbox. Paths with an invalid bbox will be
rejected during parsing.</li>
<li>All <code>usvg</code> methods that return bounding boxes return non-optional <code>Rect</code> now.
No <code>NonZeroRect</code> as well.</li>
<li><code>usvg::Text::flattened</code> returns <code>&amp;Group</code> and not <code>Option&lt;&amp;Group&gt;</code> now.</li>
<li><code>usvg::ImageHrefDataResolverFn</code> and <code>usvg::ImageHrefStringResolverFn</code>
require <code>fontdb::Database</code> argument.</li>
<li>All shared nodes are stored in <code>Arc</code> and not <code>Rc&lt;RefCell&gt;</code> now.</li>
<li><code>resvg::render_node</code> now includes filters bounding box. Meaning that a node with a blur filter
no longer be clipped.</li>
<li>Replace <code>usvg::utils::view_box_to_transform</code> with <code>usvg::ViewBox::to_transform</code>.</li>
<li>Rename <code>usvg::XmlOptions</code> into <code>usvg::WriteOptions</code> and embed <code>xmlwriter::Options</code>.</li>
</ul>
<h3>Removed</h3>
<ul>
<li><code>usvg::Tree::postprocess()</code> and <code>usvg::PostProcessingSteps</code>. No longer needed.</li>
<li><code>usvg::ClipPath::units()</code>, <code>usvg::Mask::units()</code>, <code>usvg::Mask::content_units()</code>,
<code>usvg::Filter::units()</code>, <code>usvg::Filter::content_units()</code>, <code>usvg::LinearGradient::units()</code>,
<code>usvg::RadialGradient::units()</code>, <code>usvg::Pattern::units()</code>, <code>usvg::Pattern::content_units()</code>
and <code>usvg::Paint::units()</code>. They are always <code>userSpaceOnUse</code> now.</li>
<li><code>usvg::Units</code>. No longer needed.</li>
</ul>
<h3>Fixed</h3>
<ul>
<li>Text bounding box is accounted during SVG size resolving.
Previously, only paths and images were included.</li>
<li>Font selection when an italic font isn't explicitly marked as one.</li>
</ul>
<!-- raw HTML omitted -->
</blockquote>
<p>... (truncated)</p>
</details>
<details>
<summary>Commits</summary>
<ul>
<li><a href="https://github.com/RazrFalcon/resvg/commit/6b973b2eb02229d49c22f0e26fa0f50d76f96d89"><code>6b973b2</code></a> Version bump.</li>
<li><a href="https://github.com/RazrFalcon/resvg/commit/b50e3c67849e31e77c9760380de5eb9325c80915"><code>b50e3c6</code></a> Rename <code>usvg::XmlOptions</code> into <code>usvg::WriteOptions</code>.</li>
<li><a href="https://github.com/RazrFalcon/resvg/commit/db4904d745520caf1f7d014bc89a45bf6b0e2da4"><code>db4904d</code></a> Update changelog.</li>
<li><a href="https://github.com/RazrFalcon/resvg/commit/17708a90814dda044c82e2ac824f714a8aac844f"><code>17708a9</code></a> Update usvg readme.</li>
<li><a href="https://github.com/RazrFalcon/resvg/commit/20c8dcc0d9b1c006f3964a995bbcf68c3f3da41d"><code>20c8dcc</code></a> Fix build.</li>
<li><a href="https://github.com/RazrFalcon/resvg/commit/cadb1728f8af4309bd5173115ea7b5abf64ecd49"><code>cadb172</code></a> Replace <code>usvg::utils::view_box_to_transform</code> with <code>usvg::ViewBox::to_transform</code>.</li>
<li><a href="https://github.com/RazrFalcon/resvg/commit/782948f2e9b90698fc0d729a8a42e9282deb3d24"><code>782948f</code></a> Try to preserve original gradient and pattern objects when converting units.</li>
<li><a href="https://github.com/RazrFalcon/resvg/commit/e042024d3eac056372cad3b21eea9eb610d009f9"><code>e042024</code></a> Gradients and patterns are always in <code>userSpaceOnUse</code> units now.</li>
<li><a href="https://github.com/RazrFalcon/resvg/commit/6af096e03bea37d121fc3e6fbfa003dc8fa31740"><code>6af096e</code></a> Use layer bbox during individual nodes rendering.</li>
<li><a href="https://github.com/RazrFalcon/resvg/commit/196156424947e8777e5001da5c8c2da2d306a292"><code>1961564</code></a> Fix defs id generation.</li>
<li>Additional commits viewable in <a href="https://github.com/RazrFalcon/resvg/compare/v0.29.0...v0.40.0">compare view</a></li>
</ul>
</details>
<br />


[![Dependabot compatibility score](https://dependabot-badges.githubapp.com/badges/compatibility_score?dependency-name=resvg&package-manager=cargo&previous-version=0.29.0&new-version=0.40.0)](https://docs.github.com/en/github/managing-security-vulnerabilities/about-dependabot-security-updates#about-compatibility-scores)

Dependabot will resolve any conflicts with this PR as long as you don't alter it yourself. You can also trigger a rebase manually by commenting `@dependabot rebase`.

[//]: # (dependabot-automerge-start)
[//]: # (dependabot-automerge-end)

---

<details>
<summary>Dependabot commands and options</summary>
<br />

You can trigger Dependabot actions by commenting on this PR:
- `@dependabot rebase` will rebase this PR
- `@dependabot recreate` will recreate this PR, overwriting any edits that have been made to it
- `@dependabot merge` will merge this PR after your CI passes on it
- `@dependabot squash and merge` will squash and merge this PR after your CI passes on it
- `@dependabot cancel merge` will cancel a previously requested merge and block automerging
- `@dependabot reopen` will reopen this PR if it is closed
- `@dependabot close` will close this PR and stop Dependabot recreating it. You can achieve the same result by closing it manually
- `@dependabot show <dependency name> ignore conditions` will show all of the ignore conditions of the specified dependency
- `@dependabot ignore this major version` will close this PR and stop Dependabot creating any more for this major version (unless you reopen the PR or upgrade to it yourself)
- `@dependabot ignore this minor version` will close this PR and stop Dependabot creating any more for this minor version (unless you reopen the PR or upgrade to it yourself)
- `@dependabot ignore this dependency` will close this PR and stop Dependabot creating any more for this dependency (unless you reopen the PR or upgrade to it yourself)


</details>

---

_Label `internal` added by @dependabot[bot] on 2024-03-11 21:35_

---

_Comment by @charliermarsh on 2024-03-11 21:36_

@dependabot ignore this dependency

---

_Closed by @dependabot[bot] on 2024-03-11 21:36_

---

_Comment by @dependabot[bot] on 2024-03-11 21:36_

OK, I won't notify you about resvg again, unless you re-open this PR.

---

_Branch deleted on 2024-03-11 21:36_

---

_Comment by @zanieb on 2024-03-11 21:39_

Can you share why? for posterity

---

_Comment by @charliermarsh on 2024-03-11 21:46_

Yes! We use this exclusively for generating SVGs in the benchmark document (`BENCHMARKS.md`). CI seemed to fail and IMO not worth investing in keeping this dep up-to-date since it's used one-off in an offline dev workflow.

---
