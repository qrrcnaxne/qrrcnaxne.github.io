# qrrcnaxne.github.io

Source for [qrrcnaxne.github.io](https://qrrcnaxne.github.io/), a personal blog built with [Hugo](https://gohugo.io/) and the [PaperMod](https://github.com/adityatelange/hugo-PaperMod) theme.

## Layout

| Path | Contents |
|---|---|
| `content/tech/` | Technical writing (topic goes in `tags`: rust, security, ...) |
| `content/finance/` | Finance and investing (empty for now) |
| `content/literature/` | Poetry and prose, migrated from the old WordPress blog (`tags = ["poetry"]` or `["prose"]`) |
| `content/pages/` | Static pages (about, privacy, "The First") |
| `layouts/home.html` | Front page: an index of every post, grouped by section (order set by `mainSections` in `hugo.toml`) |
| `themes/PaperMod` | Theme, as a git submodule |

## Local development

The theme is a submodule, so clone with it:

```sh
git clone --recurse-submodules https://github.com/qrrcnaxne/qrrcnaxne.github.io.git
cd qrrcnaxne.github.io
hugo server -D
```

If you already cloned without it: `git submodule update --init`.

New posts:

```sh
hugo new content tech/my-post.md
```

Posts use TOML front matter. The section says what kind of writing it is and `tags` say what it's about. Name files after the slug (no number prefix), set `lastmod` when you revise a post after publishing, and remove `draft = true` to publish.

## Deployment

Pushing to `master` runs `.github/workflows/hugo.yaml`, which builds with a pinned Hugo version (`HUGO_VERSION` in the workflow) and deploys to GitHub Pages.

RSS feeds are disabled (`[outputs]` in `hugo.toml`).
