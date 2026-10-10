# procrastination-tools

Two small browser tools, each one the result of procrastinating on the work
it was built to help with.

Each tool is one HTML file. The file holds the markup, the styles and the
script. There is no build step, no install and no package manager. Open the
file in a browser and the tool runs.

## The tools

| Tool | What it does | Open |
|---|---|---|
| [annotate.html](annotate.html) | Draws markers, boxes, circles and arrows over a screenshot and exports an editable vector overlay. | [Run](https://USERNAME.github.io/procrastination-tools/annotate.html) |
| [gantt-a4.html](gantt-a4.html) | Turns plain text into a Gantt chart that prints on one A4 sheet. | [Run](https://USERNAME.github.io/procrastination-tools/gantt-a4.html) |

### annotate

Documentation screenshots always need one more callout, and a flattened PNG
means redrawing the whole thing. This one keeps the marks as vectors over the
image, so they can be moved later.

Drop in a screenshot, place numbered or lettered markers, boxes, circles,
lines and arrows, then take one of two exports. The **SVG** download holds the
screenshot and the marks in one file that you reference like any other image.
The **snippet** is an HTML fragment that keeps the screenshot as a separate
file and overlays the marks on top of it. Drop an exported SVG back onto the
canvas to edit its marks again.

Marks are sized relative to the image width, so they come out the same
on-screen size on every screenshot once the page scales the image to its text
column. A **Normalize** panel rescales files you exported earlier to match.

### gantt-a4

GanttProject was unpleasant to work in, and spreadsheet PDF export does not
land reliably on a known page size. This one does a single thing: a chart that
fits an A4 sheet and prints the same way every time.

The plan is plain text, one row per line:

```
%title Example plan

# Planning
Compare sensor datasheets  | 2026-09-21 | 2w | 80% | @parts
! Parts ordered            | 2026-10-09 | >parts
~ A description, attached to the row above it.
```

A task takes a start date and either an end date or a duration (`3w`, `10d`,
`2m`). After the dates, in any order: `60%` progress, `@id` to name a row,
`>id` to depend on another, `#rrggbb` to recolour it. `#` starts a phase, `!`
a milestone, `~` a description, `//` a comment.

You can also drag bars and edges in the chart, rename rows in place, and
right-click for a context menu. Those edits are written back into the text.
Output is a PDF through the browser's own print dialog. On Chromium you can link a
`.gantt` file on disk and edit it in your own editor, with changes flowing
both ways.

The tool starts with an example plan. The Reset panel reloads it.

## Run a tool

The links above open the current version. Nothing is installed.

To keep your own copy:

1. Open `annotate.html` or `gantt-a4.html` in this repository.
2. Press **Download raw file**.
3. Open the downloaded file in a browser.

The copy runs offline. Neither tool makes any network request, loads any font
or library from a CDN, or contains third-party code.

## Your data

The tools run entirely in your browser. There is no server to send anything
to, and neither tool makes a network request of any kind.

Your work is kept in the browser's local storage under these keys:

| Tool | Keys |
|---|---|
| annotate | `ptools.annotate.img`, `ptools.annotate.state`, `ptools.annotate.layout` |
| gantt-a4 | `ptools.gantt-a4.state`, and an IndexedDB database `ptools.gantt-a4.link` for the linked-file handle |
| all three pages | `ptools.theme`, the light or dark choice |

That storage belongs to one browser on one device. A private window, a cleared
cache or a different machine all start empty. Export anything you want to
keep: **Download SVG** in annotate, **Save .gantt** in gantt-a4.

One quirk worth knowing: annotate stores the screenshot itself, so a large
image can exceed the browser's storage quota. It warns you once when that
happens and then stops saving that image.

## Limits

Both tools were only ever tested in Chromium on Linux. They are not tested in
Firefox, Safari or on Windows, macOS or any phone. They probably work; nobody
has checked.

**annotate**

- Needs a reasonably current browser. Pointer events, CSS custom properties
  and the `inset` shorthand are all assumed.
- Markers and shapes only. There is no free-text annotation tool.
- **Normalize** only accepts SVGs this tool made; it reads a size attribute it
  writes itself.
- A letter marker's upper or lower case is restored per file, not per marker,
  when you re-import.

**gantt-a4**

- **Ctrl+Z in the source box is unreliable.** Dragging a bar rewrites the text
  programmatically, which desynchronises the browser's native undo. There is no
  undo of its own. Save often.
- Dependency arrows are drawn only when both ends land on the same printed
  page. An arrow across a page break is silently dropped.
- Dates are literal. There is no scheduling: moving a task never moves its
  dependents, nothing is validated against anything, and a dependency cycle
  draws arrows in both directions without complaint.
- Duplicating a phase moves its tasks under the copy.
- About 32 rows fit a landscape page at the default row height, 50 in
  portrait. More rows spill onto further pages, with the header repeated.
- Past roughly 44 weeks the columns get too narrow to read; the tool says so
  and suggests a coarser time unit.
- Print with margins set to **None** and scale **100%**, or the millimetre
  layout stops being millimetres.
- On US Letter, Chromium scales the whole A4 layout down by about 6% rather
  than clipping it. Nothing is lost, but the page is no longer the size it
  claims. There is no Letter layout.
- Linking a file on disk needs the File System Access API, so Chromium only.
  Firefox and Safari get the Save and Open buttons instead.

## Written with AI

I did not write the code. An AI assistant (Claude) wrote all of it. My part
was describing what each tool should do, using what came back, saying what was
wrong, and repeating that until the tools were useful.

That means nobody has reviewed this code line by line. It comes with no
warranty. Read the source before you trust a tool with something that matters:
each one is a single unminified file, so that is a realistic thing to do.

Report anything that looks wrong. I will fix it or say that I will not.

## License

MIT. See [LICENSE](LICENSE).

## Contributions

Issues are welcome, in particular a bug with the steps to reproduce it.

I fix what gets in my way, so a pull request may wait a long time. If you want
a tool to do something different, forking it is one file and no build step.
