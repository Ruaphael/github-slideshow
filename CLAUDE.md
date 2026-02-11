# CLAUDE.md

## Project Overview

This is **github-slideshow**, a GitHub Learning Lab course repository that teaches Git and GitHub through an interactive slide-based presentation. It uses **Jekyll** (Ruby static site generator) with **Reveal.js 3.9.2** (presentation framework) to render Markdown slides as a browser-based slideshow hosted on GitHub Pages.

## Repository Structure

```
github-slideshow/
├── _config.yml              # Jekyll + Reveal.js configuration
├── _includes/               # Jekyll HTML partials
│   ├── head.html            # Document head (CSS/JS refs, moon theme)
│   ├── script.html          # Reveal.js initialization + plugin loading
│   └── slide.html           # Individual slide section template
├── _layouts/                # Jekyll layout templates
│   ├── presentation.html    # Main presentation wrapper
│   ├── print.html           # PDF export layout
│   └── slide.html           # Single slide layout
├── _posts/                  # Slide content (Markdown files)
│   ├── 0000-01-01-intro.md  # Introduction slide
│   └── 0000-01-02-*.md      # Additional slides
├── script/                  # Shell scripts for build/deploy
│   ├── cibuild              # CI build: jekyll build + htmlproofer
│   ├── server               # Local dev server: jekyll serve
│   ├── setup                # Environment setup (macOS/Ruby)
│   └── stage                # Staging deployment script
├── node_modules/            # npm packages (reveal.js)
├── Gemfile                  # Ruby dependencies
├── Gemfile.lock             # Locked gem versions
├── index.html               # Jekyll entry point (iterates _posts)
├── .editorconfig            # Editor formatting rules
└── .gitignore               # Ignores _site/, .sass-cache/, etc.
```

## Tech Stack

- **Static Site Generator:** Jekyll 3.9.x (via `github-pages` gem >= 207)
- **Presentation Framework:** Reveal.js 3.9.2 (installed via npm)
- **Markdown Processor:** kramdown
- **Syntax Highlighting:** rouge (Ruby) + highlight.js (Reveal.js)
- **Theme:** Reveal.js "moon" theme, solarized dark variant
- **HTML Validation:** html-proofer >= 3.13.0
- **Emoji Support:** jemoji plugin

## Development Commands

### Local development server
```sh
script/server
# or directly:
bundle exec jekyll serve
```

### CI build (build + validate HTML)
```sh
script/cibuild
# Runs: bundle exec jekyll build --baseurl "."
# Then: htmlproofer _site/index.html --empty-alt-ignore
```

### Initial setup
```sh
script/setup
# Installs Ruby gems via Bundler, initializes git submodules
```

## Adding Slides

Slides are Markdown files in `_posts/`. Each file follows the naming convention:

```
0000-01-NN-<slide-name>.md
```

The numeric prefix controls slide ordering. Each slide file needs Jekyll front matter:

```yaml
---
layout: slide
title: ""
---

Your slide content in Markdown here.
```

## Code Style Conventions

Defined in `.editorconfig`:

| File Type | Indent Style | Indent Size |
|-----------|-------------|-------------|
| Default (all files) | Tabs | 4 |
| JSON, JS, CSS, SCSS, YML, HTML | Spaces | 2 |
| Markdown | Spaces | 4 |

- **Line endings:** LF (`\n`)
- **Charset:** UTF-8
- **Trailing whitespace:** trimmed (except Markdown)
- **Final newline:** inserted for Markdown files only

## Configuration

All presentation and Jekyll settings live in `_config.yml`. Key settings:

- **Slide dimensions:** 1000x920 px
- **Transition:** linear (with slide background transitions)
- **Slide numbering:** flattened/total (`c/t` format)
- **Controls overlay:** disabled
- **Progress bar:** enabled
- **Keyboard navigation:** enabled
- **Browser history:** enabled (each slide updates URL)

## Git Workflow

- **Main branch:** `master`
- **Feature branches:** named descriptively (e.g., `my-slide`)
- **Merge strategy:** Pull Requests with merge commits
- **Dependency updates:** automated via Dependabot/Snyk

## Build Output

Jekyll builds to `_site/` (gitignored). The built site is a self-contained static HTML presentation that can be served by any web server or GitHub Pages.

## Key Gotchas

- The `baseurl` in `_config.yml` is commented out. The CI build overrides it with `--baseurl "."` for relative paths.
- Reveal.js is installed as an npm module under `node_modules/`, not as a git submodule (migrated from submodule in project history).
- The `_posts/` date prefix (`0000-01-XX`) is a Jekyll convention used here purely for ordering, not for actual dates.
- The `exclude` list in `_config.yml` prevents Reveal.js source files from being included in the Jekyll build output.
