# Playbook-Based E2E Testing with Playwright CLI and AI Agents

Slides from the talk, published as a static deck viewer.

**▶ [View the slides](https://avindrafernando.github.io/playbook-e2e-testing-playwright-cli/)**

This is the 29-slide version of the talk. The earlier 50-slide version lives in
[playbook-based-e2e-testing-with-playwright-cli-and-ai-agents](https://github.com/avindrafernando/playbook-based-e2e-testing-with-playwright-cli-and-ai-agents).

## Controls

| Key | Action |
| --- | --- |
| <kbd>→</kbd> <kbd>Space</kbd> <kbd>PgDn</kbd> <kbd>J</kbd> | Next slide |
| <kbd>←</kbd> <kbd>PgUp</kbd> <kbd>K</kbd> | Previous slide |
| <kbd>Home</kbd> / <kbd>End</kbd> | First / last slide |
| digits then <kbd>Enter</kbd> | Jump to a slide number |
| <kbd>G</kbd> | Thumbnail overview of every slide |
| <kbd>F</kbd> | Fullscreen |
| <kbd>?</kbd> | Shortcut help |
| <kbd>Esc</kbd> | Close overlay |

On touch devices, swipe left/right or tap the left/right edge of the slide. Every
slide is deep-linkable — `#12` opens slide 12.

## Layout

```
index.html   the whole viewer: markup, styles, script, slide manifest
build.py     regenerates the manifest inside index.html from slides/
embeds.json  optional: live video / website embeds and links for specific slides
slides/      one PNG per slide, named <number>_<Title-with-dashes>.png
```

There are no dependencies and no build step for deployment — GitHub Pages serves
`index.html` straight from the repo root.

## Working on it locally

```bash
python3 -m http.server 4327
```

Then open <http://localhost:4327>. A plain `file://` open works too, though some
browsers restrict local image loading.

## Adding or replacing slides

Drop the PNGs into `slides/` using the `<number>_<Title-with-dashes>.png`
convention — the number sets the order, the rest becomes the slide title shown in
the overview — then run:

```bash
python3 build.py
```

Gaps in the numbering are fine; slides are sorted by the numeric prefix, not by
being consecutive.

## Video and live-site slides

Slide exports can't capture embedded content, so a slide that held a video or a
website comes out as an empty frame or a bare link card. `embeds.json`
puts the real thing back:

```json
{
  "STORY-004-Playwright-CLI-Demo": {
    "title": "STORY-004 Playwright CLI demo (video)",
    "embed": "https://www.youtube-nocookie.com/embed/gP1szvJeo8g",
    "link": "https://www.youtube.com/watch?v=gP1szvJeo8g"
  }
}
```

- The key is the slide's filename without the number prefix or `.png`, so the
  entry still matches after slides are renumbered or re-exported.
- `embed` shows that URL in an iframe in place of the PNG. Use
  `youtube-nocookie.com/embed/<id>` for YouTube.
- `link` adds an open-in-new-tab button to the toolbar on that slide.
- `title` overrides the label in the overview.

Run `python3 build.py` after editing it. It warns about entries that no longer
match a slide.
