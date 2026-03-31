# GESIS PSRT Conference Program Template

A simple Quarto (Quarto > pandoc > LaTeX) template for generating a conference program PDF from two CSV files. Produces a one-page schedule overview followed by abstracts grouped by panel. Originally designed for the GESIS Political Science Roundtable.

## Usage

```bash
quarto render program.qmd
```

The output is `program.pdf` ([preview](program.pdf))

## Input files

### `data/schedule.csv`

The conference schedule. Each row is one schedule item.

| Column | Description | Values |
|--------|-------------|--------|
| `time` | Start time | e.g. `09:00` |
| `title` | Item title | For panels, use the format `Panel: Panel Name` (prefix is configurable via `panel_prefix`) |
| `type` | Item type | `panel`, `break`, or `event` |
| `panel_id` | (panels only) Unique ID for this panel | e.g. `dem_back`, `comp_methods`. Used to link papers to panels. Leave blank for non-panel rows. |
| `date` | (optional) Date | Only needed for multi-day conferences. If present with more than one unique value, date headers are shown in the schedule and panel headings include the date. |

Papers are linked to panels via `panel_id`. The display title comes from the schedule's `title` column, so you can rename a panel without touching `papers.csv`.

### `data/papers.csv`

All papers/presentations. Row order within a panel determines presentation order.

| Column | Description |
|--------|-------------|
| `panel_id` | Panel ID, must match a `panel_id` in `schedule.csv` |
| `author` | Author or presenter name |
| `title` | Paper title |
| `type` | Paper type (free text, e.g. `paper`, `presentation`, `lightning`) |
| `abstract` | Abstract text |

## Schedule-only / print mode

To render just the schedule page (no abstracts, no hyperlinks, no page number) for printing as a single A4 handout:

```bash
quarto render program.qmd -P schedule_only:true
```

Or set `schedule_only: true` in the YAML frontmatter.

## Configuration

All configuration lives in the YAML frontmatter of `program.qmd`.

### Standard Quarto fields

| Field | Description |
|-------|-------------|
| `title` | Conference title |
| `subtitle` | Subtitle (e.g. "Conference Program") |
| `date` | Conference date |
| `date-format` | Date display format (e.g. `long` for "June 15, 2026") |

### Parameters (`params`)

| Parameter | Default | Description |
|-----------|---------|-------------|
| `schedule_only` | `false` | Print mode: schedule page only, no abstracts, no hyperlinks, no page number |
| `venue` | | Venue name, shown below the title on page 1 |
| `schedule_csv` | `data/schedule.csv` | Path to the schedule CSV |
| `papers_csv` | `data/papers.csv` | Path to the papers CSV |
| `panel_prefix` | `Panel: ` | Prefix stripped from schedule panel titles when matching to `papers.csv` |
| `panel_clearpage` | `true` | Start each panel's abstracts on a new page |
| `panel_show_time` | `true` | Show the time range next to panel headings in the abstracts section |
| `type_icons` | `{paper: "P", presentation: "T", lightning: "L"}` | Mapping of paper types to icon letters shown in the schedule and abstracts. Add or change entries to match the types in your `papers.csv`. |


## Styling

Visual styling is controlled in `_preamble.tex`, including

- **Page geometry**: `papersize` and `geometry` fields in YAML frontmatter
- **Running header text**: `\fancyhead[L]{...}` and `\fancyhead[R]{...}`
- **Link colors**: `\hypersetup{linkcolor=..., urlcolor=...}`
- **Schedule table row spacing**: `\renewcommand{\arraystretch}{1.4}` (set in `program.qmd`)
- **Abstract entry spacing**: adjust values in `\paperentry`, `\paperseparator`, `\panelsection`
- **Type icon style**: `\typeicon` command controls how icon letters are rendered

## Pulling template updates

When you create a repo from this template, GitHub does not maintain a link to the original. To pull in future template updates (e.g. bug fixes or new features), add this repo as a remote:

```bash
# One-time setup
git remote add template https://github.com/ghxm/gesis-psrt-program.git

# Pull updates
git fetch template
git merge template/main --allow-unrelated-histories
```

Your data files (`data/schedule.csv`, `data/papers.csv`) and the YAML frontmatter in `program.qmd` will likely conflict since you've customized them. Resolve by keeping your versions. To automate this for the CSV files, add a `.gitattributes` to your repo:

```
data/schedule.csv merge=ours
data/papers.csv merge=ours
```

This tells git to always keep your version of those files during merges. Changes to `_preamble.tex` and the R code in `program.qmd` should merge cleanly in most cases.
