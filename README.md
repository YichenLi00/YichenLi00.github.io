# yichenli00.github.io

Personal homepage. Jekyll, built and served by GitHub Pages from `main`.

## Editing content

All content lives in `_data/` — you should not need to touch templates to add a
paper, a news item, or a job.

| File | What it drives |
|---|---|
| `_data/publications.yml` | the Publications page |
| `_data/news.yml` | the News section |
| `_data/vitae.yml` | the Vitæ timeline |
| `_data/honors.yml` | Honors & Awards |
| `_data/services.yml` | Services |
| `_data/navigation.yml` | the top nav |

Each file starts with a comment explaining its fields. Two things worth knowing:

- **Publications need a `year`** — it drives the grouping. Seven Journal-First
  entries are currently marked `# TODO` because the original prose carried no
  year; they group under "Journal-First" until filled in.
- **`selected: true`** puts a paper in the default view. The rest appear when the
  reader switches to "All".

Author markers are data, not markup: list the name under `equal` or
`corresponding` and the template adds the `*` / `#` superscript and bolds
"Yichen Li" itself.

## Structure

```
_data/            content
_layouts/base.html   the only layout
_includes/        head, nav, footer, icon
assets/css/main.scss single stylesheet
_pages/           three pages: about, publications, contact
```

`_includes/icon.html` holds the hand-drawn Simpsons icons. Two constraints there,
both learned the hard way:

- **Quote every SVG attribute.** `fill=#FEBE34` fails to parse and the shape
  collapses to a flat coloured blob.
- **The size parameter is `px`, not `size`.** `include.size` resolves to Liquid's
  builtin size property on the parameter hash — i.e. the number of parameters —
  so the icon silently renders 1px wide.

Colours in `main.scss` are sampled per-pixel from the official character art. The
palette is deliberately two-tier: the saturated Simpsons colours all fail WCAG
contrast on white, so they are used only for icon fills and small blocks, never
for text. Darkened same-hue variants carry the text.

## Local preview

The system Ruby (2.6) cannot resolve the dependency set in the committed
`Gemfile.lock`, so a local build needs its own lock kept outside the repo —
otherwise `bundle install` rewrites the committed one with downgraded versions.

```sh
gem install --user-install bundler -v 2.3.26
export PATH="$HOME/.gem/ruby/2.6.0/bin:$PATH"
mkdir -p /tmp/jekyll-local && cp Gemfile /tmp/jekyll-local/
export BUNDLE_GEMFILE=/tmp/jekyll-local/Gemfile
bundle _2.3.26_ install --path vendor/bundle
bundle _2.3.26_ exec jekyll build --source . --destination /tmp/jbuild
```

Then serve the output over HTTP rather than opening `file://` — asset paths are
root-relative, so `file://` resolves them against the filesystem root and every
stylesheet 404s:

```sh
cd /tmp/jbuild && python3 -m http.server 8899
```
